
## Índice
1. [Visión general](#1-visión-general)
2. [Configuración del entorno](#2-configuración-del-entorno)
3. [Carga y gestión de Tenants](#3-carga-y-gestión-de-tenants)
4. [Securización de peticiones HTTP](#4-securización-de-peticiones-http)
5. [Uso de credenciales en llamadas externas](#5-uso-de-credenciales-en-llamadas-externas)
6. [Flujo completo](#6-flujo-completo)

---

## 1. Visión general

El sistema implementa una arquitectura **multi-tenant** en la que cada tenant tiene su propia instancia de Keycloak y configuración de base de datos. La securización se realiza en tres capas:

```
Petición HTTP
     │
     ▼
KeycloakHTTPBearer   ← Valida el token JWT
     │
     ▼
get_current_tenant   ← Identifica el tenant por el issuer del token
     │
     ▼
Request State        ← Almacena tenant_id y roles
     │
     ▼
Endpoint             ← Usa las credenciales del tenant para llamadas externas
```

---

## 2. Configuración del entorno

Las variables de entorno en `enviroment.env` (`env_file_gis-apirest.env` en los entornos) se dividen en dos grupos:

### 2.1 Configuración de Keycloak (fallback)

Usadas cuando el **Tenant Admin no está disponible**:

```dotenv
AUTH_SERVER_URL=https://auth-eu.go-aigua.com/auth   # URL base de Keycloak
AUTH_CLIENT_ID=go-aigua-soc                          # Client ID de Keycloak
AUTH_REALM=gis_devtools                              # Realm de Keycloak
```

### 2.2 Configuración del Tenant Admin

Servicio centralizado que provee la configuración de todos los tenants:

```dotenv
TENANT_API_URL=https://gateway-proxy-dev.go-aigua.com/tenant-admin-service
TENANT_API_KEY=n0zVMVP648fMeWBA1bcg
```

### 2.3 Configuración del GaPortal

API key usada para obtener credenciales de servicios externos por tenant:

```dotenv
GA_PORTAL_API_KEY=n0zVMVP648fMeWBA1bcg
```

---

## 3. Carga y gestión de Tenants

### 3.1 Obtención de tenants (`get_tenants`)

Al arrancar la aplicación (o cuando se forza recarga), se llama al Tenant Admin:

```
GET {TENANT_API_URL}/tenants/service/{PROJECT_NAME}/extended
Authorization: {TENANT_API_KEY}
```

**Flujo de decisión:**

```
¿Existen TENANTS en memoria?
        │
       SÍ ──────────────────────────────────► Devuelve caché (Singleton)
        │
       NO
        │
¿Hay TENANT_API_KEY y TENANT_API_URL configurados?
        │
       NO ──────────────────────────────────► get_fallback_tenant()
        │
       SÍ
        │
¿Respuesta 200 del Tenant Admin?
        │
       NO ──────────────────────────────────► get_fallback_tenant()
        │
       SÍ
        │
       ▼
  Tenant.from_json() por cada tenant
```

### 3.2 Construcción de un Tenant (`Tenant.from_json`)

Por cada tenant recibido del Tenant Admin, se construye una instancia de `Tenant` con su propia instancia `KeycloakOpenID`:

```python
# Se extrae la URL base y el realm del issuer
# Ejemplo: issuer = "https://auth-eu.go-aigua.com/auth/realms/gis_devtools"
#          server_url = "https://auth-eu.go-aigua.com/auth/"
#          realm_name = "gis_devtools"

keycloak_instance = KeycloakOpenID(
    server_url=server_url,
    client_id=client_id,   # KEYCLOAK_CLIENT_ID del entorno
    realm_name=realm_name,
)
```

También se calcula el **suffix** del schema de base de datos:

| `tenant_id` | `suffix` |
|---|---|
| `1` o `18031919` | `""` (sin sufijo, tenant por defecto) |
| Cualquier otro | `"_t{tenantId}"` |

### 3.3 Tenant de fallback (`get_fallback_tenant`)

Si el Tenant Admin no está disponible, se construye un tenant desde las variables de entorno locales:

```python
Tenant(
    tenant_id=18031919,    # ID reservado para fallback
    issuer=f"{AUTH_SERVER_URL}/realms/{AUTH_REALM}",
    host=HOST_POSTGRES,
    port=PORT_POSTGRES,
    username=USER_POSTGRES,
    password=PASSWORD_POSTGRES,
    api_key=GA_PORTAL_API_KEY,
    suffix="",             # Sin sufijo → schema por defecto
    ...
)
```

---

## 4. Securización de peticiones HTTP

### 4.1 URLs permitidas sin token

Definidas en la variable de entorno `ALLOWED_URLS` (por defecto):

```dotenv
/docs, /redoc, /openapi.json, /actuator/health, /actuator/prometheus
```

Si la URL de la petición contiene alguna de estas rutas, se devuelve un token ficticio y **no se valida nada**.

### 4.2 Validación del token JWT (`KeycloakHTTPBearer`)

Para el resto de rutas, el flujo es:

```
1. Extrae el header "Authorization: Bearer <token>"
        │
        ▼
2. get_current_tenant(token)
   → Itera sobre TODOS los tenants registrados
   → Intenta decodificar el token con la clave pública de cada Keycloak
   → El primer Keycloak que lo valide correctamente identifica el tenant
        │
        ▼
3. Almacena en request.state:
   - request.state.tenant_id  → ID del tenant identificado
   - request.state.roles      → Roles del usuario en ese client
        │
        ▼
4. Verifica el ACCESS_ROLE (si ENABLE_SECURITY=True)
   → Si el usuario no tiene el rol de acceso → HTTP 401
```

> **Excepción para realm `gis_devtools`/`dev-gis`**: No se verifica el `ACCESS_ROLE`, permitiendo acceso sin ese rol específico.

### 4.3 Filtrado adicional por rol (`role_filter`)

Decorador para endpoints que requieren roles específicos:

```python
@role_filter(roles=["ADMIN", "EDITOR"])
async def my_endpoint(request: Request):
    ...
```

Si `ENABLE_SECURITY=True`, verifica que **todos** los roles indicados estén en `request.state.roles`.

---

## 5. Uso de credenciales en llamadas externas

Una vez identificado el tenant, sus credenciales se usan para llamar a servicios externos a través del **GaPortal Service**.

### 5.1 Estructura de las llamadas (`gaportal_service_requests.py`)

Cada llamada usa la `api_key` del tenant como Bearer token:

```python
headers = {
    'Content-Type': 'application/x-www-form-urlencoded',
    'Authorization': f"Bearer {tenant_api_key}",   # ← api_key del Tenant
}
```

### 5.2 Servicios disponibles

| Función | Variable de entorno | Descripción |
|---|---|---|
| `gaportal_service_iot_credentials_call` | `IOT_GAPORTAL_SERVICE_URL` | Credenciales IoT |
| `gaportal_service_geoserver_credentials_call` | `GEOSERVER_GAPORTAL_SERVICE_URL` | Credenciales GeoServer |
| `gaportal_service_dmd_credentials_call` | `DMD_GAPORTAL_SERVICE_URL` | Credenciales DMD |
| `gaportal_service_credentials_call` | Derivada de `DMD_GAPORTAL_SERVICE_URL` | Cualquier servicio genérico |

### 5.3 Particularidad del servicio IoT

La URL devuelta por el GaPortal para IoT puede incluir el protocolo `https://`. En ese caso, se elimina automáticamente:

```python
if "https" in response_json.get("url"):
    fixed_url = response_json.get("url").split("//")[1]
    response_json["url"] = fixed_url
```

---

## 6. Flujo completo

```
┌─────────────────────────────────────────────────────────────────┐
│                        ARRANQUE                                  │
│  get_tenants() → Tenant Admin API → Lista de Tenant[]           │
│  Cada Tenant tiene su KeycloakOpenID configurado                │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    PETICIÓN ENTRANTE                             │
│                                                                  │
│  1. KeycloakHTTPBearer.__call__()                               │
│     └─ ¿URL permitida? → Devuelve dummy token                   │
│     └─ retrieve_decoded_token()                                  │
│        └─ Extrae Bearer token                                    │
│        └─ get_current_tenant(token)                              │
│           └─ Prueba cada Keycloak hasta encontrar el válido      │
│        └─ Almacena tenant_id y roles en request.state           │
│        └─ Verifica ACCESS_ROLE                                   │
│                                                                  │
│  2. Endpoint recibe request con tenant_id identificado           │
│     └─ get_tenant(request.state.tenant_id)                      │
│        └─ Obtiene config de BD, api_key, etc.                   │
│                                                                  │
│  3. Para llamadas externas:                                      │
│     └─ gaportal_service_*_call(tenant.api_key)                  │
│        └─ Retorna credenciales específicas del tenant            │
└─────────────────────────────────────────────────────────────────┘
```