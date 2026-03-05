# go-gis â€” Arquitectura completa del cÃ³digo fuente (`src/`)

> **Generado:** Marzo 2026
> **VersiÃ³n del proyecto:** ver `package.json`
> **Documento de ejemplos:** `docs/examples/EXAMPLES.md`

---

## 0. Protocolo de navegaciÃ³n para agentes IA â€” Tres fuentes de conocimiento

> **REGLA FUNDAMENTAL:** Este documento (`GO-GIS-ARCHITECTURE.md`) es la fuente de verdad de **tipos y firmas TypeScript**. `EXAMPLES.md` es la fuente de verdad de **patrones de uso listos para copiar**. `go-gis-VERBATIM.md` contiene el **cÃ³digo fuente real** de los 20 archivos clave de la librerÃ­a. Los tres forman un grafo de conocimiento completo; ninguno es suficiente por sÃ­ solo para entender la librerÃ­a al completo.

### Los tres documentos

| # | Documento | Ruta relativa al repo | LÃ­neas | PropÃ³sito |
|---|-----------|----------------------|--------|-----------|
| 1 | **Este documento** | `src/GO-GIS-ARCHITECTURE.md` | ~2200 | Tipos TS, firmas de mÃ©todos, jerarquÃ­a de clases, enums, contratos de API |
| 2 | **Ejemplos de uso** | `docs/examples/EXAMPLES.md` | ~1900 | Patrones JSON+JS listos para copiar, catÃ¡logo de 170+ ejemplos, guÃ­a de agentes Â§15 |
| 3 | **CÃ³digo fuente** | `go-gis-VERBATIM.md` (raÃ­z del repo) | ~15500 | ImplementaciÃ³n fuente verbatim de 20 archivos clave de `src/goapi/` |

### Protocolo de uso para agentes

1. **Para generar un fragmento de cÃ³digo** â†’ ir primero a `EXAMPLES.md Â§15` (guÃ­a de agentes) + la secciÃ³n `EXAMPLES Â§` que corresponda + verificar tipos y firmas aquÃ­.
2. **Para entender una firma TypeScript o tipo** â†’ buscar en este documento la secciÃ³n `Â§4â€“Â§11` correspondiente. La columna `[FILES L-N]` apunta a la lÃ­nea exacta en `go-gis-VERBATIM.md` donde ver la implementaciÃ³n real.
3. **Para profundizar en la implementaciÃ³n** â†’ ir a `go-gis-VERBATIM.md` a la lÃ­nea indicada en la columna `[FILES]`.
4. **Para depurar un error** â†’ consultar `EXAMPLES.md Â§15d Tabla de errores comunes` + `Â§7 Eventos` de este documento.
5. **Para aÃ±adir una capa** â†’ seguir `EXAMPLES.md Â§4` + verificar el enum `GOLayerType` en `Â§5` de este documento + comprobar `layerOptions` en `[FILES L9104â€“L9685]`.

### Ãndice maestro de archivos fuente â€” `go-gis-VERBATIM.md`

Cada archivo ocupa exactamente las lÃ­neas indicadas en `go-gis-VERBATIM.md`. Usar estas referencias como `[FILES L{inicio}â€“L{fin}]`.

| Archivo fuente (`src/goapi/...`) | Inicio | Fin | SecciÃ³n ARCH | SecciÃ³n EXAMPLES |
|-----------------------------------|--------|-----|--------------|-----------------|
| `Utils/types/types.ts` | **L1** | **L1373** | Â§11 Tipos clave | Â§2â€“Â§4 (todos los tipos JSON) |
| `Map/GoMap.ts` | **L1374** | **L2608** | Â§4 GoMap | Â§3, Â§4, Â§6, Â§7, Â§9 |
| `Map/GoMapLoader.ts` | **L2609** | **L3331** | Â§4 GoMapLoader | Â§3a loadScripts, Â§13 |
| `Map/GoStagingMap.ts` | **L3332** | **L4129** | Â§4 GOStagingMap | Â§4 getLayer, Â§6bâ€“6k, Â§9, Â§12 |
| `Layers/GoLayer/index.ts` | **L4130** | **L5131** | Â§5 GOLayer | Â§4 (todos los tipos de capa) |
| `Layers/GoVectorLayer/index.ts` | **L5132** | **L6096** | Â§5 GOVectorLayer | Â§4b, Â§5aâ€“5c, Â§6e, Â§6gâ€“6h |
| `Layers/GoClusterLayer/index.ts` | **L6097** | **L7310** | Â§5 GOClusterLayer | Â§4f |
| `Layers/GoGeoJsonLayer/index.ts` | **L7311** | **L7915** | Â§5 GOGeoJsonLayer | Â§4e, Â§12 |
| `Layers/GOWMS/index.ts` | **L7916** | **L8214** | Â§5 GOWMSLayer | Â§4c, Â§4a (base WMS) |
| `Layers/GoWFS/index.ts` | **L8215** | **L9012** | Â§5 GOWFSLayer | Â§4d |
| `UIMap/types.ts` | **L9013** | **L9034** | Â§9 UIMap | Â§3e ui botones |
| `Interactions/Features/index.ts` | **L9035** | **L9064** | Â§6 Features | Â§6b Highlight, Â§6c Hover |
| `Interactions/functions/getFeaturesByLayer.ts` | **L9065** | **L9068** | Â§6 Firmas | Â§6i getFeatures |
| `Services/GoStore/index.ts` | **L9069** | **L9103** | Â§7 GoStore | Â§3a loadScripts, Â§8c |
| `Utils/constants/index.ts` | **L9104** | **L9685** | Â§11 Enums + layerOptions | Â§5b estilos, Â§6f filtros, Â§7 eventos |
| `Layers/common/terrainColors.ts` | **L9686** | **L9696** | Â§5 Colores terreno | Â§4 (paletas) |
| `Interactions/geoTools/buffer/index.ts` | **L9697** | **L9968** | Â§6 geoTools GoBuffer | Â§6d Buffer/Intersect |
| `Interactions/index.ts` (GoInteractions) | **L9969** | **L13490** | Â§6 Firmas completas | Â§6 todos los grupos |
| `Toc/GoOwnToc.ts` | **L13491** | **L15458** | Â§8 GoOwnToc | Â§3d optionToc, Â§9 TOC dinÃ¡mico |
| `Interactions/Features/interact/GetFeaturesByLayer.ts` | **L15459** | **L15462** | Â§6 Firmas | Â§6i getFeatures |

### Grafo de dependencias entre documentos (para navegaciÃ³n bidireccional)

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  GO-GIS-ARCHITECTURE.md  (TIPOS + FIRMAS)                           â”‚
â”‚  Â§4 GoMap / Â§5 Layers / Â§6 Interactions / Â§11 Types                 â”‚
â”‚    â†• [EXAMPLES Â§N]  â†â†’  [FILES L-N]                                 â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
            â†•                              â†•
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”   â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  EXAMPLES.md  (PATRONES)   â”‚   â”‚  go-gis-VERBATIM.md  (FUENTE)  â”‚
â”‚  Â§1â€“Â§15 snippets listos    â”‚â†â†’ â”‚  L1â€“L15462 cÃ³digo source real      â”‚
â”‚  Â§14 catÃ¡logo 170+ ejemplosâ”‚   â”‚  20 archivos clave de src/goapi/   â”‚
â”‚  Â§15 guÃ­a agentes          â”‚   â”‚                                    â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜   â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## RelaciÃ³n con EXAMPLES.md

> **Para agentes IA:** Este documento describe la arquitectura interna (`src/`). El documento complementario `docs/examples/EXAMPLES.md` contiene los patrones de uso, snippets JSON/JS listos para copiar y el catÃ¡logo de 170+ ejemplos. La bÃºsqueda cruzada es **bidireccional**:
> - Desde aquÃ­ hacia ejemplos: cada secciÃ³n lleva una nota `[EXAMPLES Â§N]` con el enlace exacto al patrÃ³n correspondiente.
> - Desde ejemplos hacia aquÃ­: cada patrÃ³n lleva una nota `[ARCH Â§N]` con la firma TypeScript exacta.
>
> **Regla para agentes:** Para generar un ejemplo, leer primero `EXAMPLES.md Â§15` (guÃ­a de agentes) + la secciÃ³n `EXAMPLES Â§` que corresponda + regresar aquÃ­ para verificar tipos y firmas.

### Ãndice cruzado ARCHITECTURE â†’ EXAMPLES â†’ go-gis-VERBATIM

| SecciÃ³n ARCH | SecciÃ³n EXAMPLES relacionada | CÃ³digo fuente en go-gis-VERBATIM.md |
|---|---|---|
| Â§1 Punto de entrada (`@ifc`, `@mapbox`) | Â§11 Visor BIM/IFC, Â§10 Visor 3D/Mapbox | â€” (ver `src/index.ts` exterior al verbatim) |
| Â§2 Arquitectura general | Â§3 InicializaciÃ³n, Â§13 Ejemplo mÃ­nimo | â€” |
| Â§3 MÃ³dulo Config (GO_CONFIG, URLs) | Â§3a loadScripts (enviroment) | â€” (ver `src/goapi/Config/index.ts`) |
| Â§4 GoMapLoader / MapConfigLoader | Â§3a loadScripts, Â§13 Ejemplo mÃ­nimo | **[FILES L2609â€“L3331]** `Map/GoMapLoader.ts` |
| Â§4 GoMap â€” mÃ©todos de mapa/vista/capa | Â§3b config.json, Â§4 Patrones de capas, Â§6 Interacciones | **[FILES L1374â€“L2608]** `Map/GoMap.ts` |
| Â§4 GOStagingMap | Â§4 (getLayer), Â§6bâ€“6k (interacciones), Â§9 TOC, Â§12 Descarga | **[FILES L3332â€“L4129]** `Map/GoStagingMap.ts` |
| Â§4 GoView | Â§3c optionView | **[FILES L1374â€“L2608]** (dentro de `GoMap.ts`) |
| Â§5 GOLayer / GOStyledLayer | Â§4 Patrones de capas (todas las subsecciones) | **[FILES L4130â€“L5131]** `Layers/GoLayer/index.ts` |
| Â§5 GOLayerType enum | Â§4 (campo `layerType` en cada capa) | **[FILES L9104â€“L9685]** `Utils/constants/index.ts` |
| Â§5 layerOptions (todos los campos) | Â§4 campos comunes + Â§4aâ€“4m especÃ­ficos | **[FILES L9104â€“L9685]** `Utils/constants/index.ts` |
| Â§5 GOVectorLayer mÃ©todos | Â§6e Filtro CQL, Â§6g Editar WFS-T, Â§6h Eliminar | **[FILES L5132â€“L6096]** `Layers/GoVectorLayer/index.ts` |
| Â§5 GOGeoJsonLayer | Â§4e GeoJSON, Â§12 Descarga | **[FILES L7311â€“L7915]** `Layers/GoGeoJsonLayer/index.ts` |
| Â§5 GOClusterLayer | Â§4f ClÃºster | **[FILES L6097â€“L7310]** `Layers/GoClusterLayer/index.ts` |
| Â§5 GOWMSLayer | Â§4c WMS | **[FILES L7916â€“L8214]** `Layers/GOWMS/index.ts` |
| Â§5 GOWFSLayer | Â§4d WFS | **[FILES L8215â€“L9012]** `Layers/GoWFS/index.ts` |
| Â§6 Analysis (GoMeasure, GoDensityTool) | Â§6k MediciÃ³n | **[FILES L9969â€“L13490]** `Interactions/index.ts` |
| Â§6 Features (GoHighlight, GoHover) | Â§6b Highlight, Â§6c Hover | **[FILES L9035â€“L9064]** `Interactions/Features/index.ts` |
| Â§6 Selection | Â§6j SelecciÃ³n por polÃ­gono | **[FILES L9969â€“L13490]** `Interactions/index.ts` |
| Â§6 ExternalsConnections (HERE, WMS) | Â§8a GeocodificaciÃ³n, Â§8b Routing | **[FILES L9969â€“L13490]** `Interactions/index.ts` |
| Â§6 Hydraulics (GoCloseTools, GoUpDown) | CatÃ¡logo Â§14: ex_close_tool, ex_upstream | **[FILES L9969â€“L13490]** `Interactions/index.ts` |
| Â§6 geoTools (GoBuffer, GoIntersect) | Â§6d Buffer + IntersecciÃ³n | **[FILES L9697â€“L9968]** `Interactions/geoTools/buffer/index.ts` |
| Â§6 Firmas de mÃ©todos clave | Â§6 Patrones de interacciÃ³n (todas) | **[FILES L9969â€“L13490]** `Interactions/index.ts` |
| Â§7 GoStore / GoState | Â§3a loadScripts (isExample, token) | **[FILES L9069â€“L9103]** `Services/GoStore/index.ts` |
| Â§7 GoAuth / GoAxiosServices | Â§8c GestiÃ³n de errores | **[FILES L9069â€“L9103]** `Services/GoStore/index.ts` |
| Â§8 GoOwnToc | Â§3d optionToc, Â§9 TOC dinÃ¡mico | **[FILES L13491â€“L15458]** `Toc/GoOwnToc.ts` |
| Â§9 UIConfig | Â§3e ui botones | **[FILES L9013â€“L9034]** `UIMap/types.ts` |
| Â§10 i18nService / SupportedLangs | Â§3f translations | **[FILES L9104â€“L9685]** `Utils/constants/index.ts` |
| Â§11 ISVGIcon | Â§5a styleSVG, Â§5b condicional | **[FILES L1â€“L1373]** `Utils/types/types.ts` |
| Â§11 styleFromUser / GOGeometryType | Â§5c styleGeojson, Â§6b Highlight | **[FILES L1â€“L1373]** `Utils/types/types.ts` |
| Â§11 GoFilter / GoFilterCondition | Â§6e Filtro CQL, Â§6f Filtro No-CQL | **[FILES L9104â€“L9685]** `Utils/constants/index.ts` |
| Â§11 GOMapEvents / GOLayerEvents | Â§7 Sistema de eventos | **[FILES L9104â€“L9685]** `Utils/constants/index.ts` |
| Â§11 themes / typeOn / UpDownType | Â§2 Plantilla HTML, Â§6a Clic, Â§15 GuÃ­a agentes | **[FILES L9104â€“L9685]** `Utils/constants/index.ts` |
| Â§12 Mapa de interconexiones | Â§15b Receta para agentes | â€” |
| Â§13 PatrÃ³n de uso tÃ­pico | Â§13 Ejemplo mÃ­nimo completo, Â§15 GuÃ­a agentes | â€” |

---

## Tabla de contenidos

0. [Protocolo de navegaciÃ³n tri-documento](#0-protocolo-de-navegaciÃ³n-para-agentes-ia--tres-fuentes-de-conocimiento)
1. [Punto de entrada](#1-punto-de-entrada)
2. [Arquitectura general](#2-arquitectura-general)
3. [MÃ³dulo Config](#3-mÃ³dulo-config)
4. [MÃ³dulo Map](#4-mÃ³dulo-map)
   - [GoMapLoader](#gomaploadergomaploadersts)
   - [GoMap](#gomapgomapsts)
   - [GOStagingMap](#gostagingmapgostagingmapts)
   - [GoView](#goviewgoviewts)
   - [GoCapabilities](#gocapabilitiesgocapabilitiests)
5. [Sistema de Capas (Layers)](#5-sistema-de-capas-layers)
   - [JerarquÃ­a de clases](#jerarquÃ­a-de-clases)
   - [GOLayer â€” clase base abstracta](#golayer--clase-base-abstracta)
   - [GOStyledLayer](#gostyledlayer)
   - [Tipos de capa disponibles](#tipos-de-capa-disponibles)
   - [Enum GOLayerType](#enum-golayertype)
   - [Tipo layerOptions](#tipo-layeroptions--configuraciÃ³n-universal-de-capa)
6. [MÃ³dulo Interactions](#6-mÃ³dulo-interactions)
   - [Analysis](#analysis)
   - [Features](#features)
     - [Clase GoHighlight](#clase-gohighlight)
     - [Clase GoHover](#clase-gohover)
   - [Selection](#selection)
   - [ExternalsConnections](#externalsconnections)
   - [Hydraulics](#hydraulics)
   - [Views](#views)
   - [geoTools](#geotools)
   - [Firmas de mÃ©todos clave de GoInteractions](#firmas-de-mÃ©todos-clave-de-gointeractions)
7. [MÃ³dulo Services](#7-mÃ³dulo-services)
   - [GoState](#gostate-interface)
8. [MÃ³dulo TOC](#8-mÃ³dulo-toc)
9. [MÃ³dulo UIMap](#9-mÃ³dulo-uimap)
10. [MÃ³dulo i18nService](#10-mÃ³dulo-i18nservice)
11. [MÃ³dulo Utils](#11-mÃ³dulo-utils)
    - [Enums clave](#enums-clave)
    - [Tipos clave](#tipos-clave)
12. [Mapa de interconexiones entre mÃ³dulos](#12-mapa-de-interconexiones-entre-mÃ³dulos)
13. [PatrÃ³n de uso tÃ­pico](#13-patrÃ³n-de-uso-tÃ­pico)

---

## 1. Punto de entrada

> **[EXAMPLES]** Los mÃ³dulos `@ifc/index` y `@mapbox/index` tienen sus patrones de uso en `EXAMPLES.md Â§11 Visor BIM/IFC` y `EXAMPLES.md Â§10 Visor 3D/Mapbox` respectivamente. El resto de mÃ³dulos (`@interactions`, `@layers`, `@utils`, etc.) se usan en `EXAMPLES.md Â§3â€“Â§9`.

**Archivo:** `src/index.ts`

La API pÃºblica de la librerÃ­a se construye re-exportando todos los mÃ³dulos bajo `src/goapi/`:

```typescript
export * from "@config/index";       // Constantes GO_CONFIG
export * from "@goTypes/index";      // Todas las definiciones de tipos
export * from "@ifc/index";          // Soporte IFC (BIM)
export * from "@interactions/index"; // Todas las interacciones del mapa
export * from "@layers/index";       // Todos los tipos de capa
export * from "@services/index";     // Servicios (store, auth, axios, ...)
export * from "@toc/index";          // Tabla de contenidos
export * from "@utils/index";        // Utilidades, enums, tipos
export * from "@mapbox/index";       // Visor Mapbox
```

---

## 2. Arquitectura general

> **[EXAMPLES Â§3 + Â§13]** El flujo de este diagrama se traduce directamente en `EXAMPLES.md Â§3 InicializaciÃ³n del mapa` (cÃ³mo se configura cada nodo) y `EXAMPLES.md Â§13 Ejemplo mÃ­nimo funcional completo` (el patrÃ³n mÃ¡s corto posible con todos los nodos presentes). La guÃ­a de agentes en `EXAMPLES.md Â§15b` incluye una receta paso a paso usando este mismo flujo.

```
GoMapLoader  â”€â”€(factory estÃ¡tica async)â”€â”€>  GoMap
                                              â”‚
              â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
              â”‚                               â”‚
       GoInteractions                 Map<id, GOStagingMap>
       (compartido entre todos        â”‚
        los mapas)                    â”œâ”€â”€ OlMap (OpenLayers core)
                                      â”œâ”€â”€ GoView
                                      â”œâ”€â”€ Map<layerId, GOLayer>
                                      â”œâ”€â”€ GoOwnToc
                                      â”œâ”€â”€ UIDisplay
                                      â””â”€â”€ MapLayerPreviewer
```

**Flujo de creaciÃ³n:**

1. `GoMapLoader.loadGoMap(config)` â€” configura `GoStore`, autentica via `GoAuth`, construye un `GoMap`.
2. `GoMap` â€” fachada principal. Gestiona un `Map<string, GOStagingMap>` (uno por ID de mapa).
3. `GOStagingMap` â€” contenedor interno por instancia de mapa: almacena el `OlMap` raw, `GoView`, todas las capas `GOLayer`, `GoLayerInteractions`, `GoOwnToc`, `UIDisplay`, `MapLayerPreviewer`.
4. `GoInteractions` â€” gestor de herramientas interactivas, compartido, indexado por `(idMap, idInteraction)`.

---

## 3. MÃ³dulo Config

> **[EXAMPLES Â§3a]** La URL de entorno (`GO_CONFIG.GO_AIGUA_WEB`, etc.) es la que se pasa como `enviroment` en `MapConfigLoader` dentro de `loadScripts`. Ver `EXAMPLES.md Â§3a loadScripts` para el contexto completo de uso.

**Archivo:** `src/goapi/Config/index.ts`

```typescript
export const GO_CONFIG = {
  // URLs de entornos
  GO_AIGUA_WEB:       "https://gateway-proxy-gis-devtools.go-aigua.com",
  GO_AIGUA_PRE:       "https://gateway-proxy-dev.go-aigua.com",
  GO_AIGUA_DEV_GIS:   "https://gateway-proxy-dev-gis.go-aigua.com",
  GOOGLE_MAPS_API:    "https://maps.googleapis.com/maps/api/geocode/json",
  SVG_URL:            "https://goaiguagisapi.blob.core.windows.net/go-gis-api-dev/fw/svg/",

  REGION_ENVIROMENTS: {
    EUROPE: "https://gateway-proxy-gis-devtools.go-aigua.com"
  },

  SERVICES: {
    ICON_GENERATOR:            "/icongenerator-service/api/v1/icons/generator",
    ICON_GENERATOR_WEBGL:      "/icongenerator-service/api/v1/icons/generator_webgl",
    ICON_GENERATOR_SPRITE:     "/icongenerator-service/api/v1/icons/sprite_map",
    ICON_GENERATOR_DEFAULT_URL: "https://goaiguagisapi.blob.core.windows.net/..."
  },

  PALETTE_SECTOR_COLORS:      [ /* 15 colores HEX */ ],
  PALETTE_RGBA_SECTOR_COLORS: [ /* 15 colores RGBA */ ],
}
```

---

## 4. MÃ³dulo Map

> **[EXAMPLES Â§3 + Â§4â€“Â§9 + Â§12â€“Â§13]** Este mÃ³dulo es la base de todos los ejemplos:
> - `GoMapLoader` â†’ `EXAMPLES.md Â§3a loadScripts`
> - `GoMap` (mÃ©todos de vista) â†’ `EXAMPLES.md Â§3c optionView`
> - `GoMap` (mÃ©todos de capa) â†’ `EXAMPLES.md Â§4 Patrones de capas`
> - `GoMap` (mÃ©todos de interacciÃ³n/eventos) â†’ `EXAMPLES.md Â§6` y `Â§7`
> - `GoMap.setLanguage` / `setThemeAllMaps` â†’ `EXAMPLES.md Â§3f translations`
> - `GOStagingMap.getLayer(id)` â†’ base de `EXAMPLES.md Â§6bâ€“6k`
> - `GOStagingMap.getOwnToc()` â†’ `EXAMPLES.md Â§9 TOC dinÃ¡mico`
> - `map.createGeoJson()` / descarga â†’ `EXAMPLES.md Â§12 Descarga de capas`

**Directorio:** `src/goapi/Map/`

| Archivo | Responsabilidad |
|---------|----------------|
| `GoMapLoader.ts` | Factory estÃ¡tica: autenticaciÃ³n + creaciÃ³n del `GoMap` |
| `GoMap.ts` | Fachada principal de usuario |
| `GoStagingMap.ts` | Estado interno por instancia de mapa |
| `GoView.ts` | Vista extendida de OpenLayers |
| `GoCapabilities.ts` | InterrogaciÃ³n de capacidades WMS |

---

### `GoMapLoader` (`GoMapLoader.ts`)

> **[FILES L2609â€“L3331]** CÃ³digo fuente completo en `go-gis-VERBATIM.md` lÃ­neas 2609â€“3331 (`src/goapi/Map/GoMapLoader.ts`).

```typescript
/** Argumento de GoMapLoader.loadGoMap() */
interface MapConfigLoader {
  mapConfig:            configOptions; // REQUIRED â€” JSON completo de configuraciÃ³n
  enviroment:           string;        // REQUIRED â€” URL base del backend (ej: "https://my-backend.com")
  userToken?:           string;        // opcional â€” Bearer token para peticiones autenticadas
  useBackendFeatures?:  boolean;       // opcional, default: true â€” habilita capas que requieren backend
  isExample?:           boolean;       // opcional, default: false â€” modo demo/ejemplo
}

class GoMapLoader {
  /** Propiedades estÃ¡ticas de diagnÃ³stico */
  static linkArray: Array<string>;     // Lista de CSS/JS inyectados
  static dataBack:  Array<any>;        // Cache de respuestas del backend

  /**
   * Factory principal. Autentica, configura GoStore y devuelve un GoMap listo.
   * @param config - ConfiguraciÃ³n completa del mapa
   * @returns Promise<GoMap>
   */
  static async loadGoMap(config: MapConfigLoader): Promise<GoMap>
}
```

**Responsabilidades:**
- Detecta localhost â†’ sustituye capas MapBox por XYZ como fallback.
- Negocia autenticaciÃ³n (`GoAuth.getGeoServerAuth`).
- Despacha estado global en `GoStore`.
- Construye y devuelve el `GoMap`.

---

### `GoMap` (`GoMap.ts`)

> **[FILES L1374â€“L2608]** CÃ³digo fuente completo en `go-gis-VERBATIM.md` lÃ­neas 1374â€“2608 (`src/goapi/Map/GoMap.ts`).

Fachada principal. Todos los mÃ©todos estÃ¡n disponibles en la instancia devuelta por `GoMapLoader.loadGoMap`.

#### MÃ©todos de Mapa

| MÃ©todo | Firma completa | DescripciÃ³n |
|--------|---------------|-------------|
| `addMap` | `(options: mapOptionsBasic, interactions: interactions\|null, mapOpts: GOMapOptions): void` | Crea un nuevo mapa OL + vista + GOStagingMap |
| `unmountMap` | `(mapId: string): void` | Destruye el mapa y limpia todos sus recursos (memory leak prevention) |
| `getMap` | `(idMap: string): OlMap \| undefined` | Devuelve el `OlMap` (OpenLayers nativo) |
| `getAllStaging` | `(): Map<string, GOStagingMap>` | Todos los staging maps |
| `getStaging` | `(idMap: string): GOStagingMap \| undefined` | Un staging map individual |
| `refreshMap` | `(idMap: string): void` | Fuerza `renderSync()` |
| `createProjection` | `(options: projOptions): void` | Registra una proyecciÃ³n Proj4 personalizada |
| `getState` | `(): GoState` | Proxy de `GoStore.getState()` |

#### MÃ©todos de Vista

| MÃ©todo | Firma completa | DescripciÃ³n |
|--------|---------------|-------------|
| `getCenter` | `(idMap: string): Coordinate \| undefined` | Centro actual en la proyecciÃ³n activa |
| `setCenter` | `(idMap: string, center: [number, number]): void` | Establece el centro sin animaciÃ³n |
| `getZoom` | `(idMap: string): number \| undefined` | Nivel de zoom actual |
| `setZoom` | `(idMap: string, zoom: number): void` | Establece el zoom sin animaciÃ³n |
| `goToExtent` | `(idMap: string, extent: Extent): void` | Ajusta vista a un extent (bounding box) |
| `flyToCenter` | `(idMap: string, centerTo: [number, number], duration?: number, zoom?: number): void` | AnimaciÃ³n de desplazamiento suave |
| `getProjection` | `(idMap: string): string` | Retorna cÃ³digo EPSG ej: `"EPSG:4326"` |
| `getView` | `(idMap: string): OL.View` | Objeto `View` de OpenLayers |
| `bindViews` | `(refView: string, viewsToBind?: Array<string>): void` | Sincroniza la vista de mÃºltiples mapas (idMap) |
| `unbindAllViews` | `(option: any): void` | Desvincula todas las vistas sincronizadas |

#### MÃ©todos de Capa

| MÃ©todo | Firma completa | DescripciÃ³n |
|--------|---------------|-------------|
| `addLayer` | `(idMap: string, layerOpts: layerOptions): GOLayer \| undefined` | AÃ±ade una capa al mapa |
| `removeLayer` | `(idMap: string, idLayer: string): void` | Elimina y limpia una capa |
| `refreshLayer` | `(idMap: string, idLayer?: Array<string>): void` | Fuerza recarga de la fuente WFS/WMS |
| `getLayers` | `(idMap: string): Map<string, GOLayer> \| undefined` | Todas las capas indexadas por ID |
| `injectDataGeoJson` | `(idMap: string, idLayer: string, clear: boolean, data: any): void` | Inyecta features GeoJSON en una capa GO_GEOJSON_LAYER en runtime |
| `existLayer` | `(idMap: string, idLayer: string): boolean \| undefined` | Comprueba si una capa con ese ID existe en el mapa |

#### MÃ©todos de InteracciÃ³n y Eventos

| MÃ©todo | Firma completa | DescripciÃ³n |
|--------|---------------|-------------|
| `getInteractions` | `(): GoInteractions` | Instancia compartida de interacciones |
| `addInteractions` | `(id: string, interactions: interactions): void` | Agrega interacciones a un mapa concreto |
| `onMap` | `(idMap: string, type: typeOn, callback: Function): void` | Suscribe a un evento nativo de OpenLayers (usa enum `typeOn`) |
| `emit` | `(eventName: string, data: any): void` | Emite un evento (GOLayerEvents/GOMapEvents) |
| `on` | `(eventName: string, callback: Function): void` | Suscribe a un evento de la librerÃ­a |
| `once` | `(eventName: string, callback: Function): void` | SuscripciÃ³n de un solo disparo |
| `off` | `(eventName?: string, callback?: Function): void` | Desuscribe un evento |

#### MÃ©todos de Idioma y Tema

| MÃ©todo | Firma completa | DescripciÃ³n |
|--------|---------------|-------------|
| `setLanguage` | `(lang: SupportedLangs): void` | Cambia idioma. `SupportedLangs = "en"\|"es"\|"ca"\|"fr"\|"ro"\|"it"` |
| `getLanguage` | `(): SupportedLangs` | Idioma actual |
| `setThemeAllMaps` | `(theme: themes): void` | Aplica tema. `themes.DARK = "DARK"`, `themes.LIGHT = "LIGHT"` |
| `getTheme` | `(): themes` | Tema actual |

---

### `GOStagingMap` (`GoStagingMap.ts`)

> **[FILES L3332â€“L4129]** CÃ³digo fuente completo en `go-gis-VERBATIM.md` lÃ­neas 3332â€“4129 (`src/goapi/Map/GoStagingMap.ts`).

Contenedor de estado interno por instancia de mapa.

| Propiedad | Tipo | DescripciÃ³n |
|-----------|------|-------------|
| `map` | `OlMap` | Mapa OpenLayers raw |
| `view` | `GoView` | Vista extendida |
| `layers` | `Map<string, GOLayer \| BaseLayer>` | Capas indexadas por ID |
| `filters` | `Map<string, string>` | Filtros CQL por capa |
| `layerInteractions` | `Map<string, GoLayerInteractions>` | Interacciones por capa |
| `ownToc` | `GoOwnToc` | Componente TOC |
| `mapLayerPreviewer` | `MapLayerPreviewer` | Previewer de capas |
| `uiDisplay` | `UIDisplay` | Controles UI del mapa |

**MÃ©todos:**

| MÃ©todo | DescripciÃ³n |
|--------|-------------|
| `getLayer(id)` | Obtiene una capa por ID |
| `getOwnToc()` | Obtiene el componente TOC |
| `addOrUpdateTranslations(translations)` | AÃ±ade/actualiza traducciones en runtime |
| `unmount()` | Limpieza completa / prevenciÃ³n de memory leaks |

---

### `GoView` (`GoView.ts`)

Extiende la clase `View` de OpenLayers.

```typescript
class GoView extends View {
  mapViewLinked: Array<string>; // IDs de mapas con vistas sincronizadas

  getMapsLinked(): Array<string>
  getid(): string
  setId(id: string): void
}
```

---

### `GoCapabilities` (`GoCapabilities.ts`)

```typescript
class GoCapabilities {
  getLayerExtent(layer: GOVectorLayer): CancelablePromise<unknown>
  getCapabilities(layer: GOVectorLayer): CancelablePromise<unknown>
}
```

---

## 5. Sistema de Capas (Layers)

> **[EXAMPLES Â§4 + Â§5]** Cada clase de capa tiene su patrÃ³n JSON+JS en `EXAMPLES.md Â§4`:
> - `GOVectorLayer` â†’ `EXAMPLES Â§4b`  |  `GOWMSLayer` â†’ `EXAMPLES Â§4c`  |  `GOWFSLayer` â†’ `EXAMPLES Â§4d`
> - `GOGeoJsonLayer` â†’ `EXAMPLES Â§4e`  |  `GOClusterLayer` â†’ `EXAMPLES Â§4f`  |  `GoHeatMapLayer` â†’ `EXAMPLES Â§4g`
> - `GoHexGridLayer` â†’ `EXAMPLES Â§4h`  |  `GoIOTLayer` â†’ `EXAMPLES Â§4i`  |  `GoTruckLayer` â†’ `EXAMPLES Â§4j`
> - `GoWebGlLayer` â†’ `EXAMPLES Â§4k`  |  `GOVectorTiles` â†’ `EXAMPLES Â§4l`  |  `GoRemoteSensing` â†’ `EXAMPLES Â§4m`
> - Mapas base (`GO_MAPBOX`, `GO_XYZ`, `GO_ESRI_BASEMAP`) â†’ `EXAMPLES Â§4a`
>
> Los estilos (`ISVGIcon`, `styleFromUser`, `styleGeojson`, `sldStyle`) â†’ `EXAMPLES.md Â§5`.
> Los mÃ©todos de runtime de `GOVectorLayer` (`addFilter`, `addFilterNoCQL`) â†’ `EXAMPLES.md Â§6eâ€“6f`.

**Directorio:** `src/goapi/Layers/`

---

### JerarquÃ­a de clases

```
GOLayer (abstracta)
â”‚
â”œâ”€â”€ GOStyledLayer (abstracta)
â”‚   â”œâ”€â”€ GOVectorLayer        â€” WFS/GeoServer vector, estrategia bbox
â”‚   â”œâ”€â”€ GOGeoJsonLayer       â€” GeoJSON estÃ¡tico, URL o data inline
â”‚   â”œâ”€â”€ GOWFSLayer           â€” OGC WFS o ESRI FeatureServer
â”‚   â”œâ”€â”€ GOClusterLayer       â€” Vector con clÃºster
â”‚   â”œâ”€â”€ GoHeatMapLayer       â€” Mapa de calor (OpenLayers)
â”‚   â”œâ”€â”€ GoHexGridLayer       â€” AgregaciÃ³n hexagonal
â”‚   â”œâ”€â”€ GoHexGrid
â”‚   â”œâ”€â”€ GoIOTLayer           â€” IoT/tiempo real con auto-recarga
â”‚   â”œâ”€â”€ GoTruckLayer         â€” Seguimiento de vehÃ­culos
â”‚   â”œâ”€â”€ GoWebGlLayer         â€” Puntos con renderizado WebGL
â”‚   â””â”€â”€ GOVectorTiles / GOVectorTilesOwn  â€” Tiles vectoriales (MVT)
â”‚
â”œâ”€â”€ GOWMSLayer               â€” OGC WMS TileLayer
â”œâ”€â”€ GOWMTSLayer              â€” WMTS TileLayer
â”œâ”€â”€ GoXYZ                    â€” XYZ/OSM tiles
â”œâ”€â”€ GoMapBoxLayer            â€” Capa base estilo MapBox
â”œâ”€â”€ GoEsriBaseMap            â€” Capa base ESRI
â”œâ”€â”€ GoRemoteSensing          â€” SatÃ©lite/raster con UI temporal
â”œâ”€â”€ GoVerticalRemoteSensing  â€” Variante RS vertical
â””â”€â”€ GoWaterLeakRemoteSensing â€” Variante RS detecciÃ³n fugas
```

---

### `GOLayer` â€” clase base abstracta

> **[FILES L4130â€“L5131]** CÃ³digo fuente completo en `go-gis-VERBATIM.md` lÃ­neas 4130â€“5131 (`src/goapi/Layers/GoLayer/index.ts`).

**Directorio:** `src/goapi/Layers/GoLayer/`

```typescript
abstract class GOLayer {
  // Propiedades protegidas
  protected layerId:    string;
  protected layerAlias: string;
  protected layerName:  string;
  protected layerType:  string;
  protected layerUrl:   string;
  protected baseLayer:  boolean;
  protected layer;                  // primitiva OL subyacente
  protected layerStyle: GoStyle;

  // Propiedades pÃºblicas
  public olMap:              OlMap;
  public useBboxFilter:      boolean;
  public cqlFilterIsCanceled: boolean;

  // Abstractos â€” deben implementarse en subclases
  abstract getLayer(): VectorLayer | TileLayer | ...
  abstract type(): string
  abstract canFilter(): boolean

  // API comÃºn
  getLayerUrl(): string
  validateLayerConfig(opts): void
  cancelActiveRequests(): void
  unmount(): void
  emit(event: string, data: any): void
  on(event: string, callback: Function): void
}
```

---

### `GOStyledLayer`

Extiende `GOLayer` con gestiÃ³n de estilos SLD, iconos SVG y estilos condicionales.

```typescript
abstract class GOStyledLayer extends GOLayer {
  public style:                Style;
  public mapStyle:             Map<string, Map<string, Style | styleOptions | ...>>;
  public mapStyleConditions:   Map<string, Style>;
  public sldLayer;
  public sldObject;
  public styleHasChanged:      boolean;

  abstract changeStyle(style: styleFromUser): void

  hasLabelStyle(): boolean
  // + mÃºltiples helpers de estilo internos
}
```

---

### Tipos de capa disponibles

#### `GOVectorLayer` â€” `src/goapi/Layers/GoVectorLayer/`

> **[FILES L5132â€“L6096]** CÃ³digo fuente en `go-gis-VERBATIM.md` lÃ­neas 5132â€“6096 (`src/goapi/Layers/GoVectorLayer/index.ts`).

Capa vectorial WFS/GeoServer con estrategia bbox o all-features.

| Propiedad clave | Tipo | DescripciÃ³n |
|-----------------|------|-------------|
| `layerProjection` | `string` | EPSG fuente |
| `toProjection` | `string` | EPSG destino |
| `useBboxFilter` | `boolean` | Carga solo bbox visible |
| `geomName` | `string` | Nombre del campo geometrÃ­a |

**MÃ©todos:**

| MÃ©todo | DescripciÃ³n |
|--------|-------------|
| `addFilter(cql, reload?)` | Aplica filtro CQL al servidor |
| `removeFilter()` | Elimina el filtro CQL |
| `addFilterNoCQL(filter: GoFilter)` | Filtro client-side sin llamada al servidor |
| `removeFilterNoCQL()` | Elimina filtro client-side |
| `isLoadedData(timeInterval, limit)` | Promise de polling hasta que los datos se cargan |
| `changeStyle(style: styleFromUser)` | Cambia el estilo en tiempo real |
| `getFeatures()` | Devuelve las features cargadas actualmente |

---

#### `GOGeoJsonLayer` â€” `src/goapi/Layers/GoGeoJsonLayer/`

> **[FILES L7311â€“L7915]** CÃ³digo fuente en `go-gis-VERBATIM.md` lÃ­neas 7311â€“7915 (`src/goapi/Layers/GoGeoJsonLayer/index.ts`).

Capa GeoJSON estÃ¡tica (URL o datos inline).

- Acepta `layerOpts.url` **O** `layerOpts.data` (no los dos a la vez)
- Soporta reproyecciÃ³n (`layerProjection` â†’ `toProjection`)
- Usa `VectorImageLayer` por defecto; `VectorLayer` si `disableImageEffect: true`

| MÃ©todo | DescripciÃ³n |
|--------|-------------|
| `addFeatures(geojson)` | AÃ±ade features al layer |
| `refresh()` | Recarga / limpia features |

---

#### `GOWFSLayer` â€” `src/goapi/Layers/GoWFS/`

> **[FILES L8215â€“L9012]** CÃ³digo fuente en `go-gis-VERBATIM.md` lÃ­neas 8215â€“9012 (`src/goapi/Layers/GoWFS/index.ts`).

OGC WFS o ESRI FeatureServer.

| Propiedad | Tipo | DescripciÃ³n |
|-----------|------|-------------|
| `esri` | `boolean` | Usar formato EsriJSON + auth de token |
| `useBboxFilter` | `boolean` | Estrategia bbox |
| `WFStoken` | `string` | Token ESRI |
| `user` / `password` | `string` | Credenciales bÃ¡sicas |

---

#### `GOWMSLayer` â€” `src/goapi/Layers/GOWMS/`

> **[FILES L7916â€“L8214]** CÃ³digo fuente en `go-gis-VERBATIM.md` lÃ­neas 7916â€“8214 (`src/goapi/Layers/GOWMS/index.ts`).

Capa tile WMS.

- Usa `AuthTileWMS` (extensiÃ³n personalizada de OL TileWMS con auth)
- Soporta `geoparams`, `sldStyle`, `version`, `minResolution`, `maxResolution`
- `type()` â†’ `"GoWMS"` ; `canFilter()` â†’ `false`

---

#### `GOClusterLayer` â€” `src/goapi/Layers/GoClusterLayer/`

> **[FILES L6097â€“L7310]** CÃ³digo fuente en `go-gis-VERBATIM.md` lÃ­neas 6097â€“7310 (`src/goapi/Layers/GoClusterLayer/index.ts`).

Capa clÃºster con tres estilos configurables: icono individual, clÃºster individual, clÃºster mÃºltiple.

| MÃ©todo | DescripciÃ³n |
|--------|-------------|
| `getFeaturesAtPixel(pixel)` | Features en el pixel clicado |
| `changeStyle(style)` | Cambia el estilo del clÃºster |

---

### Enum `GOLayerType`

> **[FILES L9104â€“L9685]** DefiniciÃ³n completa del enum en `go-gis-VERBATIM.md` lÃ­neas 9104â€“9685 (`src/goapi/Utils/constants/index.ts`).

Los valores del enum son strings idÃ©nticos al nombre del miembro (usen siempre el enum, nunca strings literales):

```typescript
enum GOLayerType {
  GO_HEXGRID_LAYER   = "GO_HEXGRID_LAYER",
  GO_HEXGRID         = "GO_HEXGRID",
  GO_HEATMAP_LAYER   = "GO_HEATMAP_LAYER",
  GO_VECTOR_LAYER    = "GO_VECTOR_LAYER",
  GO_IOT_LAYER       = "GO_IOT_LAYER",
  OSM                = "OSM",
  GO_WMS_LAYER       = "GO_WMS_LAYER",
  GO_WMTS_LAYER      = "GO_WMTS_LAYER",
  GO_CLUSTER_LAYER   = "GO_CLUSTER_LAYER",
  GO_GEOJSON_LAYER   = "GO_GEOJSON_LAYER",
  GO_MAPBOX          = "GO_MAPBOX",
  GO_XYZ             = "GO_XYZ",
  GO_VECTOR_TILE     = "GO_VECTOR_TILE",
  GO_WFS_LAYER       = "GO_WFS_LAYER",
  GO_REMOTE_SENSING  = "GO_REMOTE_SENSING",
  GO_VECTOR_TILE_OWN = "GO_VECTOR_TILE_OWN",
  GO_ESRI_BASEMAP    = "GO_ESRI_BASEMAP",
  GO_WEBGL_LAYER     = "GO_WEBGL_LAYER",
  GO_TRUCK_LAYER     = "GO_TRUCK_LAYER"
}
```

---

### Tipo `layerOptions` â€” ConfiguraciÃ³n universal de capa

> **[FILES L9104â€“L9685]** DefiniciÃ³n completa del tipo `layerOptions` y todos sus campos en `go-gis-VERBATIM.md` lÃ­neas 9104â€“9685 (`src/goapi/Utils/constants/index.ts`).

> Campos marcados con **\*** son **obligatorios**. El resto son opcionales (`?`).

```typescript
type layerOptions = {
  // â”€â”€ Campos OBLIGATORIOS â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  id:           string;              // * Identificador Ãºnico de la capa
  alias:        string;              // * Nombre visible en TOC (soporta {{__key__}})
  layerType:    GOLayerType;         // * Tipo de capa (enum)

  // â”€â”€ Campos de conexiÃ³n â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  url?:              string;         // URL base del servicio (GeoServer/ESRI/â€¦)
  layerName?:        string;         // Nombre de la capa en el servidor (WMS/WFS)
  geoparams?:        string;         // ParÃ¡metros extra para URL GeoServer
  layerProjection?:  string;         // EPSG fuente, ej: "4326"
  toProjection?:     string;         // EPSG destino  ej: "3857"
  version?:          string;         // VersiÃ³n del protocolo WMS

  // â”€â”€ Visibilidad y z-order â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  defaultVisibility?: boolean;       // Visibilidad inicial (default: true)
  isBaseLayer?:       boolean;       // Es capa base (mutuamente excluyentes en TOC)
  opacity?:           number;        // 0.0 â€“ 1.0 (default: 1)
  zIndex?:            number;        // Orden de renderizado
  minResolution?:     number;        // ResoluciÃ³n mÃ­nima OL (mayor zoom = menor resoluciÃ³n)
  maxResolution?:     number;        // ResoluciÃ³n mÃ¡xima OL

  // â”€â”€ TOC / Leyenda â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  toc?:           boolean;           // Mostrar en TOC (default: true)
  legend?:        boolean;           // Mostrar leyenda
  urlFolder?:     string;            // Carpeta TOC, ej: "Assets/" o "Red/Assets/"
  toc_link?:      string[];          // IDs de capas que se activan junto a esta

  // â”€â”€ Filtros â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  filter?:           string;         // Filtro CQL inicial
  filterOverload?:   string;         // Filtro CQL que sobreescribe al anterior
  useBboxFilter?:    boolean;        // Carga solo features del bbox visible

  // â”€â”€ Estilos â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  sldStyle?:                    styleOptions;          // Estilo SLD del servidor
  styleSVG?:                    ISVGIcon;              // Icono SVG para VectorLayer
  styleSVGCluster?:             ISVGIcon;              // Icono del grupo cluster
  styleSVGClusterInd?:          ISVGIcon;              // Icono cluster de elemento individual
  styleSVGConditional?:         styleSVG;              // Estilos SVG condicionales por campo
  styleSVGWebGLConditional?:    styleSVGWebGLConditional; // Estilos WebGL condicionales
  styleGeojson?:                styleGeojson;          // Estilos GeoJSON (unique/classified)
  styleHexBin?:                 hexGridColor;          // Colores de hexgrid
  styleCluster?:                Array<clusterMode>;    // Config visual del clÃºster
  styleWFS?:                    styleWFS;              // Estilo WFS bÃ¡sico {radius,fillcolor,strokecolor}
  style?:                       any;                   // Estilo genÃ©rico (Mapbox style ID)
  disableImageEffect?:          boolean;               // Usar VectorLayer en vez de VectorImageLayer (GeoJSON)
  visibilityAnnotationDef?:     boolean;               // Mostrar anotaciones por defecto
  offsetXAnnotationDef?:        number;
  offsetYAnnotationDef?:        number;
  fontSizeAnnotationDef?:       string;
  fontColorAnnotationDef?:      string;
  showTextOnUnique?:            boolean;               // Mostrar texto en capas Cluster con symbol Ãºnico

  // â”€â”€ Cluster â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  distance?:              number;    // Distancia de clusterizaciÃ³n en pÃ­xeles
  clusterField?:          string;    // Campo para agrupar en clÃºster
  maxResolutionCluster?:  number;    // ResoluciÃ³n mÃ¡xima donde se muestra el clÃºster

  // â”€â”€ HeatMap â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  radiusHeatMap?:  number;           // Radio del heatmap
  blurHeatMap?:    number;           // Desenfoque del heatmap
  fieldHeatMap?:   string;           // Campo de peso para el heatmap

  // â”€â”€ HexGrid â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  hexSize?:            number;
  hexGridResolution?:  hexGridResolution;

  // â”€â”€ GeoJSON inline â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  data?:  any;                       // FeatureCollection GeoJSON directamente

  // â”€â”€ IOT / Tiempo real â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  reloadFrequency?:   number;        // Intervalo de recarga en ms
  labelNameIOT?:      string;        // Campo para etiqueta en capa IOT
  iotUrl?:            string;
  iotToken?:          string;
  iotVersion?:        string;

  // â”€â”€ AutenticaciÃ³n ESRI / WFS â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  esri?:      boolean;               // Usar formato ESRI FeatureServer
  user?:      string;                // Usuario autenticaciÃ³n bÃ¡sica WFS
  password?:  string;                // ContraseÃ±a autenticaciÃ³n bÃ¡sica WFS
  WFStoken?:  string;                // Token ESRI

  // â”€â”€ ESRI Basemap â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  idEsri?:     string;               // ID del basemap ESRI
  esriApiKey?: string;               // API Key de ArcGIS

  // â”€â”€ Mapbox â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  token?:      string;               // Token de Mapbox
  watermark?:  string;               // Texto de marca de agua

  // â”€â”€ Vector Tile â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  vt_type?:  string;                 // Tipo de tile vectorial

  // â”€â”€ GeometrÃ­a â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  geometryType?:   LayerGeometryType; // Override del tipo de geometrÃ­a
  geometryField?:  string;            // Nombre del campo de geometrÃ­a
  envelope?:       boolean;          // Usar envelope en WFS (optimizaciÃ³n)
  propertyName?:   Array<string>;    // Subconjunto de atributos a traer del WFS

  // â”€â”€ Remote Sensing â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  remoteSensingType?:  string;       // "vertical" | "waterLeak"
  vrsColors?:          Array<string>; // Colores para RS vertical
  rsClientID?:         string;
  plotsIds?:           Array<string>;
  rsInitialDate?:      number;       // Unix timestamp
  rsEndDate?:          number;       // Unix timestamp

  // â”€â”€ Truck / VehÃ­culos â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  controlPoints?:       Array<controlPoints>;
  listener?:            Array<listenerControl>;
  timeNoSignal?:        number;
  timeWorking?:         number;
  timeNexusPosition?:   number;
  timeReSyncRoute?:     number;

  // â”€â”€ Interacciones por capa â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  interactions?:  LayerInteractions; // Hover por capa {active, idInteraction, ...}

  // â”€â”€ Origen de datos â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
  sourceType?:   GoLayerSourceType;  // "GEOSERVER" | "GEOJSON"
  originData?:   string;
};
```

---

## 6. MÃ³dulo Interactions

> **[FILES L9969â€“L13490]** ImplementaciÃ³n completa de `GoInteractions` en `go-gis-VERBATIM.md` lÃ­neas 9969â€“13490 (`src/goapi/Interactions/index.ts`). Sub-mÃ³dulos: `Features` [FILES L9035â€“L9064], `geoTools/buffer` [FILES L9697â€“L9968], `getFeaturesByLayer` [FILES L9065â€“L9068].

> **[EXAMPLES Â§6 + Â§8 + Â§14]** Cada sub-mÃ³dulo tiene su secciÃ³n de ejemplo:
> - `Features` (GoHighlight, GoHover, GoEditFeatures) â†’ `EXAMPLES Â§6b`, `Â§6c`, `Â§6g`, `Â§6h`
> - `geoTools` (GoBuffer, GoIntersect) â†’ `EXAMPLES Â§6d`
> - `Selection` (GoSelectByPolygon) â†’ `EXAMPLES Â§6j`
> - `Analysis` (GoMeasure, GoDensityTool) â†’ `EXAMPLES Â§6k`
> - `ExternalsConnections` (GoGeocodingHERE, GoRouting) â†’ `EXAMPLES Â§8a`, `Â§8b`
> - `Hydraulics` (GoCloseTools, GoUpDown) â†’ catÃ¡logo `EXAMPLES Â§14`: `ex_close_tool`, `ex_upstream`, `ex_downstream`
> - `Views` (GoOverlay, GoPrint) â†’ catÃ¡logo `EXAMPLES Â§14`: `ex_popup`, `ex_print_pdf`
>
> La firma completa de todos los mÃ©todos de `GoInteractions` estÃ¡ en `Â§6 Firmas de mÃ©todos clave` (este documento) y correlacionada en `EXAMPLES.md Â§15c` (tabla de campos obligatorios).

**Directorio:** `src/goapi/Interactions/`

```typescript
// Exportaciones con namespace
export * as Analysis             from "./Analysis";
export * as ExternalsConnections from "./ExternalsConnections";
export * as Features             from "./Features";
export * as Hydraulics           from "./Hydraulics";
export * as Selection            from "./Selection";
export * as Views                from "./Views";
export * as Utils                from "./utils";
```

---

### Analysis

**Archivo:** `src/goapi/Interactions/Analysis/index.ts`

Re-exporta los mÃ³dulos de anÃ¡lisis de `geoTools` mÃ¡s las herramientas de densidad y mediciÃ³n propias.

| Export | Clase/FunciÃ³n | DescripciÃ³n |
|--------|--------------|-------------|
| `GoIntersect` | Clase | IntersecciÃ³n geomÃ©trica (re-exportada de `geoTools/Intersect`) |
| `GoNearData` | Clase | Elemento mÃ¡s cercano (re-exportada de `geoTools/nearData`) |
| `GoDensityTool` | Clase base | Herramienta base de densidad (HeatMap / HexGrid / IsolÃ­neas) |
| `GoHeatMapTool` | Clase | Densidad tipo mapa de calor |
| `GoHexgridTool` | Clase | Densidad en rejilla hexagonal |
| `densityLayer.methods` | Funciones helper | MÃ©todos de ciclo de vida de la capa de densidad |
| `GoMeasure` | Clase | MediciÃ³n lineal o poligonal con tooltip interactivo |
| `TurfTools` | Objeto utilidades | Funciones de geoprocesamiento basadas en Turf.js |

---

### Features

**Archivo:** `src/goapi/Interactions/Features/index.ts`

| Export | DescripciÃ³n |
|--------|-------------|
| `GoEditFeatures` | Clase: dibujar/editar/borrar features vectoriales (WFS-T) |
| `GoHighlight` | Clase: resaltar un conjunto de features con estilo personalizado |
| `GoHover` | Clase: resaltado en hover + callback por capa |
| `GoHoverFeature` | Clase: handle de hover para una feature individual |
| `addHighlight` | Helper funciÃ³n: instancia y aÃ±ade un `GoHighlight` al mapa indexado por `idHighlight` |
| `hasHighlight` | Helper funciÃ³n: comprueba si existe un highlight con `idHighlight` dado |
| `getHighlight` | Helper funciÃ³n: devuelve la instancia `GoHighlight` por `idHighlight` |
| `changeStyleOfHighlight` | Helper funciÃ³n: cambia el estilo de un highlight existente |
| `changeHighlightZIndex` | Helper funciÃ³n: cambia el z-index de la capa highlight |
| `restoreHighlightStyles` | Helper funciÃ³n: restaura los estilos originales del highlight |
| `removeHighlightIfExist` | Helper funciÃ³n: elimina el highlight si existe (no lanza error si no existe) |
| `removeHighlight` | Helper funciÃ³n: elimina un highlight por `idHighlight` |
| `mouseHover` | Helper funciÃ³n: instancia y activa un `GoHover` en una capa |
| `removeHover` | Helper funciÃ³n: desactiva y elimina un `GoHover` por `idHover` |
| `hasHoverFeature` | Helper funciÃ³n: comprueba si existe un hover con `idHover` dado |
| `getLayerHover` | Helper funciÃ³n: devuelve la instancia `GoHover` por `idHover` |
| `hasGetFeature` | Helper funciÃ³n: comprueba si hay una feature en la posiciÃ³n actual del hover |
| `addTooltipHoverFeature` | Helper funciÃ³n: aÃ±ade tooltip visual al hover sobre features |
| `hasToolTipHover` | Helper funciÃ³n: comprueba si existe un tooltip asociado al hover |
| `getTooltipHoverFeature` | Helper funciÃ³n: obtiene el tooltip del hover |
| `removeTooltipHoverFeature` | Helper funciÃ³n: elimina el tooltip del hover |
| `changeHoverZIndex` | Helper funciÃ³n: cambia el z-index de la capa hover |
| `getAllFeaturesProperties` | Helper funciÃ³n: obtiene todas las propiedades de las features de una capa |
| `getFeaturesByLayer` / `GetFeaturesByLayer` | FunciÃ³n/clase: recupera features de una capa por nombre o CQL (ver Â§6 Firmas) |

---

#### Clase `GoHighlight`

Extiende `GOInteractionTool`. Crea una capa temporal `VectorImageLayer` con las features resaltadas.

```typescript
class GoHighlight extends GOInteractionTool {
  constructor(
    idHighlight: string,
    userFeatures: string | Array<Feature>,  // WKT string O array de features OL
    userStyle?: styleFromUser,
    textFunction?: Function,                // FunciÃ³n que devuelve Style[] con texto
    styleSVG?: ISVGIcon,
    svgIcon?: string,
    zIndex?: number                         // default: 99
  )
}
```

| MÃ©todo pÃºblico | Firma | DescripciÃ³n |
|----------------|-------|-------------|
| `getStyle()` | `(): Style` | Devuelve el `Style` de OL actualmente aplicado |
| `setStyle(userStyle?, styleSVG?, svgIcon?)` | `(userStyle?: styleFromUser, styleSVG?: ISVGIcon, svgIcon?: string): void` | Aplica un nuevo estilo al highlight |
| `addFeatures(features)` | `(features: Array<Feature>): void` | AÃ±ade features adicionales a la capa de highlight |
| `clearFeatures()` | `(): void` | Elimina todas las features del highlight (sin destruir la capa) |
| `getLayer()` | `(): VectorLayer \| VectorImageLayer` | Devuelve la capa OL subyacente |
| `getId()` | `(): string` | Devuelve el `idHighlight` |
| `unmount()` | `(): void` | Destruye la capa y limpia listeners (llamado automÃ¡ticamente por `removeHighlight`) |

**Flujo habitual:**
```typescript
// Via GoInteractions (recomendado)
map.getInteractions().addHighlight({
  idMap: "main", idHighlight: "hl1",
  features: featuresArray,
  style: { type: "ALL", stroke: { color: "#FF0000", width: 3 }, fill: { color: "rgba(255,0,0,0.2)" } }
});

// Acceso directo a la instancia para aÃ±adir/limpiar features
const hl = map.getInteractions().getHighlight({ idMap: "main", idHighlight: "hl1" });
hl.clearFeatures();
hl.addFeatures(newFeatures);

// Cambio de estilo en runtime
map.getInteractions().changeStyleOfHighlight({
  idMap: "main", idHighlight: "hl1",
  userStyle: { type: "ALL", stroke: { color: "#00FF00" } }
});
```

---

#### Clase `GoHover`

Extiende `GOInteractionTool`. Escucha `pointermove` en el mapa OL y resalta la feature bajo el cursor.

```typescript
class GoHover extends GOInteractionTool {
  constructor(
    idHover: string,
    olMap: OlMap,
    layer?: GOLayer | GOVectorLayer | GOGeoJsonLayer | VectorLayer | undefined,
    style?: styleFromUser,
    callback?: (feature: Feature<Geometry>) => void,
    styleSVG?: ISVGIcon,
    svgIcon?: string,
    zIndex?: number             // default: 101
  )
}
```

| MÃ©todo pÃºblico | Firma | DescripciÃ³n |
|----------------|-------|-------------|
| `getCallback(idHover)` | `(idHover: string): Function` | Devuelve la funciÃ³n callback registrada |
| `setCallback(idHover, callback)` | `(idHover: string, callback: (feature: Feature<Geometry>) => void): void` | Sustituye el callback en runtime |
| `removeActions()` | `(): void` | Desregistra los listeners `pointermove` del mapa OL |
| `cleanLayer()` | `(): void` | VacÃ­a la capa hover (quita el resaltado actual sin destruir el hover) |
| `getHoverLayer()` | `(): VectorLayer \| VectorImageLayer` | Devuelve la capa OL del hover |

**Flujo habitual:**
```typescript
// Activar hover sobre una capa especÃ­fica
map.getInteractions().mouseHover({
  idMap: "main", idHover: "hov1",
  layer: "pipes",                      // id de capa registrada en GOStagingMap
  callback: (feature) => console.log(feature.getProperties()),
  style: { type: "ALL", fill: { color: "rgba(255,200,0,0.5)" } },
  zindex: 100
});

// Hover global (sin capa especÃ­fica) â€” resalta cualquier feature del mapa
map.getInteractions().mouseHover({ idMap: "main", idHover: "hovGlobal" });

// Cambiar callback en runtime sin recrear el hover
const hov = map.getInteractions().getLayerHover({ idMap: "main", idHover: "hov1" });
hov.setCallback("hov1", (feature) => showPopup(feature));

// Limpiar resaltado actual (sin apagar el hover)
hov.cleanLayer();

// Eliminar hover completamente
map.getInteractions().removeHover({ idMap: "main", idHover: "hov1" });
```

---

---

### Selection

**Archivo:** `src/goapi/Interactions/Selection/index.ts`

| Export | DescripciÃ³n |
|--------|-------------|
| `GoSelectByPolygon` | Clase: gestiona la lÃ³gica de selecciÃ³n de features dentro de un polÃ­gono dibujado |
| `GoDrawSelectByPolygon` | Clase: activa el modo de dibujo para la selecciÃ³n poligonal en capas vectoriales |
| `GoDrawSelectByPolygonInCluster` | Clase: variante de selecciÃ³n poligonal para capas `GOClusterLayer` |

**MÃ©todos GoInteractions relacionados:** `addSelectByPolygon`, `removeSelectByPolygon`, `clearSelectByPolygon`, `getMultiExtent`

---

### ExternalsConnections

**Archivo:** `src/goapi/Interactions/ExternalsConnections/index.ts`

Re-exporta todos los conectores con servicios externos: geocodificaciÃ³n, catastro, DMD, WMS, IoT.

| Export | DescripciÃ³n |
|--------|-------------|
| `GoGeocoding` | GeocodificaciÃ³n Google (coordâ†’direcciÃ³n via `getDirectionFromCoordinates`, direcciÃ³nâ†’coord via `getCoordinatesFromDirection`) |
| `GoGeocodingHERE` | GeocodificaciÃ³n HERE Maps + autocomplete (`registerGeocodingHERE`, `suggestAddress`, `getCoordinatesFromDirectionHERE`) |
| `GoRouting` | Enrutamiento HERE Maps simple (`addRoutingHERE`) y multi-parada (`addMultiRoutingHERE`) |
| `GoNexusInfo` | Lectura de datos de sensores IoT Nexus |
| `GoWMSGetFeatureInfo` | WMS GetFeatureInfo â€” interrogaciÃ³n de features por clic en coordenadas | 
| `GoCadastre` | Acceso al servicio catastral (`createCadastre`, `getCadastre`, `hasCadastre`, `removeCadastre`) |
| `GoDMD` | GestiÃ³n de activos digitales DMD (`createDMD`, `getDMD`, `hasDMD`, `removeDMD`) |

---

### Hydraulics

**Archivo:** `src/goapi/Interactions/Hydraulics/index.ts`

| Export | DescripciÃ³n |
|--------|-------------|
| `GoCloseTools` | Clase: herramienta de cierre de red (tuberÃ­as + vÃ¡lvulas afectadas). Consume API hidrÃ¡ulica backend. |
| `GoUpDown` | Clase: trazado de red aguas arriba (`upstream`) o aguas abajo (`downstream`) desde un nodo. |
| `GoflowLinesMove` | Clase: lÃ­neas de flujo animadas sobre tuberÃ­as (solo geometrÃ­a `LineString`). Se activa via `addFlowLines`. |

---

### Views

**Archivo:** `src/goapi/Interactions/Views/index.ts`

| Export | DescripciÃ³n |
|--------|-------------|
| `GoOverlay` | Overlays/popups anclados a coordenadas del mapa |
| `GoRotation` | Control de rotaciÃ³n del mapa (via interfaz UI o programÃ¡tico) |
| `GoPrint` | ImpresiÃ³n/exportaciÃ³n del mapa a imagen o PDF |
| `GoFullScreen` | Toggle de pantalla completa; expone eventos `onFullScreenEnter` / `onFullScreenExit` |

---

### geoTools

**Directorio:** `src/goapi/Interactions/geoTools/`

Herramientas de geoprocesamiento disponibles via `GoInteractions`. Cada herramienta sigue el patrÃ³n `add*/has*/remove*`.

| Sub-herramienta | Clase exportada | DescripciÃ³n | MÃ©todos GoInteractions |
|----------------|-----------------|-------------|----------------------|
| Buffer | `GoBuffer` | Buffer geomÃ©trico (llama al backend) | `addBuffer`, `hasBuffer`, `removeBuffer` |
| Intersect | `GoIntersect` | IntersecciÃ³n geomÃ©trica entre dos capas/GeoJSON | `addIntersect`, `hasIntersect`, `removeIntersect` |
| Interpolation | `GOInterpolation` | InterpolaciÃ³n espacial de valores puntuales | `addInterpolation`, `hasInterpolation`, `removeInterpolation` |
| Isolines | `GOIsolines` | GeneraciÃ³n de isolÃ­neas / isobandas a partir de puntos | `addIsolines`, `hasIsolines`, `removeIsolines` |
| NearData | `GoNearData` | Elemento mÃ¡s cercano a una coordenada (radio + tipo) | `addNearData`, `hasNearData`, `removeNearData` |
| NearDataAttributes | `GoNearDataAttributes` | Atributos del elemento mÃ¡s cercano | `addNearDataAttributes`, `hasNearDataAttributes`, `removeNearDataAttributes` |
| OpticalFiber | `GoOpticalFiber` | AnÃ¡lisis de segmentos de fibra Ã³ptica | `addOpticalFiber`, `hasOpticalFiber`, `removeOpticalFiber` |
| PolygonSector | `GoPolygonSector` | GeneraciÃ³n de grid de sectores poligonales | `addPolygonSector`, `hasPolygonSector`, `removePolygonSector` |
| SectorColor | `GoSectorColor` | Coloreado automÃ¡tico de sectores por atributo | `addSectorColor`, `hasSectorColor`, `removeSectorColor` |
| Watershed | `GoWatershed` | Cuencas hidrogrÃ¡ficas (aguas arriba desde punto) | `addWatershed`, `hasWatershed`, `removeWatershed` |

---

### Firmas de mÃ©todos clave de GoInteractions

> Todos los mÃ©todos de `GoInteractions` se invocan a travÃ©s de la instancia devuelta por `map.getInteractions()`. Los parÃ¡metros son siempre un Ãºnico objeto destructurado `{ idMap, id*, ... }`.

---

#### Highlight

```typescript
// AÃ±adir/gestionar resaltado de features
addHighlight({ idMap, idHighlight, features, style?, textFunction?, styleSVG?, zIndex? }): GoHighlight
hasHighlight({ idMap, idHighlight }): boolean
getHighlight({ idMap, idHighlight }): GoHighlight
changeStyleOfHighlight({ idMap, idHighlight, userStyle?, styleSVG? }): Promise<GoHighlight | string>
changeHighlightZIndex({ idMap, idHighlight, zIndex }): void
restoreHighlightStyles({ idMap, idHighlight }): void
removeHighlightIfExist({ idMap, idHighlight }): void
removeHighlight({ idMap, idHighlight }): void
```

---

#### Hover

```typescript
// Activar/gestionar hover sobre features
mouseHover({ idMap, idHover, layer?, style?, callback?, styleSVG?, zindex? }): GoHover | void
removeHover({ idMap, idHover }): void
hasHoverFeature({ idMap, idHover }): boolean
getLayerHover({ idMap, idHover }): GoHover
addTooltipHoverFeature({ idMap, idHover, featuresGeom, style? }): void
hasToolTipHover({ idMap, idHover }): boolean
getTooltipHoverFeature({ idMap, idHover }): GoHoverFeature
removeTooltipHoverFeature({ idMap, idHover }): void
changeHoverZIndex({ idMap, idHover, zIndex }): void
```

---

#### Overlay / Popup

```typescript
addOverlay({ idMap, idOverlay, overlayData }): GoOverlay
removeOverlay({ idMap, idOverlay }): void
```

---

#### Rotation

```typescript
// RotaciÃ³n del mapa
addRotation({ idMap, idRotation }): GoRotation
disableRotation({ idMap, idRotation }): void
getRotation({ idMap, idRotation }): GoRotation
addMouseRotation({ olMap, sensibilityMultiplier? }): void  // RotaciÃ³n con botÃ³n derecho
addCustomRotateCompass({ idMap }): HTMLElement | null
```

---

#### Print

```typescript
// ImpresiÃ³n/exportaciÃ³n del mapa
addPrint({ idMap, idPrint, mapTitle?, name?, author?, date? }): void
hasPrint({ idMap, idPrint }): boolean
getPrint({ idMap, idPrint }): GoPrint
removePrint({ idMap, idPrint }): void
```

---

#### FullScreen

```typescript
// Pantalla completa
addFullScreen({ idMap }): void
hasFullScreen({ idMap }): boolean
getFullScreen({ idMap }): GoFullScreen
removeFullScreen({ idMap }): boolean
onFullScreenEnter({ idMap, callback }): void  // Evento: al entrar en pantalla completa
onFullScreenExit({ idMap, callback }): void   // Evento: al salir de pantalla completa
```

---

#### Zoom personalizado

```typescript
addCustomZoom({ idMap, theme }): void           // Controles de zoom estilizados
removeCustomZoom({ idMap, theme }): void
removeAllCustomZoom({ idMap }): void
addCtrlScrollZoom({ idMap }): void              // Zoom solo con Ctrl+Scroll
hasCtrlScrollZoom({ idMap }): boolean
removeCtrlScrollZoom({ idMap }): void
disableDoubleClickZoom({ idMap }): void
enableDoubleClickZoom({ idMap }): void
hasDisabledDoubleClickZoom({ idMap }): boolean
```

---

#### Measure (MediciÃ³n)

```typescript
// âš ï¸ geometryType: "LineString" (lineal) | "Polygon" (Ã¡rea)
addMeasure({
  idMap, idMeasure,
  geometryType: "LineString" | "Polygon",
  callbackStart?: (feature: Feature) => void,
  callbackEnd?:   (feature: Feature) => void,
  style?:         styleFromUser,
  defaultTooltip?: boolean,
  unitSystemSelected?: unitSystem            // unitSystem.METRIC | unitSystem.IMPERIAL
}): GoMeasure

hasMeasure({ idMap, idMeasure }): boolean
setActiveMeasure({ idMap, idMeasure, active: boolean }): void
clearMeasure({ idMap, idMeasure }): void
removeMeasure({ idMap, idMeasure }): void
```

---

#### FlowLines (LÃ­neas de flujo animadas)

```typescript
// Solo para geometrÃ­as LineString
addFlowLines({ idMap, idFlowLines, idLayer }): GoflowLinesMove
hasFlowLines({ idMap, idFlowLines }): boolean
removeFlowLines({ idMap, idFlowLines }): void
```

---

#### DensityLayer (Capa de densidad)

```typescript
// type: densityToolType.HEATMAP | HEXGRID | ISOLINE | ISOBAND
addDensityLayer({ idMap, idDensityTool, layer, type }): GoDensityTool
hasDensityLayer({ idMap, idDensityTool, type }): boolean
getDensityLayer({ idMap, idDensityTool, type }): GoDensityTool
removeDensityLayer({ idMap, idDensityTool }): void
```

---

#### WMS GetFeatureInfo

```typescript
addWMSGetFeatureInfo({
  idMap, idWMSGetFeatureInfo,
  layers: string[],
  coordinates: Coordinate
}): Promise<GoWMSGetFeatureInfo>
```

---

#### Hydraulics â€” Close Tool

```typescript
// âš ï¸ Requiere backend hidrÃ¡ulico
addCloseTool({
  idMap, idCloseTool,
  municipalityId?:    string | number,
  userStyleValve?:    styleFromUser,
  userStylePipes?:    styleFromUser
}): Promise<CloseToolResult>

// CloseToolResult:
// { affectedPipes: [{id, layerId, properties}], affectedValves: [{id, layerId, properties}],
//   isolatedArea?: {type, coordinates}, metadata?: {pipeId, pipeLayerName, analysisTime} }
```

---

#### Hydraulics â€” UpDown Stream

```typescript
// âš ï¸ type: UpDownType.upstream = "upstream" | UpDownType.downstream = "downstream"
addUpDownStream({
  idMap, idUpDown,
  type: UpDownType,
  municipalityId?: string | number
  /* + campos de estilo opcionales para la red */
}): Promise<unknown>
```

---

#### Buffer

```typescript
// dataBuffer.bufferData: Feature | FeatureCollection | string (WKT)
addBuffer({ idMap, idBuffer, dataBuffer: bufferParams, style?: styleFromUser }): Promise<{ serviceBuffer: GeoJSON }>
hasBuffer({ idMap, idBuffer }): boolean
removeBuffer({ idMap, idBuffer }): void
```

---

#### Intersect

```typescript
addIntersect({
  idMap, idIntersect,
  intersectDataIn:   { type: "JSON" | "LAYER", data: string },  // JSON = GeoJSON string, LAYER = idLayer
  intersectDataClip: { type: "JSON" | "LAYER", data: string },
  style: styleFromUser
}): CancelablePromise<unknown>
hasIntersect({ idMap, idIntersect }): boolean
removeIntersect({ idMap, idIntersect }): void
```

---

#### NearData

```typescript
addNearData({
  idMap, idNearData,
  pointCoordinate: [number, number],
  lookingForData:  dataTools,             // Capa destino
  stylePoint?:     ISVGIcon,
  style?:          styleFromUser
}): CancelablePromise<any>
hasNearData({ idMap, idNearData }): boolean
removeNearData({ idMap, idNearData }): void
```

#### NearDataAttributes

```typescript
addNearDataAttributes({
  idMap, idInteraction,
  pointCoordinate: [number, number],
  layer:           dataTools,
  stylePoint?:     ISVGIcon,
  style?:          styleFromUser
}): CancelablePromise<any>
hasNearDataAttributes({ idMap, idInteraction }): boolean
removeNearDataAttributes({ idMap, idInteraction }): void
```

---

#### SectorColor

```typescript
addSectorColor({
  idMap, sectorColorId,
  sectorColorData:   dataTools,
  sectorColorParams: sectorColorParams    // { attribute, paletteSize }
}): CancelablePromise<any>
hasSectorColor({ idMap, idSectorColor }): boolean
removeSectorColor({ idMap, idSectorColor }): void
```

#### PolygonSector

```typescript
addPolygonSector({
  idMap, polygonSectorId,
  polygonSectorData:   dataTools,
  polygonSectorParams: polygonSectorParams  // { gridSize }
}): Promise<any>
hasPolygonSector({ idMap, idPolygonSector }): boolean
removePolygonSector({ idMap, idPolygonSector }): void
```

#### OpticalFiber

```typescript
addOpticalFiber({ idMap, idOpticalFiber, params: opticalFiberParams }): void
hasOpticalFiber({ idMap, idOpticalFiber }): boolean
removeOpticalFiber({ idMap, idOpticalFiber }): void
```

#### Interpolation

```typescript
addInterpolation({ idMap, idInterpolation, params: interpolationParams }): void
hasInterpolation({ idMap, idInterpolation }): boolean
removeInterpolation({ idMap, idInterpolation }): void
```

#### Isolines

```typescript
addIsolines({ idMap, idIsolines, params: isolinesParams }): void
hasIsolines({ idMap, idIsolines }): boolean
removeIsolines({ idMap, idIsolines }): void
```

#### Watershed

```typescript
addWatershed({ idMap, idWatershed, params: watershedParams }): CancelablePromise<any>
hasWatershed({ idMap, idWatershed }): boolean
removeWatershed({ idMap, idWatershed }): void
```

---

#### ThematicMap

```typescript
addThematicMap({ goMap, mapId, thematicMapId, layerGroupIA: LayerGroupIA }): void
hasThematicMap({ mapId, thematicMapId }): boolean
getThematicMap({ mapId, thematicMapId }): LayerGroupIA
removeThematicMap({ mapId, thematicMapId }): void
```

---

#### IFC (BIM)

```typescript
createIFC(props: IFCConfig): Promise<GoIFCComponents>
hasIFCModel({ idMap, idEditFeature }): boolean
removeIFCModel({ idMap, idEditFeature }): void
```

---

#### DMD (Activos digitales)

```typescript
createDMD({ dmdId, dmdToken }): GoDMD
getDMD({ dmdId }): GoDMD
getDMDs(): Array<{ dmdId: string; instance: GoDMD }>
hasDMD({ dmdId }): boolean
removeDMD({ dmdId }): boolean
```

---

#### Cadastre (Catastro)

```typescript
createCadastre({ cadastreId }): GoCadastre
getCadastre({ cadastreId }): GoCadastre
getCadastres(): Array<GoCadastre>
hasCadastre({ cadastreId }): boolean
removeCadastre({ cadastreId }): boolean
```

---

#### SelecciÃ³n por polÃ­gono

```typescript
addSelectByPolygon({ idMap, idSelectByPolygon, style?, callback? }): void
removeSelectByPolygon({ idMap, idSelectByPolygon }): void
clearSelectByPolygon({ idMap, idSelectByPolygon }): void
getMultiExtent({ idMap, idLayers: string[] }): Extent | null
```

---

#### Features WFS-T (EdiciÃ³n / Borrado)

```typescript
addEditFeature({ idEditFeature, idMap, idLayer }): void
hasEditFeature({ idMap, idEditFeature }): boolean
getEditFeatures({ idMap, idEditFeature }): GoEditFeatures
removeEditFeature({ idMap, idEditFeature }): void
saveEditFeatures({ idEditFeature, idMap, idLayer, user, pass }): void
deleteFeatures({ idMap, idLayer, feature: Feature[], user, pass }): void
createFeature(feature: any): Feature  // Convierte GeoJSON raw a Feature OL
```

---

#### Consulta WFS

```typescript
getFeaturesByLayer({
  idMap: string, layerId?: string, layerName?: string,
  cqlFilter?: string, maxFeatures?: number,
  propertyName?: string[]
}): Promise<GeoJSON.FeatureCollection>
```

---

#### GeocodificaciÃ³n Google

```typescript
getDirectionFromCoordinates({ lat: number, lng: number }): Promise<unknown>
getCoordinatesFromDirection({ direction: string }): Promise<unknown>
```

---

#### GeocodificaciÃ³n HERE

```typescript
registerGeocodingHERE({ environmentID, appID, clientID, apiKey? }): Promise<void>
suggestAddress({ params: any }): Promise<any>
getCoordinatesFromDirectionHERE({ direction: string }): Promise<{ lat: number, lng: number }>
```

---

#### Enrutamiento HERE

```typescript
// Ruta simple Aâ†’B
addRoutingHERE({
  idMap, routingID,
  transportMode: string,                 // "car" | "pedestrian" | "bicycle" | ...
  points: Array<geodesicCoordinates>,    // [{ lat, lng }, ...]
  lang?: string
}): Promise<any>

// Ruta multi-parada
addMultiRoutingHERE({
  idMap, routingID,
  points: Array<geodesicCoordinates>,
  stops?: Array<geodesicCoordinates>,
  transportMode: string,
  apiKey: string
}): Promise<any>
```

---

#### Bus de eventos de GoInteractions

```typescript
interactions.on(event: string, callback: Function): void
interactions.off(event: string, callback: Function): void
interactions.once(event: string, callback: Function): void
interactions.hasEvent(event: string): boolean
interactions.getEventHandlers(): object
interactions.emit(event: string, data: object): void
```



---

## 7. MÃ³dulo Services

> **[FILES L9069â€“L9103]** CÃ³digo fuente de `GoStore` en `go-gis-VERBATIM.md` lÃ­neas 9069â€“9103 (`src/goapi/Services/GoStore/index.ts`).

> **[EXAMPLES Â§3a + Â§8c]** `GoStore.getState()` y `GoStore.dispatch()` se usan implÃ­citamente en `loadScripts` (`EXAMPLES Â§3a`). `GoAxiosServices` es el HTTP client interno; su equivalente en la gestiÃ³n de errores aparece en `EXAMPLES Â§8c`. `GoIFCServices` se usa en `EXAMPLES Â§11 Visor BIM/IFC`. `GoIOTNexus` es el backend de `GO_IOT_LAYER` (`EXAMPLES Â§4i`).

**Directorio:** `src/goapi/Services/`

| Servicio | DescripciÃ³n | MÃ©todos clave |
|----------|-------------|---------------|
| `GoStore` | Store global singleton (sin Redux) | `getState(): GoState` ; `dispatch(payload): GoState` |
| `GoAuth` | AutenticaciÃ³n contra GeoServer/backend | `getGeoServerAuth(enviroment): CancelablePromise` |
| `GoAxiosServices` | Wrapper HTTP (axios) con cabeceras de auth | `get()`, `post()`, `put()`, `delete()` |
| `GoCadastre` | Consulta a servicio catastral | MÃ©todos de bÃºsqueda catastral |
| `GoDMD` | Activos digitales DMD | CRUD de activos |
| `GoGeoServices` | Consultas geo/WFS generales | |
| `GoIFCServices` | Servicios de modelos BIM IFC | |
| `GoIOTNexus` | IntegraciÃ³n telemetrÃ­a IoT | |
| `GoLocalStorageService` | Wrapper tipado de localStorage | |
| `GoRSService` | Backend de teledetecciÃ³n | |
| `GoVectorTileOwnServices` | Servicios de tiles vectoriales propios | |

---

### `GoState` interface

Estado global de la aplicaciÃ³n, gestionado por `GoStore`.

```typescript
interface GoState extends Partial<IFCConfig> {
  mapConfig?:         configOptions;           // ConfiguraciÃ³n completa del mapa
  enviroment?:        string;                  // URL del backend
  geoserverAuthData?: { url: string };         // URL autenticada de GeoServer
  token?:             string;                  // Token JWT Bearer
  geocodingHERE?:     HEREParamsService;       // Config del servicio HERE
  iotConfig?:         { url: string; token: string }; // Config IoT
  useBackendFeatures?: boolean;
  isExample?:         boolean;
  config?:            { GOMapOptions: GOMapOptions };
}
```

**Ejemplo de uso:**

```typescript
import { GoStore } from "@services/index";

// Leer estado
const env = GoStore.getState().enviroment ?? "";
const token = GoStore.getState().token;

// Mutar estado
GoStore.dispatch({ enviroment: "https://my-backend.com", token: "eyJhbG..." });
```

---

## 8. MÃ³dulo TOC

> **[FILES L13491â€“L15458]** CÃ³digo fuente de `GoOwnToc` en `go-gis-VERBATIM.md` lÃ­neas 13491â€“15458 (`src/goapi/Toc/GoOwnToc.ts`).

> **[EXAMPLES Â§3d + Â§9]** La configuraciÃ³n JSON del TOC (`optionsToc`, `folderOptions`) estÃ¡ documentada en `EXAMPLES.md Â§3d`. La manipulaciÃ³n programÃ¡tica en runtime (`getOwnToc()`, `setLanguage`, `toc_link`) estÃ¡ en `EXAMPLES.md Â§9 TOC dinÃ¡mico`. `GoCustomLegend` se usa en el catÃ¡logo `EXAMPLES Â§14`: `ex_dynamic_toc`, `ex_manage_toc`, `ex_multiple_toc`.

**Directorio:** `src/goapi/Toc/`

```typescript
export { GoCustomLegend } from "./CustomLegend";
export { default as GoOwnToc } from "./GoOwnToc";
export * from "./GoTocState";
```

| Clase | DescripciÃ³n |
|-------|-------------|
| `GoOwnToc` | Componente TOC: toggles de visibilidad, leyenda, carpetas, buscador de capas, multiidioma, temas dark/light |
| `GoCustomLegend` | Componente de leyenda personalizada independiente |

**MÃ©todos de `GoOwnToc`:**

| MÃ©todo | DescripciÃ³n |
|--------|-------------|
| `setLanguage(lang: string)` | Cambia el idioma del TOC |
| GestiÃ³n de grupos/carpetas | Ver `GoTocState` |

---

## 9. MÃ³dulo UIMap

> **[FILES L9013â€“L9034]** Tipos de UI en `go-gis-VERBATIM.md` lÃ­neas 9013â€“9034 (`src/goapi/UIMap/types.ts`).

> **[EXAMPLES Â§3e]** Todos los campos de `UIConfig` estÃ¡n documentados con su efecto visual en `EXAMPLES.md Â§3e ui â€” botones del mapa`. `UIDisplay` es el componente interno que renderiza estos controles dentro de `GOStagingMap`. `MapLayerPreviewer` se activa con `showLayersPreviewer: true`.

**Directorio:** `src/goapi/UIMap/`

### `UIConfig` interface

```typescript
interface UIConfig {
  showUserLocationButton?:          boolean; // BotÃ³n de localizaciÃ³n del usuario
  showFullScreenButton?:            boolean; // BotÃ³n pantalla completa
  showAddressSearch?:               boolean; // Buscador de direcciones
  showInfoButton?:                  boolean; // BotÃ³n de informaciÃ³n
  showLayersPreviewer?:             boolean; // Previsualizador de capas
  showPointSelectionButton?:        boolean; // SelecciÃ³n por punto
  showLinearMeasurementButton?:     boolean; // MediciÃ³n lineal
  showPolygonMeasurementButton?:    boolean; // MediciÃ³n poligonal
  showMeasurementByPolygonsButton?: boolean; // MediciÃ³n por polÃ­gonos
  showFilterButton?:                boolean; // BotÃ³n de filtro
  showMap3DButton?:                 boolean; // BotÃ³n vista 3D
  showExportButton?:                boolean; // BotÃ³n de exportaciÃ³n
}

enum IOrientation { LEFT = "LEFT", RIGHT = "RIGHT" }
```

| Sub-mÃ³dulo | DescripciÃ³n |
|-----------|-------------|
| `UIDisplay` | Renderizador de controles del mapa |
| `MapLayerPreviewer` | Preview de capas antes de aÃ±adirlas |
| `Utils` | Utilidades UI internas |

---

## 10. MÃ³dulo i18nService

> **[EXAMPLES Â§3f]** Los tokens `{{__key__}}` en `alias`/`urlFolder` y el objeto `translations` del config JSON se documentan en `EXAMPLES.md Â§3f translations â€” i18n en config`. Los idiomas vÃ¡lidos (`SupportedLangs`) y el mÃ©todo `map.setLanguage()` tambiÃ©n se cubren ahÃ­. `addOrUpdateTranslations` de `GOStagingMap` permite inyectar traducciones en runtime sin recargar.

**Directorio:** `src/goapi/i18nService/`

Interfaz del servicio singleton de internacionalizaciÃ³n (envuelve `i18next`).

```typescript
interface II18nService {
  setLanguage(lang: string): void
  isSupportedLang(lang: string): lang is SupportedLangs
  getLanguage(): SupportedLangs
  getTranslation(key: string, variables?: Record<string, any>): string
  onLanguageChanged(callback: () => void): void
}
```

**Locales disponibles:** `en`, `es`, `it`, `fr`, `ro`, `ca`

**Archivos de locale:** `src/goapi/i18nService/locales/{lang}.json`

**Uso:**

```typescript
import i18nService from "@i18n/index";

i18nService.setLanguage("es");
const msg = i18nService.getTranslation("myKey", { name: "Valor" });
i18nService.onLanguageChanged(() => { /* rerender */ });
```

---

## 11. MÃ³dulo Utils

> **[FILES L1â€“L1373]** Tipos principales (`Utils/types/types.ts`) en `go-gis-VERBATIM.md` lÃ­neas 1â€“1373. **[FILES L9104â€“L9685]** Enums, constantes y tipo `layerOptions` completo (`Utils/constants/index.ts`) en lÃ­neas 9104â€“9685. **[FILES L9686â€“L9696]** Paleta de colores de terreno en lÃ­neas 9686â€“9696.

> **[EXAMPLES Â§5 + Â§6eâ€“6f + Â§7 + Â§15]** Este mÃ³dulo contiene todos los tipos y enums usados en ejemplos:
> - `ISVGIcon` â†’ `EXAMPLES Â§5a` (styleSVG) y `Â§5b` (condicional)
> - `styleFromUser` / `GOGeometryType` â†’ `EXAMPLES Â§5c` (styleGeojson) y `Â§6b` (Highlight)
> - `GoFilter` / `GoFilterCondition` (16 operadores) â†’ `EXAMPLES Â§6f` (Filtro No-CQL)
> - `GOMapEvents` / `GOLayerEvents` / `GOFeaturesEvents` â†’ `EXAMPLES Â§7` (eventos)
> - `themes`, `typeOn`, `UpDownType`, `SupportedLangs` â†’ `EXAMPLES Â§15d` (errores comunes)
> - `MapEventManager` â†’ base del bus de eventos en `EXAMPLES Â§7`
> - `GoError` / `ErrorSeverity` â†’ `EXAMPLES Â§8c` (gestiÃ³n de errores)
>
> La tabla completa de correlaciones enumâ†’ejemplo estÃ¡ en `EXAMPLES.md Â§15f`.

**Directorio:** `src/goapi/Utils/`

```typescript
export * from "./Autocomplete"; // Helpers de autocompletado de direcciones
export * from "./classes";      // GoStyle, GoError, GoIcons, GoLoader, MapEventManager, ...
export * from "./constants";    // Todos los enums
export * from "./eval-function";// Evaluador de expresiones
export * from "./functions";    // Funciones puras de utilidad
export * from "./sldreader";    // Parser de estilos SLD
export * from "./types";        // Todos los tipos TypeScript
```

**Clases notables:**

| Clase | DescripciÃ³n |
|-------|-------------|
| `GoStyle` | Envuelve `Style` de OL, aplica `styleFromUser` o iconos SVG |
| `StyleUtils` | Helpers estÃ¡ticos: `editStyle(styleFromUser): Style` |
| `GoIcons` | Generador/cachÃ© de iconos SVG |
| `MapEventManager` | Suscripciones centralizadas a eventos del mapa OL |
| `GoError` | GestiÃ³n de errores tipados con niveles de severidad |

---

### Enums clave

```typescript
enum GOLayerType { /* 19 valores, ver Â§5 */ }

enum GoLayerStyleType  { VECTOR = "VECTOR", GEOJSON = "GEOJSON", CLUSTER = "CLUSTER" }
enum GoLayerSourceType { GEOSERVER = "GEOSERVER", GEOJSON = "GEOJSON" }

enum GoStyleVariant {
  JSON                     = "JSON",
  JSON_CONDITIONAL         = "JSON_CONDITIONAL",
  SVG                      = "SVG",
  SVG_CONDITIONAL          = "SVG_CONDITIONAL",
  SLD                      = "SLD",
  WEBGL_JSON               = "WEBGL_JSON",
  WEBGL_JSON_CONDITIONAL   = "WEBGL_JSON_CONDITIONAL",
  WEBGL_SVG                = "WEBGL_SVG",
  WEBGL_SVG_CONDITIONAL    = "WEBGL_SVG_CONDITIONAL"
}

enum RegistedEnviroments { PRODUCTION = "PRODUCTION", DEVELOPMENT = "DEVELOPMENT", TEST = "TEST" }
enum RegistedApps        { WORK_ORDERS = "WORK_ORDERS", GIS_API_DOCS = "GIS_API_DOCS" }
enum RegistedClients     { HOUSTON = "HOUSTON", IDRICA = "IDRICA" }

enum HexConditions { AVG = "AVG", SUM = "SUM", COUNT = "COUNT" }

enum GOGeometryType {
  POINT       = "POINT",
  LINE_STRING = "LINE_STRING",
  POLYGON     = "POLYGON",
  ALL         = "ALL",
  NOTGEOM     = "NOT_GEOM"
}

// Eventos del mapa OpenLayers raw (para onMap())
enum typeOn {
  change         = "change",
  click          = "click",
  dblclick       = "dblclick",
  singleclick    = "singleclick",
  moveend        = "moveend",
  movestart      = "movestart",
  pointerdrag    = "pointerdrag",
  pointermove    = "pointermove",
  postrender     = "postrender",
  rendercomplete = "rendercomplete"
}

enum pageSize { A5="a5", A4="a4", A3="a3", A2="a2", A1="a1", A0="a0" }

enum IconMode {
  SELECTED       = "selected",
  INACTIVE       = "inactive",
  WARNING_YELLOW = "warning-yellow",
  WARNING_ORANGE = "warning-orange",
  ERROR          = "error"
}

enum IconShape {
  MARKER  = "marker",
  PIN     = "pin",
  CLUSTER = "cluster",
  BADGE   = "badge",
  STACK   = "stack"
}

// Valores reales de los eventos (usar siempre los enums, no strings literales)
enum GOMapEvents {
  MAP_ERROR                = "MAP_ERROR",
  MAP_LOADED               = "MAP_LOADED",
  MAP_LOADING              = "MAP_LOADING",
  BASE_LAYER_CHANGED       = "BASE_LAYER_CHANGED",
  LAYER_VISIBILITY_CHANGED = "layer-visibility-changed"   // âš ï¸ kebab-case
}

enum GOLayerEvents {
  LAYER_ADDED         = "LAYER_ADDED",
  LAYER_ERROR         = "LAYER_ERROR",
  LAYER_LOADED        = "LAYER_LOADED",
  LAYER_LOADING       = "LAYER_LOADING",
  LAYER_REMOVED       = "LAYER_REMOVED",
  LAYER_CHANGED       = "LAYER_CHANGED",
  LAYER_STYLE_CHANGED = "LAYER_STYLE_CHANGED"
}

enum GOFeaturesEvents {
  FEATURES_LOAD_STARTED = "FEATURES_LOAD_STARTED",
  FEATURES_LOAD_ENDED   = "FEATURES_LOAD_ENDED",
  FEATURES_LOAD_ERROR   = "FEATURES_LOAD_ERROR"
}

// âš ï¸ UPPERCASE â€” NO confundir con CSS; el valor real es "DARK" / "LIGHT"
enum themes { DARK = "DARK", LIGHT = "LIGHT" }

// Todos los operadores de filtro disponibles (valor = string exacto que recibe el backend)
enum GoFilterCondition {
  EQUAL_TO                   = "EQUAL TO",
  NOT_EQUAL_TO               = "NOT EQUAL TO",
  BETWEEN                    = "BETWEEN",
  NOT_BEETWEEN               = "NOT BETWEEN",   // ojo: typo histÃ³rico en clave
  GREATER_THAN               = "GREATER THAN",
  GREATER_THAN_OR_EQUAL_TO   = "GREATER THAN OR EQUAL TO",
  LESS_THAN                  = "LESS THAN",
  LESS_THAN_OR_EQUAL_TO      = "LESS THAN OR EQUAL TO",
  IS_NULL                    = "IS NULL",
  IS_NOT_NULL                = "IS NOT NULL",
  IS_EMPTY                   = "IS EMPTY",
  IS_NOT_EMPTY               = "IS NOT EMPTY",
  IS_IN                      = "IS IN",
  IS_NOT_IN                  = "IS NOT IN",
  IS_LIKE                    = "IS LIKE",
  IS_NOT_LIKE                = "IS NOT LIKE"
}

// âš ï¸ valores LOWERCASE
enum UpDownType { upstream = "upstream", downstream = "downstream" }

enum densityToolType {
  HEATMAP = "HEATMAP",
  HEXGRID = "HEXGRID",
  ISOLINE = "ISOLINE",   // isolÃ­neas
  ISOBAND = "ISOBAND"    // isobandas
}

enum unitSystem { METRIC = "metric", IMPERIAL = "imperial" }

const SystemLanguages = { EN: "en", ES: "es", CA: "ca", FR: "fr", RO: "ro", IT: "it" } as const;
type SupportedLangs = typeof SystemLanguages[keyof typeof SystemLanguages];
// => "en" | "es" | "ca" | "fr" | "ro" | "it"
```

---

### Tipos clave

```typescript
// â”€â”€ ConfiguraciÃ³n de la vista â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
type optionsView = {
  id:           string;      // REQUIRED
  center:       Coordinate;  // REQUIRED â€” [x, y] en la proyecciÃ³n configurada
  zoom:         number;      // REQUIRED
  projection?:  string;      // EPSG sin prefijo, ej: "3857" o "4326" (default: "3857")
  extent?:      Extent;      // RestricciÃ³n de area del mapa
  rotation?:    number;      // RotaciÃ³n en radianes
  maxZoom?:     number;
  minZoom?:     number;
  multiWorld?:  boolean;
};

// â”€â”€ ConfiguraciÃ³n del TOC â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
type optionsToc = {
  id:           string;      // REQUIRED â€” ID del elemento HTML del TOC
  active:       boolean;     // REQUIRED
  maxHeight:    number;      // REQUIRED â€” altura mÃ¡xima en px
  legend:       boolean;     // REQUIRED
  panelId?:     string;      // ID del panel contenedor
  mapId?:       string;      // ID del mapa asociado (si â‰  del TOC)
  theme?:       themes;      // themes.DARK | themes.LIGHT
  foldersOptions?:           Array<folderOptions>;
  searchLayersFilter?:       boolean;  // Mostrar buscador de capas en TOC
  allFoldersExpanded?:       boolean;
  legendOpenByDefault?:      boolean;
  tocAlwaysOpen?:            boolean;
};

// ConfiguraciÃ³n de carpetas dentro del TOC
type folderOptions = {
  folderName:               string;   // REQUIRED â€” debe coincidir con urlFolder, ej: "Assets/"
  folderExpandedByDefault?: boolean;
  hasSwitcher?:             boolean;  // Mostrar toggle de visibilidad del grupo
  onlyOneVisibleLayer?:     boolean;  // Radio-button de exclusividad
  folderAlwaysExpanded?:    boolean;
};

// â”€â”€ ConfiguraciÃ³n completa de un mapa â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
type GOMapOptions = {
  id:             string;              // REQUIRED â€” ID Ãºnico del mapa
  div:            string;              // REQUIRED â€” ID del elemento HTML contenedor
  lang:           SupportedLangs;      // REQUIRED â€” "en"|"es"|"ca"|"fr"|"ro"|"it"
  ui:             UIConfig;            // REQUIRED â€” botones/controles UI
  optionView:     optionsView;         // REQUIRED â€” configuraciÃ³n de vista
  translations:   ConfigTranslations;  // REQUIRED â€” traducciones inline para alias/tooltips...
  optionToc?:     optionsToc;          // TOC lateral
  filter?:        string;              // Filtro CQL global para todas las capas
  layer?:         Array<layerOptions>; // Array de capas iniciales
  interactions?:  interactions;        // Herramientas interactivas predefinidas
  projections?:   Array<projOptions>;  // Proyecciones Proj4 adicionales
};

// Estructura JSON raÃ­z (lo que va en mapConfig)
type configOptions = { GOMapOptions: Array<GOMapOptions> };

// â”€â”€ Interacciones en la config del mapa â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
type interactions = {
  closeTool?:    CloseTool;
  updownStream?: UpDown;
  rotation?:     basicInteraction;
  perspective?:  basicInteraction;
  hover?:        basicInteraction;
};

// CloseTool config
type CloseTool = {
  active:           boolean;
  idInteraction:    string;
  municipalityId?:  string | number;
  /* + campos de estilo para tuberÃ­as y vÃ¡lvulas afectadas */
};

// UpDown config
type UpDown = {
  active:        boolean;
  idInteraction: string;
  type:          UpDownType;    // UpDownType.upstream | UpDownType.downstream
  municipalityId?: string | number;
};

// Resultado del close tool
type CloseToolResult = {
  affectedPipes:  Array<{ id: number; layerId: string; properties: Record<string, unknown> }>;
  affectedValves: Array<{ id: number; layerId: string; properties: Record<string, unknown> }>;
  isolatedArea?:  { type: string; coordinates: unknown };
  metadata?:      {
    pipeId:           number;
    pipeLayerName:    string;
    analysisTime:     number;
    municipalityId?:  string | number;
  };
};

// â”€â”€ Filtro geomÃ©trico cliente â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
interface GoFilter {
  field:     string;                                           // REQUIRED
  condition: GoFilterCondition;                                // REQUIRED
  value:     string | number | Array<string | number>;         // REQUIRED
  or?:       Array<GoFilter>;   // Condiciones OR encadenadas
  and?:      Array<GoFilter>;   // Condiciones AND encadenadas
}

// â”€â”€ Estilo global â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
interface GoGlobalStyle {
  layerStyleType?: GoLayerStyleType;  // "VECTOR" | "GEOJSON" | "CLUSTER"
  styleType?:      GoStyleVariant;    // Variante de estilo
  styleData?:      GoStyleData;       // Datos del estilo
  filter?:         GoFilter;
  layerOpts?:      layerOptions | LayerOptsMapbox;
  inheritsFrom?:   number;            // Ãndice de GoGlobalStyle del que hereda
}

// â”€â”€ Estilo desde usuario â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
type styleFromUser = {
  type?:            GOGeometryType;   // "POINT"|"LINE_STRING"|"POLYGON"|"ALL"|"NOT_GEOM"
  stroke?:          StrokeOptions;    // { color: string; width?: number }
  fill?:            FillOptions;      // { color: string }
  icon?:            boolean;          // Usar icono en lugar de punto
  img?:             StyleFromUserImg;
  graphic?:         StyleFromUserGraphic;
  text?:            StyleFromUserText;
  geometryFilter?:  [filterGeometry];
  zIndex?:          number;
};

// â”€â”€ Icono SVG â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
interface ISVGIcon {
  iconSvg?:          string;              // Nombre del icono de la librerÃ­a go-gis
  iconUrl?:          string;              // URL de imagen externa (.png/.svg)
  shape?:            IconShape;           // "marker"|"pin"|"cluster"|"badge"|"stack"
  mode?:             IconMode | string;   // "selected"|"inactive"|"warning-yellow"|"warning-orange"|"error" o color hex
  anchor?:           [number, number];    // [0.5, 0.5] = centro del icono
  scale?:            number;
  rotate?:           number;              // RotaciÃ³n fija en radianes
  backgroundColor?:  string;             // Color HEX/rgba de fondo
  iconColor?:        string;             // Color HEX/rgba del icono
  highlight?:        string;
  x?:                number;
  y?:                number;
  size?:             number[];            // [width, height] en pÃ­xeles
  elevation?:        number;
  haloSize?:         number;
  fontColor?:        string;
  fontSize?:         string;
  // RotaciÃ³n dinÃ¡mica por campo
  rotationField?:    string;             // Nombre del campo de la feature con el Ã¡ngulo
  rotationUnits?:    "deg" | "grad" | "rad";
  rotationAdd?:      number;
  rotationInverse?:  boolean;
}

// â”€â”€ Config servicio HERE â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
interface HEREParams {
  environmentID:  RegistedEnviroments;   // "PRODUCTION" | "DEVELOPMENT" | "TEST"
  appID:          RegistedApps;          // "WORK_ORDERS" | "GIS_API_DOCS"
  clientID:       RegistedClients;       // "HOUSTON" | "IDRICA"
  apiKey?:        string;
}

// â”€â”€ Idiomas soportados â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
// Valores exactos (SIEMPRE minÃºsculas):
type SupportedLangs = "en" | "es" | "ca" | "fr" | "ro" | "it";
```

---

## 12. Mapa de interconexiones entre mÃ³dulos

> **[EXAMPLES Â§15b]** El diagrama de este apartado es la base de la Â«Receta para agentesÂ» de `EXAMPLES.md Â§15b`. Cada flecha de este grafo corresponde a un paso del checklist `EXAMPLES Â§15a`.

```
GoMapLoader
  â”œâ”€â”€ lee:     GoStore, GoAuth, GO_CONFIG
  â”œâ”€â”€ construye: GoMap â†’ GOStagingMap[]
  â””â”€â”€ usa:     GoOwnToc, UIDisplay, MapLayerPreviewer

GoMap
  â”œâ”€â”€ posee:   Map<id, GOStagingMap>
  â”œâ”€â”€ posee:   GoInteractions (compartido entre todos los mapas)
  â””â”€â”€ delega en GOStagingMap para operaciones por mapa

GOStagingMap
  â”œâ”€â”€ contiene: OlMap (OpenLayers core)
  â”œâ”€â”€ contiene: GoView extends OL View
  â”œâ”€â”€ contiene: Map<layerId, GOLayer>
  â”œâ”€â”€ contiene: GoOwnToc (UI TOC)
  â”œâ”€â”€ contiene: UIDisplay (controles mapa)
  â”œâ”€â”€ contiene: MapLayerPreviewer
  â””â”€â”€ referencia a: GoInteractions (compartido)

GOLayer (abstracta)
  â”œâ”€â”€ recibe:  GOStagingMap (acceso al olMap)
  â”œâ”€â”€ emite:   GOLayerEvents via EventEmitter
  â””â”€â”€ usa:     GoStyle, GoIcons, GoAxiosServices, GoGeoServices

GOStyledLayer â†’ GOVectorLayer
  â”œâ”€â”€ obtiene datos de GeoServer via filtros CQL
  â”œâ”€â”€ usa:     StyleUtils (estilos SVG/SLD/JSON)
  â””â”€â”€ usa:     GoAxiosServices para HTTP

GoInteractions
  â”œâ”€â”€ recibe:  Map<id, GOStagingMap> (acceso a todos los mapas)
  â”œâ”€â”€ herramientas indexadas por: (idMap, idInteraction)
  â”œâ”€â”€ usa:     GoGeocoding, GoGeocodingHERE (geocodificaciÃ³n)
  â”œâ”€â”€ usa:     GoCadastre, GoDMD (servicios de datos)
  â”œâ”€â”€ usa:     GoNexusInfo (IoT)
  â””â”€â”€ usa:     GoUpDown, GoCloseTools (hidrÃ¡ulica)

GoStore (singleton global)
  â”œâ”€â”€ almacena: token, enviroment, mapConfig, geoserverAuthData
  â””â”€â”€ leÃ­do por: GoAuth, GoAxiosServices, GoRSService,
                 getRemoteSensingApiBaseUrl, todos los servicios

i18nService (singleton)
  â”œâ”€â”€ envuelve: i18next
  â”œâ”€â”€ idiomas: EN/ES/IT/FR/RO/CA
  â””â”€â”€ consumido por: GoOwnToc, GoMap.setLanguage, UIs de RS layer
```

---

## 13. PatrÃ³n de uso tÃ­pico

> **[EXAMPLES Â§13 + Â§15]** Este snippet TypeScript es la versiÃ³n de librerÃ­a pura del patrÃ³n. La versiÃ³n para el entorno de docs (con `loadScripts`, `config.json` y HTML) estÃ¡ en `EXAMPLES.md Â§13 Ejemplo mÃ­nimo funcional completo`. La guÃ­a completa para agentes que deben generar variantes de este patrÃ³n estÃ¡ en `EXAMPLES.md Â§15`.

```typescript
import { GoMapLoader, GoMap, GOLayerType, GoFilterCondition, themes, UpDownType, typeOn,
         MapEventManager, GOLayerEvents, GOFeaturesEvents, RegistedEnviroments,
         RegistedApps, RegistedClients } from "go-gis";

// 1. CreaciÃ³n del mapa
const map: GoMap = await GoMapLoader.loadGoMap({
  enviroment:          "https://my-gis-backend.example.com",
  userToken:           "eyJhbGciOi...",
  useBackendFeatures:  true,
  mapConfig: {
    GOMapOptions: [{
      id:   "main",
      div:  "map-container",
      lang: "es",
      ui: {
        showFullScreenButton: true,
        showAddressSearch:    true,
        showLayersPreviewer:  true,
      },
      optionView: {
        id:         "view1",
        center:     [-0.377, 39.469],
        zoom:        15,
        projection:  "4326",
      },
      optionToc: {
        active:  true,
        legend:  true,
        maxHeight: 380,
      },
      interactions: {
        hover: { active: true, idInteraction: "hov1" },
      },
      layer: [
        {
          id:          "basemap",
          layerType:   GOLayerType.GO_MAPBOX,
          isBaseLayer: true,
          style:       "<mapbox-style-id>",
          token:       "<mapbox-token>",
        },
        {
          id:               "pipes",
          alias:            "TuberÃ­as",
          layerType:        GOLayerType.GO_VECTOR_LAYER,
          url:              "https://my-gis-backend.example.com/geoserver",
          layerName:        "water:pipes",
          layerProjection:  "4326",
          defaultVisibility: true,
          zIndex:            5,
          urlFolder:        "Red/",
          legend:           true,
          styleSVG: {
            iconSvg:         "001_01_pipe",
            shape:           "marker",
            backgroundColor: "#ffffff",
            mode:            "inactive",
            scale:            1,
            anchor:          [0.5, 0.5],
          },
        },
      ],
      translations: { es: {}, en: {} },
    }]
  }
});

// 2. Control de vista
map.setCenter("main", [-0.377, 39.469]);
map.setZoom("main", 16);
map.flyToCenter("main", [-0.380, 39.470], 1000, 17);
map.setThemeAllMaps(themes.DARK);   // "DARK" â€” no "dark"
map.setLanguage("en");

// 3. GestiÃ³n de capas
map.addLayer("main", {
  id:        "valves",
  layerType:  GOLayerType.GO_WFS_LAYER,
  url:        "https://...",
  layerName:  "water:valves",
});
map.removeLayer("main", "valves");
map.refreshLayer("main", ["pipes"]);

// 4. Acceso a capa y filtros
const layer = map.getStaging("main").getLayer("pipes");
layer.addFilter('"estado" = \'activo\'', true);
layer.addFilterNoCQL({ field: "diametro", condition: GoFilterCondition.GREATER_THAN, value: 100 });
layer.removeFilter();
layer.removeFilterNoCQL();

// 5b. Inyectar datos en tiempo real (GeoJSON layer)
const geoJsonLayer = map.getStaging("main").getLayer("geojson-layer-id");
map.injectDataGeoJson("main", "geojson-layer-id", true, {
  type: "FeatureCollection",
  features: [{ type: "Feature", geometry: { type: "Point", coordinates: [0, 0] }, properties: {} }]
});

// 5c. Comprobar existencia de capa
if (map.existLayer("main", "pipes")) {
  console.log("La capa pipes existe");
}

// 5d. Escuchar evento nativo de OpenLayers
map.onMap("main", typeOn.moveend, () => {
  const center = map.getCenter("main");
  console.log("Nuevo centro:", center);
});

// 5e. Desmontar un mapa (limpieza total)
// map.unmountMap("main");

// 5. Interacciones
const interactions = map.getInteractions();

// Resaltado
interactions.addHighlight({
  idMap:       "main",
  idHighlight: "sel1",
  features:    selectedFeatures,
});

// Hover
interactions.mouseHover({
  idMap:   "main",
  idHover: "hov1",
  layer:   "pipes",
  callback: (feature) => { console.log(feature.getProperties()); },
});

// Cierre de red
interactions.addCloseTool({ idMap: "main", idCloseTool: "ct1" });

// Aguas arriba/abajo
interactions.addUpDownStream({ idMap: "main", idUpDown: "ud1", type: UpDownType.downstream });

// Buffer + IntersecciÃ³n
interactions.addBuffer({ idMap: "main", idBuffer: "buf1", dataBuffer: { bufferData: feat, distance: 500 } })
  .then(({ serviceBuffer }) => {
    interactions.addIntersect({
      idMap:             "main",
      idIntersect:       "int1",
      intersectDataIn:   { type: "JSON",  data: JSON.stringify(serviceBuffer) },
      intersectDataClip: { type: "LAYER", data: "pipes" },
      style: { type: "ALL", stroke: { color: "#FF5858" }, fill: { color: "rgba(255,151,193,0.2)" } },
    });
  });

// 6. Eventos

MapEventManager.events.on(GOLayerEvents.LAYER_LOADED, ({ layerId }) => {
  console.log(`Capa ${layerId} cargada`);
});

// 7. GeocodificaciÃ³n HERE
interactions.registerGeocodingHERE({
  environmentID: RegistedEnviroments.PRODUCTION,
  appID:         RegistedApps.GIS_API_DOCS,
  clientID:      RegistedClients.IDRICA,
});
interactions.getCoordinatesFromDirectionHERE({ direction: "Calle Mayor 1, Valencia" })
  .then(({ lat, lng }) => map.flyToCenter("main", [lng, lat], 3000));
```
