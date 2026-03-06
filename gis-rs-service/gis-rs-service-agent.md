# 🗺️ SERVICE MAP — gis-rs-service

> Documento de referencia para agentes IA y desarrolladores.
> Cubre arquitectura, todos los módulos, endpoints, flujos de datos, tablas BD, variables de entorno y patrones de error más comunes.
> Versión del servicio: `1.5.19-SNAPSHOT` | Puerto por defecto: `1919`

---

## 📋 ÍNDICE

1. [Visión General](#1-visión-general)
2. [Estructura de Carpetas](#2-estructura-de-carpetas)
3. [Arranque y Ciclo de Vida](#3-arranque-y-ciclo-de-vida)
4. [Autenticación y Multi-Tenancy](#4-autenticación-y-multi-tenancy)
5. [Scheduler Diario](#5-scheduler-diario)
6. [Módulo Agriculture (Sentinel-2)](#6-módulo-agriculture-sentinel-2)
7. [Módulo Ground Movement (Sentinel-1 / InSAR)](#7-módulo-ground-movement-sentinel-1--insar)
8. [Módulo EarthPulse (Water Leak Detection)](#8-módulo-earthpulse-water-leak-detection)
9. [Módulo Evapotranspiración](#9-módulo-evapotranspiración)
10. [Módulo Service Diagnostic](#10-módulo-service-diagnostic)
11. [Capa de Infraestructura](#11-capa-de-infraestructura)
12. [Tablas de Base de Datos](#12-tablas-de-base-de-datos)
13. [Variables de Entorno Críticas](#13-variables-de-entorno-críticas)
14. [Mapa Completo de Endpoints](#14-mapa-completo-de-endpoints)
15. [Flujos End-to-End](#15-flujos-end-to-end)
16. [Patrones de Error Comunes y Diagnóstico](#16-patrones-de-error-comunes-y-diagnóstico)
17. [Dependencias Externas](#17-dependencias-externas)
18. [Cómo Añadir un Nuevo Cliente](#18-cómo-añadir-un-nuevo-cliente)

---

## 1. Visión General

`gis-rs-service` es un microservicio FastAPI de **teledetección (Remote Sensing)** que expone procesamiento satelital para:

| Producto | Satélite | Tecnología clave |
|---|---|---|
| **Agriculture** | Sentinel-2 (S2) | NDVI / NDWI / NDMI + Azure Blob |
| **Ground Movement** | Sentinel-1 (S1) | InSAR + MintPy + HyP3/ASF |
| **Water Leak / EarthPulse** | Sentinel-1 (S1) | Detección de humedad de suelo vía API EarthPulse + MinIO |
| **Evapotranspiración** | Sentinel-2 | pyet (ET Ref) |

Todos los productos comparten:
- Base de datos **PostgreSQL + PostGIS** con esquema configurable (`RS_SCHEMA_POSTGRES`).
- Gestión multi-tenant: cada tenant tiene su propia conexión BD.
- Almacenamiento de imágenes: **Azure Blob Storage** (S2) y **MinIO** (S1 / EarthPulse).

---

## 2. Estructura de Carpetas

```
src/
├── main.py                          # Punto de entrada FastAPI
├── application/
│   ├── remote_sensing_tools/
│   │   ├── pgadmin_functions.py     # ⭐ Funciones SQL globales (LEER SIEMPRE)
│   │   ├── rs_common_functions.py   # Funciones comunes: Azure, MinIO, EPSG
│   │   ├── automatic_image_download.py  # Orquestador batch de todos los clientes
│   │   ├── correct_bbd.py           # Corrección de BD Sentinel-2
│   │   ├── agriculture/             # Producto S2
│   │   │   ├── rs_rest_0/           # Descarga S2 (Copernicus/SciHub)
│   │   │   ├── rs_rest_1/           # Consulta procesos/imágenes cliente
│   │   │   ├── rs_rest_2/           # Obtener imagen (Azure)
│   │   │   ├── rs_rest_3/           # Valores históricos por parcela
│   │   │   ├── rs_rest_4/           # Geometrías del cliente
│   │   │   └── rs_rest_new_client/  # Alta nuevo cliente S2
│   │   ├── EarthPulse/              # Producto Water Leak
│   │   │   ├── common_utils.py      # get_client_id_from_project()
│   │   │   ├── create_tables.py     # Creación tablas EarthPulse
│   │   │   ├── analytics/           # Network Health WL + Leak Detection Results
│   │   │   ├── download_data_minio_upload/  # Descarga EarthPulse → MinIO
│   │   │   ├── new_client/          # Alta nuevo cliente EarthPulse
│   │   │   ├── query_for_product/   # Stats pipes, diferencias, histórico
│   │   │   └── stats_wl/            # Estadísticas WL: soil, relate_pipes
│   │   ├── ground_movement/         # Producto S1 InSAR
│   │   │   ├── ground_movement.py   # ⭐ Clase orquestadora GroundMovement
│   │   │   ├── database_set_up/     # Creación tablas S1
│   │   │   ├── rs_rest_alarms/      # Procesamiento de alarmas
│   │   │   ├── rs_rest_download_process/  # Descarga + procesado MintPy
│   │   │   ├── rs_rest_get_client_geom/   # Geometrías + stats cliente GM
│   │   │   ├── rs_rest_new_client/        # Alta nuevo cliente GM
│   │   │   ├── rs_rest_ondemand/          # On-demand imágenes S1
│   │   │   ├── rs_rest_stats/             # Stats pipes + Network Health GM
│   │   │   └── utils/                     # ground_movement_functions.py
│   │   ├── evapotranspiration/      # Cálculo ET
│   │   └── service_diagnostic/     # Diagnóstico de variables de entorno
│   └── utils/
│       ├── database_tenancy.py      # ⭐ Context manager BD multi-tenant
│       └── common_functions.py
├── infrastructure/
│   ├── auth/
│   │   ├── auth.py                  # Keycloak OAuth2 Bearer
│   │   ├── tenancy.py               # Clase Tenant + get_tenant()
│   │   └── constants.py
│   ├── config/
│   │   ├── api_config.py            # Puerto, nombre, CORS
│   │   ├── cloud_client_config.py   # Spring Cloud Config Client
│   │   ├── eureka_config.py         # Registro Eureka
│   │   └── logs_config.py
│   ├── endpoints/
│   │   ├── routes.py                # ⭐ Registro de TODAS las rutas
│   │   └── remote_sensing_api.py    # ⭐ Clase RemoteSensingAPI (todos los handlers)
│   ├── scheduler/
│   │   ├── scheduler_config.py      # APScheduler build_scheduler()
│   │   └── daily_jobs.py            # 4 jobs nocturnos
│   └── schemas/
│       └── remote_sensing/          # Pydantic schemas por producto
```

---

## 3. Arranque y Ciclo de Vida

### `src/main.py`

```
uvicorn start
    → lifespan() arranca APScheduler (4 jobs a las 00:00)
    → FastAPI app configurada
        → Si PROFILE != "local":
            - Spring Cloud Config Client (obtiene auth, eureka, openapi settings)
            - Registro en Eureka
            - Rutas protegidas con Keycloak OAuth2
        → Si PROFILE == "local":
            - Rutas sin auth
            - CORS habilitado
    → PrometheusMiddleware en /actuator/prometheus
```

### Puntos de entrada de configuración

| Variable | Uso |
|---|---|
| `SPRING_PROFILES_ACTIVE` / `PROFILE` | Activa config server y Eureka |
| `RS_TENANT_ID` | Tenant por defecto para jobs del scheduler |
| `enviroment.env` | Archivo .env cargado con python-dotenv |

---

## 4. Autenticación y Multi-Tenancy

### Flujo de autenticación

```
Request HTTP
    → OAuth2AuthorizationCodeBearer (FastAPI Security)
    → keycloak_openid.decode_token(token)
    → Si válido → handler del endpoint
    → Si inválido → HTTP 401
```

### Multi-Tenancy (`TenancyDDBB`)

**Archivo**: `src/application/utils/database_tenancy.py`

```python
# Uso estándar en TODOS los módulos:
with TenancyDDBB(tenant_id) as cursor:
    cursor.execute("SELECT ...")
    result = cursor.fetchall()
# La conexión se cierra y hace commit automáticamente
```

El `tenant_id` llega al handler desde:
```python
tenant_id = get_current_tenant_id(request)  # desde JWT o header
```

La clase `Tenant` (en `tenancy.py`) contiene: `host`, `port`, `data_base`, `username`, `password`.
Los tenants se cargan desde el servidor de configuración o desde variables de entorno locales.

> ⚠️ **NUNCA hardcodear credenciales**. Siempre usar `TenancyDDBB(tenant_id)`.

---

## 5. Scheduler Diario

**Archivo config**: `src/infrastructure/scheduler/scheduler_config.py`
**Archivo jobs**: `src/infrastructure/scheduler/daily_jobs.py`

Se ejecutan **cada día a las 00:00** (cron local del servidor):

| Job ID | Función | Qué hace |
|---|---|---|
| `download_agriculture_images` | `job_download_agriculture_images()` | Descarga imágenes S2 para todos los clientes con `grid_s2 IS NOT NULL` |
| `demand_ground_movement` | `job_demand_ground_movement()` | On-demand interferogramas S1 para clientes con `id_process = 4` |
| `download_process_ground_movement` | `job_download_process_ground_movement()` | Descarga + procesa interferogramas con MintPy |
| `stats_ground_movement` | `job_stats_ground_movement()` | Calcula estadísticas de desplazamiento por tubería |

El `tenant_id` se lee de la variable de entorno `RS_TENANT_ID`.
Si no está definida → `RuntimeError` y el job no se ejecuta.

---

## 6. Módulo Agriculture (Sentinel-2)

### Propósito
Procesa imágenes Sentinel-2 para calcular índices de vegetación (NDVI, NDWI, NDMI) y estadísticas por parcelas agrícolas.

### Flujo completo de un cliente S2

```
1. Alta cliente → POST /remote-sensing/create-new-client-s2
   └── TableCreator.create_S2_grid()
   └── new_client_s2(client, url, layerName, tenant_id)  ← WFS GeoServer
   └── table_creator.create_all_tables(client_name, ...)
   └── insert_client_data(client_name, gdf, grid_s2_list, tenant_id)

2. Descarga → POST /remote-sensing/download-process (o job nocturno)
   └── download_client_data(client_name, tenant_id)
       └── pgadmin.extract_client_info() → obtiene grid_s2
       └── scihub.time_Sentinel() → rango temporal
       └── Copernicus OData API → busca productos S2
       └── Descarga GeoTIFF → ../data/
       └── SCIHUB_processes_main.get_processes() → NDVI/NDWI/NDMI
       └── Evapotranspiration.compute() → ET
       └── image_to_azure_blob_from_local_storage_s2() → sube a Azure
       └── pgadmin.insert_historic_images() → registra en BD

3. Consulta procesos → POST /remote-sensing/get-client-data
   └── get_client_data(identifiers, tenant_id)
   └── Devuelve: {client_id: [{name, date, bounds, cloud_average}]}

4. Obtener imagen → POST /remote-sensing/get-image
   └── get_image_from_azure(image_name) → Azure Blob → base64

5. Datos de imagen → POST /remote-sensing/get-image-data
   └── get_image_data(data_dict, tenant_id)

6. Histórico parcela → POST /remote-sensing/get-plot-historic-data
   └── get_plot_historic_values(image_name, plot_id, tenant_id)

7. Geometrías → POST /remote-sensing/get-client-geometries
   └── obtain_client_geometries(client_id, plot_ids, tenant_id)
```

### Archivos clave Agriculture

| Archivo | Responsabilidad |
|---|---|
| `rs_rest_0/download/download_main_new.py` | Orquestador descarga S2 |
| `rs_rest_0/download/REQ_download_functions.py` | Llamadas Copernicus API + `time_Sentinel()` |
| `rs_rest_0/processes/SCIHUB_processes_main.py` | Pipeline NDVI/NDWI/NDMI |
| `rs_rest_0/database_set_up/create_tables.py` | `TableCreator`: crea tablas S2 |
| `rs_rest_1/client_data.py` | `get_client_data()` → consulta BD procesos |
| `rs_rest_2/remote_sensing_image.py` | `get_image_from_azure()` |
| `rs_rest_2/remote_sensing_image_data.py` | `get_image_data()` |
| `rs_rest_3/plot_historic_values.py` | Histórico estadísticas parcelas |
| `rs_rest_4/get_client_geometries.py` | `obtain_client_geometries()` |
| `rs_rest_new_client/main_insertS2client.py` | `new_client_s2()` + `insert_client_data()` |
| `evapotranspiration/evapotranspiration.py` | `Evapotranspiration` (pyet) |

---

## 7. Módulo Ground Movement (Sentinel-1 / InSAR)

### Propósito
Detecta desplazamientos verticales/horizontales del terreno (en mm/año) usando interferometría SAR (InSAR) procesada con **MintPy** a través de la plataforma **HyP3/ASF**.

### Flujo completo de un cliente GM

```
1. Alta cliente → POST /remote-sensing/ground-movement-create-client
   └── NewClientGroundMovement.create_client(data, tenant_id)
       └── fetch_features(url, typeName) → WFS GeoServer → GeoDataFrame
       └── Calcula bounding box expandido (buffer 0.05°)
       └── TablesS1Creator.create_all_tables() → crea todas las tablas S1
       └── INSERT en client_info (name, grid_s1, grid_s2)
       └── INSERT en client_processes (id_process=4)
       └── INSERT en s1_client_date

2. On-demand imágenes → POST /remote-sensing/ground-movement-demand
   └── ImagesDemander.demand_images(data, tenant_id)
       └── asf_search → busca stack Sentinel-1 en el bbox del cliente
       └── Genera pares SBAS (Small Baseline Subset)
       └── HyP3 SDK → submits interferogram jobs
       └── Registra proyectos en BD (s1_projects)

3. Descarga + Proceso → POST /remote-sensing/ground-movement-process
   └── download_images(data, tenant_id)
       └── HyP3 SDK → descarga interferogramas completados
       └── Descomprime ZIPs → GeoTIFF
       └── image_to_minio_from_local_storage_s1() → sube a MinIO
   └── MintpyProcesses.mintpy_processes(data, tenant_id)
       └── download_minio_data() → descarga de MinIO
       └── mintpy run_smallbaselineApp → procesa interferogramas
       └── clip_lines_with_raster() → recorta GeoTIFF con geometrías de tuberías
       └── Sube resultado a MinIO

4. Estadísticas → POST /remote-sensing/ground-movement-stats
   └── obtain_stats(data, tenant_id)
       └── download_minio_data() → descarga GeoTIFF procesados
       └── Para cada .tif: process_geotiff_file() → extrae valor dominante por tubería
       └── INSERT en s1_client_stats (id_client, id_pipe, stats, date)
       └── INSERT en s1_client_velocity (velocidad mm/año)

5. Rango de stats → POST /remote-sensing/get-gm-range-stats
   └── get_last_range_stats(client_name, tenant_id, threshold_estable, threshold_precaucion)
   └── Clasifica tuberías: estable / precaución / crítico

6. Network Health Dashboard → POST /remote-sensing/gm-network-health-dashboard
   └── GroundMovementNetworkHealth.get_network_health_dashboard(client_name)
   └── Calcula Health Score (0-100) por distribución ponderada

7. Tuberías RISK → POST /remote-sensing/gm-network-health-critical-pipes
   └── GroundMovementNetworkHealth.get_critical_pipes_list(client_name, threshold_risk, limit)
   └── Devuelve GeoJSON FeatureCollection de tuberías en estado RISK
```

### Clasificación de estados GM

| Estado | Umbral velocidad | Descripción |
|---|---|---|
| `stable` | < 3 mm | Ruido instrumental |
| `attention` | 3–7 mm | Daño improbable pero no despreciable |
| `critical` | 7–12 mm | Alto riesgo de grietas |
| `risk` | > 12 mm | Probable ruptura |

**Health Score** = `(% stable × 1.0) + (% attention × 0.6) + (% critical × 0.2) + (% risk × 0.0)`

### Archivos clave Ground Movement

| Archivo | Responsabilidad |
|---|---|
| `ground_movement.py` | ⭐ Clase `GroundMovement` — orquesta todos los pasos |
| `database_set_up/database_set_up.py` | `TablesS1Creator` — crea tablas S1 |
| `rs_rest_new_client/s1_new_client.py` | `NewClientGroundMovement.create_client()` |
| `rs_rest_ondemand/on_demand_main.py` | `ImagesDemander` — busca y submite jobs a HyP3/ASF |
| `rs_rest_download_process/download_main.py` | Descarga interferogramas de HyP3 |
| `rs_rest_download_process/mintpy_main.py` | `MintpyProcesses` — procesa con MintPy |
| `rs_rest_download_process/clip_pipes_images.py` | `clip_lines_with_raster()` |
| `rs_rest_stats/stats_main.py` | `obtain_stats()` — extrae estadísticas por tubería |
| `rs_rest_stats/request_stats_pipe.py` | `get_stats_from_pipe()` — stats de una tubería concreta |
| `rs_rest_stats/request_min_max_lenght_stats.py` | `get_last_range_stats()` — distribución por rangos |
| `rs_rest_stats/network_health_gm.py` | `GroundMovementNetworkHealth` — dashboard + critical pipes |
| `rs_rest_get_client_geom/get_client_geometries.py` | `obtain_client_geometries_gm()` / `..._gm_stats()` |
| `utils/ground_movement_functions.py` | Funciones auxiliares GM |

---

## 8. Módulo EarthPulse (Water Leak Detection)

### Propósito
Detecta fugas de agua en redes de tuberías usando imágenes de humedad de suelo obtenidas de la API externa **EarthPulse** (basada en Sentinel-1). Las imágenes se almacenan en **MinIO** y los resultados en PostgreSQL.

### Flujo completo de un cliente EarthPulse

```
1. Alta cliente → POST /remote-sensing/new-client-earth-pulse  (multipart/form-data)
   └── EarthPulseNewClient.send_pipes(pipe_file, workspace_name) → MinIO
   └── EarthPulseNewClient.send_csv_leaks(leaks_csv, workspace_name) → MinIO
   └── EarthPulseNewClient.new_client(project_name, workspace_name, leaks_path, pipes_path, tenant_id)
       └── TableEarthPulseCreator.create_all_tables_earthpuse() → tablas EarthPulse
       └── POST API EarthPulse → submite job
       └── create_new_client_earthpulse() → INSERT en client_info + earthpulse_client_info

2. Listado datos bucket → POST /remote-sensing/list-data-bucket-earth-pulse
   └── EarthPulseNewClient.list_elements_earth_pulse(project_name)
   └── Conecta a S3/MinIO EarthPulse y lista objetos

3. Descarga humedad suelo → POST /remote-sensing/download-soil-moisture
   └── EarthPulseDownloadWaterLeak.download_wc_image() → descarga WC desde EarthPulse S3
   └── EarthPulseDownloadWaterLeak.download_soil_moisture_image()
       └── clip_lines_with_earthpulse_raster() → recorta con geometrías tuberías
       └── WaterLeakStatsExtractor → extrae stats por tubería
       └── INSERT en earthpulse_client_stats
       └── INSERT en earthpulse_raster_img

4. Rango datos disponibles → POST /remote-sensing/water-leak-data-range
   └── EarthPulseGetImages.get_data_range_water_leak(workspace_name, project_name, tenant_id)

5. Imagen WL → POST /remote-sensing/get-image-water-leak
   └── EarthPulseGetImages.get_images_water_leak(workspace_name, project_name, date, tenant_id)

6. Imagen diferencia → POST /remote-sensing/get-image-difference-water-leak
   └── EarthPulseGetImages.get_image_difference_water_leak(..., threshold)

7. Relación S1 ↔ EarthPulse pipes → POST /remote-sensing/relation-s1-earthpulse-pipes
   └── S1EarthPulsePipesRelation.process_pipe_relations(id_client)
   └── Intersecta geometrías S1 con geometrías EarthPulse → INSERT en s1_earthpulse_pipes_relation

8. Stats tuberías por fecha → POST /remote-sensing/pipes-stats-earthpulse-date
   └── EarthPulsePipesStats.get_pipes_stats_by_date(workspace, project, date)
   └── Devuelve GeoJSON con stats por tubería

9. Diferencia stats entre fechas → POST /remote-sensing/difference-pipes-stats-earthpulse
   └── WaterLeakDifferenceAnalyzer.analyze_difference(workspace, project, date1, date2, include_geometries=True)

10. Stats generales diferencia → POST /remote-sensing/difference-general-statistics-earthpulse
    └── WaterLeakDifferenceAnalyzer.get_general_statistics(...)

11. GeoJSON tuberías diferencia → POST /remote-sensing/difference-pipes-geojson-earthpulse
    └── WaterLeakDifferenceAnalyzer.get_pipes_geojson(...)

12. Discriminación suelo → POST /remote-sensing/soil-discrimination-earthpulse
    └── SoilDiscriminator.process_soil_discrimination(workspace, project, date)
    └── Clasifica tuberías como urbanas/rústicas por tipo de suelo

13. Stats urbano/rústico → POST /remote-sensing/urban-rustic-stats-earthpulse
    └── UrbanRusticPipesStats.get_urban_rustic_stats(project, date, ranges)

14. Relación GM ↔ WL → POST /remote-sensing/gm-water-leak-relation
    └── GMWaterLeakRelation.get_water_leak_pipes_from_project(project, id_s1_pipe, date)

15. Histórico stats por tuberías → POST /remote-sensing/historic-stats-by-pipes
    └── EarthPulseHistoricStats.get_historic_stats_by_pipes(pipe_ids)
    └── EarthPulseHistoricStats.get_historic_stats_with_metadata(pipe_ids)

16. Network Health WL Dashboard → POST /remote-sensing/network-health-dashboard
    └── NetworkHealthAnalyzer.get_dashboard_summary(workspace, project)

17. Network Health WL Trend → POST /remote-sensing/network-health-trend-analysis
    └── NetworkHealthAnalyzer.get_trend_analysis(workspace, project, pipe_id?)

18. Network Health WL Critical Pipes → POST /remote-sensing/network-health-critical-pipes
    └── NetworkHealthAnalyzer.get_top_critical_pipes(workspace, project, limit)

19. WL Dashboard (WaterLeakNetworkHealth) → POST /remote-sensing/wl-network-health-dashboard
    └── WaterLeakNetworkHealth.get_network_health_dashboard(client_name)

20. WL Critical Pipes → POST /remote-sensing/wl-network-health-critical-pipes
    └── WaterLeakNetworkHealth.get_critical_pipes_list(client_name, min_stats_value, limit)

21. Añadir reporte de fuga → POST /remote-sensing/add-leak-report
    └── LeakDetectionResultsAnalyzer.add_leak_report(data)
    └── INSERT en earthpulse_leak_detection_results

22. Histórico reportes fuga → POST /remote-sensing/get-historic-leak-report
    └── LeakDetectionResultsAnalyzer.get_historic_leak_report(data)
```

### Clasificación de estados WL

| Estado | Valor stats (0–1) | Descripción |
|---|---|---|
| `stable` | 0.0 – 0.7 | Sin fugas detectadas |
| `attention` | 0.7 – 0.9 | Fugas mínimas detectadas |
| `risk` | 0.9 – 1.0 | Alto riesgo / fugas severas |

**Health Score WL** = Porcentaje de longitud de tuberías en estado `stable`.

### Archivos clave EarthPulse

| Archivo | Responsabilidad |
|---|---|
| `common_utils.py` | `get_client_id_from_project()` — lookup id_client |
| `create_tables.py` | `TableEarthPulseCreator` — crea tablas EarthPulse |
| `new_client/create_new_client.py` | `EarthPulseNewClient` — alta cliente + integración API |
| `download_data_minio_upload/download_water_leak_earthpulse.py` | `EarthPulseDownloadWaterLeak` — descarga + clip + stats |
| `download_data_minio_upload/get_images_water_leak.py` | `EarthPulseGetImages` — obtener imágenes de MinIO |
| `download_data_minio_upload/clip_pipes_earthpulse.py` | `clip_lines_with_earthpulse_raster()` |
| `stats_wl/stats_pipes_water_leak.py` | `WaterLeakStatsExtractor` |
| `stats_wl/relate_pipes.py` | `S1EarthPulsePipesRelation` |
| `stats_wl/soil_discrimination.py` | `SoilDiscriminator` |
| `query_for_product/pipes_stats.py` | `EarthPulsePipesStats` |
| `query_for_product/difference_wl.py` | `WaterLeakDifferenceAnalyzer` |
| `query_for_product/historic_stats_for_pipes.py` | `EarthPulseHistoricStats` |
| `query_for_product/relate_pipes_gm_wl.py` | `GMWaterLeakRelation` |
| `query_for_product/discrimination_pipes_urban_rustic.py` | `UrbanRusticPipesStats` |
| `analytics/network_health_analyzer.py` | `NetworkHealthAnalyzer` (EarthPulse NH) |
| `analytics/network_health_wl.py` | `WaterLeakNetworkHealth` |
| `analytics/leak_detection_results.py` | `LeakDetectionResultsAnalyzer` |

---

## 9. Módulo Evapotranspiración

**Archivo**: `src/application/remote_sensing_tools/evapotranspiration/evapotranspiration.py`

- Clase `Evapotranspiration`
- Se invoca durante el proceso de descarga S2 (`download_client_data`)
- Usa la librería `pyet` para calcular ET de referencia
- Crea un grid sobre la geometría del cliente y calcula ET por celda
- Requiere datos meteorológicos (temperatura, radiación) extraídos de las imágenes S2

---

## 10. Módulo Service Diagnostic

**Endpoint**: `POST /remote-sensing/service-diagnostic`
**Archivo**: `src/application/remote_sensing_tools/service_diagnostic/diagnostic_service.py`

Devuelve el estado de todas las variables de entorno críticas **enmascarando valores sensibles** (muestra solo los 4 primeros caracteres de passwords/keys):

```json
{
  "database": { "HOST_POSTGRES": "10.0.", "PASSWORD_POSTGRES": "pass****" },
  "minio": { "MINIO_ACCESS_KEY": "mini****" },
  "azure": { "AZURE_BLOB_CONTAINER": "remo****" },
  "earthpulse": { "API_KEY_EARTH_PULSE": "eyJh****" },
  "asf": { "ASF_USERNAME": "user****" }
}
```

> 💡 **Usar este endpoint siempre que haya errores de conexión o configuración** antes de escalar el problema.

---

## 11. Capa de Infraestructura

### `RemoteSensingAPI` (`remote_sensing_api.py`)

Clase que actúa como **controlador HTTP**. Cada método es el handler de un endpoint. Sus responsabilidades son:
1. Extraer `tenant_id` del request: `get_current_tenant_id(request)`
2. Loggear la operación
3. Instanciar la clase de aplicación correspondiente
4. Delegar la lógica al módulo de aplicación
5. Gestionar excepciones → HTTP 400/500

**No contiene lógica de negocio**. Solo orquesta.

### `routes.py`

Registro de todas las rutas sobre el `APIRouter`. Prefijo global: `/api`.
Rutas visibles en Swagger: las que NO tienen `include_in_schema=False`.

### Schemas Pydantic (`infrastructure/schemas/remote_sensing/`)

| Schema | Uso |
|---|---|
| `RemoteSensingClientRequest` | `get-client-data` |
| `RemoteSensingImageRequest` | `get-image`, `get-image-data`, `delete-image` |
| `RemoteSensingPlotRequest` | `get-plot-historic-data` |
| `RemoteSensingGeometriesRequest` | `get-client-geometries` |
| `RemoteSensingGeometriesRequestGM` | `get-client-geometries-gm` |
| `RemoteSensingCreateNewClient` | `ground-movement-create-client` |
| `RemoteSensingDataDownloadGround` | `ground-movement-demand/process/stats` |
| `RemoteSensingClientNameGround` | `get-gm-range-stats` |
| `RemoteSensingListBucketDataEarthPulse` | endpoints EarthPulse bucket |
| `RemoteSensingDataWaterLeak` | `get-image-water-leak`, `soil-discrimination` |
| `DifferenceRequest` | `difference-*-earthpulse` |
| `UrbanRusticStatsRequest` | `urban-rustic-stats-earthpulse` |
| `GMWaterLeakRelationRequest` | `gm-water-leak-relation` |
| `HistoricStatsRequest` | `historic-stats-by-pipes` |
| `NetworkHealthDashboardRequest` | `network-health-dashboard` |
| `NetworkHealthTrendRequest` | `network-health-trend-analysis` |
| `NetworkHealthCriticalPipesRequest` | `network-health-critical-pipes` |
| `GMNetworkHealthDashboardRequest` | `gm-network-health-dashboard` |
| `GMNetworkHealthCriticalPipesRequest` | `gm-network-health-critical-pipes` |
| `WaterLeakNetworkHealthDashboardRequest` | `wl-network-health-dashboard` |
| `WaterLeakNetworkHealthCriticalPipesRequest` | `wl-network-health-critical-pipes` |
| `LeakReportRequest` | `add-leak-report` |
| `HistoricLeakReport` | `get-historic-leak-report` |

---

## 12. Tablas de Base de Datos

Todas las tablas viven en el schema `RS_SCHEMA_POSTGRES` (por defecto `remote_sensing_pro`).

### Tablas compartidas

| Tabla | Descripción |
|---|---|
| `client_info` | Registro de clientes: `id, name, grid_s2, grid_s1` |
| `client_processes` | Procesos activos por cliente: `id_client, id_process` |

> `id_process = 4` → cliente Ground Movement activo

### Tablas Agriculture (S2)

| Tabla | Descripción |
|---|---|
| `client_geometries` | Geometrías (parcelas) de cada cliente |
| `historic_images` | Histórico de imágenes descargadas: `id_client, name, date, bounds, cloud_average` |
| `client_plot_stats` | Estadísticas históricas por parcela |

### Tablas Ground Movement (S1)

| Tabla | Descripción |
|---|---|
| `s1_pipes` | Tuberías S1: `id, id_client, id_geom, geom` |
| `s1_nodes` | Nodos de red |
| `s1_projects` | Proyectos HyP3 de interferogramas |
| `s1_client_stats` | Estadísticas por tubería y fecha: `id_client, id_pipe, stats, date` |
| `s1_client_velocity` | Velocidad LOS (mm/año) por tubería y fecha |
| `s1_client_date` | Control temporal: `id_client, first_process, ref_year, last_year, search_image_time` |
| `s1_alarms_type1` | Alarmas tipo 1 |
| `s1_alarms_type2` | Alarmas tipo 2 |

### Tablas EarthPulse (Water Leak)

| Tabla | Descripción |
|---|---|
| `earthpulse_client_info` | Info específica EarthPulse: `id_client, project_name, workspace_name, project_id` |
| `earthpulse_raster_img` | Registro de imágenes descargadas: `id_client, date, bounds, raster_name` |
| `earthpulse_pipes` | Tuberías EarthPulse: `id, id_client, id_geom, geom, is_urban, is_rustic` |
| `earthpulse_client_stats` | Stats por tubería: `id_client, id_pipe, stats, date` |
| `s1_earthpulse_pipes_relation` | Relación entre tuberías S1 y EarthPulse |
| `earthpulse_leak_detection_results` | Reportes de fuga: `id_client, id_pipe, date, date_leak, leak_magnitude, visible, observations` |
| `earthpulse_client_file_count` | Control de ficheros procesados por cliente |

---

## 13. Variables de Entorno Críticas

### Base de datos PostgreSQL

| Variable | Descripción |
|---|---|
| `DB_POSTGRES` | Nombre de la base de datos |
| `HOST_POSTGRES` | Host del servidor PostgreSQL |
| `PORT_POSTGRES` | Puerto (default 5432) |
| `USER_POSTGRES` | Usuario |
| `PASSWORD_POSTGRES` | Contraseña |
| `RS_SCHEMA_POSTGRES` | Schema de tablas (ej: `remote_sensing_pro`) |
| `RS_TENANT_ID` | Tenant ID para jobs del scheduler |

### Azure Blob Storage (imágenes S2)

| Variable | Descripción |
|---|---|
| `AZURE_BLOB_CONNECTION_STRING` | Connection string Azure |
| `AZURE_BLOB_CONTAINER` | Nombre del contenedor |
| `AZURE_BLOB_FOLDER` | Carpeta dentro del contenedor |

### MinIO (imágenes S1 / EarthPulse)

| Variable | Descripción |
|---|---|
| `MINIO_HOST` | Host MinIO |
| `MINIO_PORT` | Puerto MinIO |
| `MINIO_BUCKET` | Bucket principal |
| `MINIO_ACCESS_KEY` | Access key |
| `MINIO_SECRET_KEY` | Secret key |
| `MINIO_FOLDER_GM` | Carpeta Ground Movement |

### EarthPulse API

| Variable | Descripción |
|---|---|
| `API_KEY_EARTH_PULSE` | API Key de EarthPulse |
| `API_EARTH_PULSE_URL` | URL base de la API |
| `API_EARTH_PULSE_URL_SERVICE_API` | URL del servicio de procesado |
| `EARTH_PULSE_S3_KEY` | Access key S3 de EarthPulse |
| `EARTH_PULSE_S3_SECRET_KEY` | Secret key S3 de EarthPulse |
| `EARTH_PULSE_S3_URL` | Endpoint S3 EarthPulse |
| `EARTH_PULSE_S3_REGION` | Región S3 |
| `EARTH_PULSE_S3_BUCKET` | Bucket S3 de EarthPulse |

### ASF / HyP3 (Ground Movement)

| Variable | Descripción |
|---|---|
| `ASF_USERNAME` | Usuario ASF Vertex |
| `ASF_PASSWORD` | Contraseña ASF Vertex |
| `USER_TOKEN` | Token CDS (ERA5 / copernicus) |

### Sentinel-2 / Copernicus

| Variable | Descripción |
|---|---|
| `SENTINEL_API_URL` | URL de la API OData de Copernicus |
| `RS_PATH_IMAGES` | Ruta local temporal para imágenes descargadas |

### Spring Cloud / Eureka / Keycloak

| Variable | Descripción |
|---|---|
| `SPRING_PROFILES_ACTIVE` | Perfil activo |
| `KEYCLOAK_CLIENT_ID` | Client ID de Keycloak |

---

## 14. Mapa Completo de Endpoints

Prefijo base: `/api/remote-sensing/`

### Visibles en Swagger

| Endpoint | Método | Descripción | Tag |
|---|---|---|---|
| `get-client-data` | POST | Procesos e imágenes disponibles por cliente | Remote Sensing Tools |
| `get-image` | POST | Obtiene imagen S2 desde Azure | Remote Sensing Tools |
| `get-image-data` | POST | Datos de imagen S2 | Remote Sensing Tools |
| `get-plot-historic-data` | POST | Histórico de valores por parcela | Remote Sensing Tools |
| `set-up-client-data` | POST | Configuración inicial datos cliente | Remote Sensing Tools |
| `ground-movement-create-client` | POST | Alta nuevo cliente Ground Movement | Remote Sensing Tools |
| `ground-movement-demand` | POST | On-demand imágenes S1 para cliente | Remote Sensing Tools |
| `ground-movement-process` | POST | Procesa imágenes S1 (MintPy) | Remote Sensing Tools |
| `ground-movement-stats` | POST | Calcula estadísticas de desplazamiento | Remote Sensing Tools |
| `network-health-dashboard` | POST | Dashboard salud red EarthPulse | EarthPulse - Network Health |
| `network-health-critical-pipes` | POST | Tuberías críticas EarthPulse | EarthPulse - Network Health |
| `network-health-trend-analysis` | POST | Análisis tendencias EarthPulse | EarthPulse - Network Health |
| `gm-network-health-dashboard` | POST | Dashboard salud red Ground Movement | Ground Movement - Network Health |
| `gm-network-health-critical-pipes` | POST | Tuberías RISK Ground Movement | Ground Movement - Network Health |
| `wl-network-health-dashboard` | POST | Dashboard salud red Water Leak | EarthPulse - Network Health |
| `wl-network-health-critical-pipes` | POST | Tuberías críticas Water Leak | EarthPulse - Network Health |
| `add-leak-report` | POST | Añade reporte de fuga | EarthPulse - Leak Report |
| `get-historic-leak-report` | POST | Histórico de reportes de fuga | EarthPulse - Leak Report |
| `service-diagnostic` | POST | Diagnóstico variables de entorno | Service Diagnostic |

### Ocultos en Swagger (`include_in_schema=False`)

| Endpoint | Descripción |
|---|---|
| `get-client-geometries` | Geometrías cliente S2 |
| `correct-bbdd` | Corrección BD Sentinel-2 |
| `delete-image` | Eliminar imagen |
| `download-process` | Descarga imágenes S2 |
| `create-new-client-s2` | Alta cliente S2 |
| `update-data` | Descarga todos los clientes |
| `get-client-geometries-gm` | Geometrías cliente GM |
| `get-client-geometries-gm-stats` | Geometrías + stats cliente GM |
| `get-gm-stats-from-pipe` | Stats de una tubería GM concreta |
| `get-gm-range-stats` | Distribución por rangos GM |
| `ground-movement-demand-all-clients` | On-demand todos los clientes GM |
| `ground-movement-download-process-all-clients` | Descarga + proceso todos los clientes GM |
| `ground-movement-stats-all-clients` | Stats todos los clientes GM |
| `new-client-earth-pulse` | Alta cliente EarthPulse |
| `list-data-bucket-earth-pulse` | Lista bucket EarthPulse |
| `list-data-bucket-gisdevtools` | Lista bucket MinIO interno |
| `download-soil-moisture` | Descarga humedad suelo |
| `water-leak-data-range` | Rango de datos disponibles WL |
| `get-image-water-leak` | Imagen WL por fecha |
| `get-image-difference-water-leak` | Imagen diferencia WL |
| `relation-s1-earthpulse-pipes` | Relaciona tuberías S1 ↔ EarthPulse |
| `pipes-stats-earthpulse-date` | Stats tuberías EarthPulse por fecha |
| `difference-pipes-stats-earthpulse` | Diferencia stats entre fechas |
| `difference-general-statistics-earthpulse` | Stats generales diferencia |
| `difference-pipes-geojson-earthpulse` | GeoJSON tuberías diferencia |
| `soil-discrimination-earthpulse` | Discriminación suelo urbano/rústico |
| `urban-rustic-stats-earthpulse` | Stats por rangos urbano/rústico |
| `gm-water-leak-relation` | WL pipes desde una tubería GM |
| `historic-stats-by-pipes` | Histórico stats por lista de tuberías |

---

## 15. Flujos End-to-End

### Flujo 1: Nueva imagen S2 disponible (nocturno automático)

```
00:00 APScheduler
    → job_download_agriculture_images()
    → download_all_clients_images(tenant_id)
        → BD: SELECT name FROM client_info WHERE grid_s2 IS NOT NULL
        → Para cada cliente:
            → download_client_data(client_name, tenant_id)
                → Copernicus API → nuevo producto S2
                → Procesa NDVI/NDWI/NDMI
                → Evapotranspiración
                → Sube a Azure Blob
                → Registra en historic_images
```

### Flujo 2: Ciclo completo Ground Movement (nocturno automático)

```
00:00 APScheduler
    1. job_demand_ground_movement()
       → demand_all_clients(tenant_id)
       → Para cada cliente con id_process=4:
           → ImagesDemander.demand_images() → HyP3 job submitted

    2. job_download_process_ground_movement()
       → download_and_process_all_clients(tenant_id)
       → Para cada cliente:
           → download_images() → HyP3 descarga completados → MinIO
           → MintpyProcesses.mintpy_processes() → MintPy → GeoTIFF → MinIO

    3. job_stats_ground_movement()
       → stats_all_clients(tenant_id)
       → Para cada cliente:
           → obtain_stats() → GeoTIFF MinIO → stats por tubería → BD
```

### Flujo 3: Consulta Network Health (tiempo real)

```
POST /api/remote-sensing/gm-network-health-dashboard
    body: { "workspace_name": "go_moncada", "project_name": "go_moncada" }
    
    → RemoteSensingAPI.get_gm_network_health_dashboard()
    → get_current_tenant_id(request) → tenant_id
    → GroundMovementNetworkHealth(tenant_id)
    → analyzer.get_network_health_dashboard("go_moncada")
        → BD: SELECT velocity FROM s1_client_velocity WHERE id_client=X
        → Clasifica tuberías por umbrales (3/7/12 mm)
        → Health Score = weighted average
        → Calcula tendencia (últimas N mediciones)
    → Respuesta JSON con summary, distribution, statistics, trend, alerts
```

---

## 16. Patrones de Error Comunes y Diagnóstico

### ❌ Error: `No tenant_id found in request`
- **Causa**: JWT inválido o request sin header de autenticación en entorno no-local
- **Archivo**: `src/infrastructure/auth/tenancy.py` → `get_current_tenant_id()`
- **Solución**: Verificar token Bearer en la cabecera Authorization

### ❌ Error: `Cliente 'X' no encontrado en la base de datos`
- **Causa**: El `client_name` no existe en `client_info.name`
- **Archivo**: `pgadmin_functions.py` → `extract_client_info()`
- **Solución**: Verificar el nombre exacto del cliente en la tabla `client_info`

### ❌ Error: `Environment variable 'RS_TENANT_ID' is not set`
- **Causa**: El scheduler intenta ejecutarse pero la variable no está definida
- **Archivo**: `src/infrastructure/scheduler/daily_jobs.py` → `_get_tenant_id()`
- **Solución**: Definir `RS_TENANT_ID` en las variables de entorno del servidor

### ❌ Error en `TenancyDDBB`: `connection refused` o `authentication failed`
- **Causa**: Variables de BD incorrectas (HOST, PORT, USER, PASSWORD)
- **Diagnóstico**: Llamar a `POST /api/remote-sensing/service-diagnostic` para ver configuración
- **Archivo**: `src/application/utils/database_tenancy.py`

### ❌ Error en descarga S2: `La carpeta RS_PATH_IMAGES no existe`
- **Causa**: Variable `RS_PATH_IMAGES` no definida o ruta inaccesible
- **Archivo**: `agriculture/rs_rest_0/download/download_main_new.py`

### ❌ Error: `La respuesta no es GeoJSON` (alta cliente WFS)
- **Causa**: El servidor WFS no responde con `Content-Type: application/json`
- **HTTP**: Devuelve `400 Bad Request` con `"error": "Invalid WFS server response"`
- **Archivo**: `ground_movement/rs_rest_new_client/s1_new_client.py` → `fetch_features()`
- **También en**: `RemoteSensingAPI.create_new_client_s2()` para cliente S2

### ❌ Error en MintPy: fallo silencioso
- **Causa**: El subprocess de MintPy falla sin excepción Python
- **Archivo**: `ground_movement/rs_rest_download_process/mintpy_main.py`
- **Diagnóstico**: Revisar logs con nivel DEBUG del subprocess

### ❌ Error: `grid_s2 es None`
- **Causa**: El cliente fue registrado sin grid S2
- **Archivo**: `agriculture/rs_rest_0/download/download_main_new.py`
- **Solución**: Actualizar `client_info.grid_s2` en BD o registrar el cliente con grid

### ❌ Error EarthPulse: `No se encontró cliente con project_name='X'`
- **Causa**: El `project_name` no coincide con `client_info.name`
- **Archivo**: `EarthPulse/common_utils.py` → `get_client_id_from_project()`
- **Nota**: En EarthPulse, `project_name` == `client_info.name`

### ❌ Error MinIO: `S3Error` al subir/descargar
- **Causa**: Variables `MINIO_HOST`, `MINIO_ACCESS_KEY`, `MINIO_SECRET_KEY` incorrectas
- **Archivo**: `rs_common_functions.py` + `download_water_leak_earthpulse.py`
- **Diagnóstico**: Llamar a `service-diagnostic`

### ❌ Error: `CRS is None` en rasterio/geopandas
- **Causa**: El GeoTIFF descargado no tiene CRS definido
- **Archivos afectados**: `mintpy_main.py`, `clip_pipes_images.py`, `clip_pipes_earthpulse.py`
- **Solución**: Forzar CRS con `rasterio.crs.CRS.from_epsg(epsg)` antes de procesar

### ❌ HTTP 401 en entorno no-local
- **Causa**: Token expirado o perfil de Keycloak mal configurado
- **Archivo**: `src/infrastructure/auth/auth.py` → `get_auth()`

---

## 17. Dependencias Externas

| Dependencia | Tipo | Uso |
|---|---|---|
| **Copernicus OData API** | HTTP REST | Búsqueda y descarga de productos Sentinel-2 |
| **ASF Vertex / asf_search** | Python SDK | Búsqueda de imágenes Sentinel-1 |
| **HyP3 SDK** | Python SDK | Generación y descarga de interferogramas S1 |
| **MintPy** | Librería Python + CLI | Procesado InSAR (series temporales de desplazamiento) |
| **EarthPulse API** | HTTP REST | Detección de humedad de suelo / water leak |
| **EarthPulse S3** | MinIO/S3 | Almacenamiento imágenes WL |
| **MinIO interno** | MinIO S3 | Almacenamiento imágenes S1 procesadas |
| **Azure Blob Storage** | Azure SDK | Almacenamiento imágenes S2 |
| **PostgreSQL + PostGIS** | psycopg2 | BD principal multi-tenant |
| **Keycloak** | OAuth2/OIDC | Autenticación + multi-tenancy |
| **Spring Cloud Config** | HTTP | Configuración centralizada (entorno no-local) |
| **Eureka** | Netflix OSS | Service discovery (entorno no-local) |
| **Prometheus** | Métricas | `/actuator/prometheus` |

---

## 18. Cómo Añadir un Nuevo Cliente

### Cliente Sentinel-2 (Agriculture)

```
1. POST /api/remote-sensing/create-new-client-s2
   Body: {
     "client_name": "nombre_cliente",
     "url": "http://geoserver/wfs",
     "layerName": "workspace:layer"
   }
   → Crea tablas S2 + registra en client_info + inserta geometrías
   
2. POST /api/remote-sensing/download-process
   Body: { "client_name": "nombre_cliente" }
   → Primera descarga de imágenes S2
```

### Cliente Ground Movement (S1 InSAR)

```
1. POST /api/remote-sensing/ground-movement-create-client
   Body: {
     "name": "nombre_cliente",
     "url": "http://geoserver/wfs",
     "typeName": "workspace:layer"
   }
   → Crea tablas S1 + registra en client_info con id_process=4
   
2. POST /api/remote-sensing/ground-movement-demand
   Body: { "id_client": 123 }
   → Busca y solicita interferogramas S1

3. POST /api/remote-sensing/ground-movement-process
   Body: { "id_client": 123 }
   → Descarga + MintPy + clip → MinIO

4. POST /api/remote-sensing/ground-movement-stats
   Body: { "id_client": 123 }
   → Estadísticas de desplazamiento → BD
```

### Cliente EarthPulse (Water Leak)

```
1. POST /api/remote-sensing/new-client-earth-pulse  (multipart/form-data)
   Fields:
     - pipe_file: GeoJSON con geometrías de tuberías
     - leaks_csv: CSV histórico de fugas conocidas
     - project_name: "nombre_proyecto" (minúsculas)
     - workspace_name: "nombre_workspace" (minúsculas)
   → Sube archivos a MinIO → llama API EarthPulse → registra en BD

2. POST /api/remote-sensing/download-soil-moisture
   Body: {
     "project_name": "nombre_proyecto",
     "workspace_name": "nombre_workspace",
     "epsg": 25830
   }
   → Descarga imágenes de humedad → clip → stats por tubería → BD

3. POST /api/remote-sensing/relation-s1-earthpulse-pipes
   Body: { "id_client": 123 }
   → Relaciona tuberías S1 con tuberías EarthPulse por intersección geométrica
```

---

*Documento generado automáticamente el 2026-03-05. Mantener sincronizado con cambios en `routes.py` y `remote_sensing_api.py`.*

