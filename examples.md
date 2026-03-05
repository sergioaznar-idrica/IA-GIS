# go-gis â€” GuÃ­a completa de Ejemplos (`docs/examples/`)

> **Generado:** Marzo 2026
> **Documento de referencia arquitectÃ³nica:** `src/GO-GIS-ARCHITECTURE.md`
> **CÃ³digo fuente verbatim:** `go-gis-VERBATIM.md` (raÃ­z del repositorio, ~15500 lÃ­neas, 20 archivos `src/goapi/`)

---

## Contexto arquitectÃ³nico â€” cÃ³mo leer este documento

> **Para agentes IA:** Este documento trabaja en trÃ­o con otros dos. Antes de generar cualquier ejemplo, internaliza el protocolo:
>
> 1. **Este documento** (`EXAMPLES.md`) â€” patrones de uso listos para copiar, catÃ¡logo de 170+ ejemplos, guÃ­a de agentes Â§15.
> 2. **`GO-GIS-ARCHITECTURE.md`** â€” fuente de verdad de tipos TypeScript, firmas de mÃ©todos, jerarquÃ­a de clases, enums. Las referencias `[ARCH Â§N]` en este documento apuntan a secciones exactas.
> 3. **`go-gis-VERBATIM.md`** â€” cÃ³digo implementaciÃ³n real de 20 archivos `src/goapi/`. Las referencias `[FILES L{n}â€“L{n}]` en este documento apuntan a las lÃ­neas exactas del archivo.
>
> **Regla de uso:** Para tipos y firmas â†’ ir a `ARCH Â§N`. Para el cÃ³digo fuente real â†’ ir a `FILES L{n}â€“L{n}`. Para patrones de uso â†’ quedarse aquÃ­.

### Ãndice de archivos fuente â€” `go-gis-VERBATIM.md`

| Archivo (`src/goapi/...`) | LÃ­neas en ALL_FILES | SecciÃ³n ARCH |
|---------------------------|---------------------|--------------|
| `Utils/types/types.ts` | **L1â€“L1373** | Â§11 Tipos clave |
| `Map/GoMap.ts` | **L1374â€“L2608** | Â§4 GoMap |
| `Map/GoMapLoader.ts` | **L2609â€“L3331** | Â§4 GoMapLoader |
| `Map/GoStagingMap.ts` | **L3332â€“L4129** | Â§4 GOStagingMap |
| `Layers/GoLayer/index.ts` | **L4130â€“L5131** | Â§5 GOLayer |
| `Layers/GoVectorLayer/index.ts` | **L5132â€“L6096** | Â§5 GOVectorLayer |
| `Layers/GoClusterLayer/index.ts` | **L6097â€“L7310** | Â§5 GOClusterLayer |
| `Layers/GoGeoJsonLayer/index.ts` | **L7311â€“L7915** | Â§5 GOGeoJsonLayer |
| `Layers/GOWMS/index.ts` | **L7916â€“L8214** | Â§5 GOWMSLayer |
| `Layers/GoWFS/index.ts` | **L8215â€“L9012** | Â§5 GOWFSLayer |
| `UIMap/types.ts` | **L9013â€“L9034** | Â§9 UIMap |
| `Interactions/Features/index.ts` | **L9035â€“L9064** | Â§6 Features |
| `Interactions/functions/getFeaturesByLayer.ts` | **L9065â€“L9068** | Â§6 Firmas |
| `Services/GoStore/index.ts` | **L9069â€“L9103** | Â§7 Services |
| `Utils/constants/index.ts` | **L9104â€“L9685** | Â§11 Enums + layerOptions |
| `Layers/common/terrainColors.ts` | **L9686â€“L9696** | Â§5 (colores terreno) |
| `Interactions/geoTools/buffer/index.ts` | **L9697â€“L9968** | Â§6 geoTools GoBuffer |
| `Interactions/index.ts` (GoInteractions) | **L9969â€“L13490** | Â§6 Firmas de mÃ©todos |
| `Toc/GoOwnToc.ts` | **L13491â€“L15458** | Â§8 TOC |
| `Interactions/Features/interact/GetFeaturesByLayer.ts` | **L15459â€“L15462** | Â§6 Firmas |

### Modelo mental (flujo de creaciÃ³n)

```
GoMapLoader.loadGoMap(MapConfigLoader)          â† ARCH Â§4 GoMapLoader
    â””â”€â–º Promise<GoMap>                          â† ARCH Â§4 GoMap (fachada principal)
           â”œâ”€â”€ Map<id, GOStagingMap>             â† ARCH Â§4 GOStagingMap (estado interno)
           â”‚       â”œâ”€â”€ OlMap (OpenLayers)
           â”‚       â”œâ”€â”€ GoView                   â† ARCH Â§4 GoView
           â”‚       â”œâ”€â”€ Map<layerId, GOLayer>     â† ARCH Â§5 Sistema de Capas
           â”‚       â”œâ”€â”€ GoOwnToc                 â† ARCH Â§8 TOC
           â”‚       â””â”€â”€ UIDisplay                â† ARCH Â§9 UIMap
           â””â”€â”€ GoInteractions (compartido)      â† ARCH Â§6 Interactions
```

### Tabla de correlaciones EXAMPLES â†” ARCHITECTURE â†” go-gis-VERBATIM

| SecciÃ³n EXAMPLES | Clase / Tipo / Enum en ARCH | SecciÃ³n ARCH | CÃ³digo fuente (ALL_FILES) |
|------------------|-----------------------------|--------------|--------------------------|
| `loadScripts` â†’ `GoMapLoader.loadGoMap` | `MapConfigLoader`, `GoMapLoader`, `GoMap` | Â§4 | **[FILES L2609â€“L3331]** GoMapLoader |
| `config.json` raÃ­z | `configOptions = { GOMapOptions: Array<GOMapOptions> }` | Â§11 Tipos clave | **[FILES L1â€“L1373]** types.ts |
| `optionView` | `optionsView` type | Â§11 Tipos clave | **[FILES L1â€“L1373]** types.ts |
| `optionToc` / `foldersOptions` | `optionsToc`, `folderOptions` types | Â§11 Tipos clave | **[FILES L1â€“L1373]** types.ts |
| `ui` â€” botones | `UIConfig` interface | Â§9 UIMap | **[FILES L9013â€“L9034]** UIMap/types.ts |
| `translations` / idiomas | `SupportedLangs`, `i18nService` | Â§10 i18nService | **[FILES L9104â€“L9685]** constants |
| Campos comunes de capa | `layerOptions` type (todos los campos) | Â§5 | **[FILES L9104â€“L9685]** constants |
| `layerType` valores | `GOLayerType` enum (19 valores) | Â§5 Enum GOLayerType | **[FILES L9104â€“L9685]** constants |
| `GO_VECTOR_LAYER` | `GOVectorLayer` extends `GOStyledLayer` | Â§5 | **[FILES L5132â€“L6096]** GoVectorLayer |
| `GO_WMS_LAYER` | `GOWMSLayer` extends `GOLayer` | Â§5 | **[FILES L7916â€“L8214]** GOWMS |
| `GO_WFS_LAYER` | `GOWFSLayer` extends `GOStyledLayer` | Â§5 | **[FILES L8215â€“L9012]** GoWFS |
| `GO_GEOJSON_LAYER` | `GOGeoJsonLayer` extends `GOStyledLayer` | Â§5 | **[FILES L7311â€“L7915]** GoGeoJsonLayer |
| `GO_CLUSTER_LAYER` | `GOClusterLayer` extends `GOStyledLayer` | Â§5 | **[FILES L6097â€“L7310]** GoClusterLayer |
| `GO_HEATMAP_LAYER` | `GoHeatMapLayer` extends `GOStyledLayer` | Â§5 | **[FILES L5132â€“L6096]** (hereda GOVectorLayer) |
| `GO_HEXGRID_LAYER` | `GoHexGridLayer` extends `GOStyledLayer` | Â§5 | **[FILES L4130â€“L5131]** GoLayer |
| `GO_IOT_LAYER` | `GoIOTLayer` extends `GOStyledLayer` | Â§5 | â€” |
| `GO_TRUCK_LAYER` | `GoTruckLayer` extends `GOStyledLayer` | Â§5 | â€” |
| `GO_WEBGL_LAYER` | `GoWebGlLayer` extends `GOStyledLayer` | Â§5 | â€” |
| `GO_MAPBOX` / `GO_XYZ` / `GO_ESRI_BASEMAP` | `GoMapBoxLayer`, `GoXYZ`, `GoEsriBaseMap` | Â§5 | â€” |
| `styleSVG` | `ISVGIcon` interface | Â§11 Tipos clave | **[FILES L1â€“L1373]** types.ts |
| `styleSVGConditional` | `styleSVG` type + `ISVGIcon` | Â§5 layerOptions | **[FILES L1â€“L1373]** types.ts |
| `styleGeojson` | `styleGeojson` type + `styleFromUser` | Â§5, Â§11 | **[FILES L1â€“L1373]** types.ts |
| `sldStyle` | `styleOptions` type | Â§5 layerOptions | **[FILES L9104â€“L9685]** constants |
| condiciones de estilo (`"EQUAL TO"`, etc.) | `GoFilterCondition` enum | Â§11 Enums clave | **[FILES L9104â€“L9685]** constants |
| `addHighlight` / `removeHighlight` | `GoHighlight`, `Features` module | Â§6 Features | **[FILES L9035â€“L9064]** Features |
| `mouseHover` / `removeHover` | `GoHover`, `Features` module | Â§6 Features | **[FILES L9035â€“L9064]** Features |
| `addBuffer` / `addIntersect` | `GoBuffer`, `GoIntersect`, `geoTools` | Â§6 geoTools | **[FILES L9697â€“L9968]** geoTools/buffer |
| `addFilter` (CQL) | `GOVectorLayer.addFilter(cql, reload?)` | Â§5 GOVectorLayer | **[FILES L5132â€“L6096]** GoVectorLayer |
| `addFilterNoCQL` | `GoFilter` interface + `GoFilterCondition` | Â§11 Tipos clave | **[FILES L9104â€“L9685]** constants |
| `addEditFeature` / `saveEditFeatures` | `GoEditFeatures` module | Â§6 Features | **[FILES L9969â€“L13490]** Interactions |
| `getFeaturesByLayer` | `GoInteractions.getFeaturesByLayer` | Â§6 Firmas de mÃ©todos | **[FILES L9065â€“L9068]** + **[FILES L15459â€“L15462]** |
| `addSelectByPolygon` | `GoSelectByPolygon`, `Selection` module | Â§6 Selection | **[FILES L9969â€“L13490]** Interactions |
| `addMeasure` | `GoMeasure`, `Analysis` module | Â§6 Analysis | **[FILES L9969â€“L13490]** Interactions |
| `GOMapEvents` / `GOLayerEvents` / `GOFeaturesEvents` | Enums de eventos + `MapEventManager` | Â§11 Enums clave | **[FILES L9104â€“L9685]** constants |
| `registerGeocodingHERE` | `GoGeocodingHERE`, `ExternalsConnections` | Â§6 ExternalsConnections | **[FILES L9969â€“L13490]** Interactions |
| `addRoutingHERE` | `GoRouting`, `ExternalsConnections` | Â§6 ExternalsConnections | **[FILES L9969â€“L13490]** Interactions |
| `GoOwnToc` / TOC dinÃ¡mico | `GoOwnToc`, `GoCustomLegend` | Â§8 TOC | **[FILES L13491â€“L15458]** GoOwnToc |
| `MapboxViewer` | `@mapbox/index` (mÃ³dulo Mapbox) | Â§1 Punto de entrada | â€” |
| IFC / BIM | `@ifc/index` (mÃ³dulo IFC) | Â§1 Punto de entrada | â€” |
| `GoStore` / `GoState` | `GoStore.getState()`, `GoState` interface | Â§7 Services | **[FILES L9069â€“L9103]** GoStore |
| Errores | `GoError`, `ErrorSeverity` | Â§11 Utils | **[FILES L1â€“L1373]** types.ts |

### Invariantes que todo ejemplo debe respetar

1. **`loadGoMap` es siempre async** â€” toda lÃ³gica post-mapa va dentro del `.then(map => { ... })`.
2. **`layerType` es siempre un string** en `config.json` (ej: `"GO_VECTOR_LAYER"`). En cÃ³digo TypeScript usar `GOLayerType.GO_VECTOR_LAYER`.
3. **`id`** de mapa, capa e interacciÃ³n deben ser Ãºnicos globalmente en la pÃ¡gina.
4. **`GOStagingMap.getLayer(id)`** requiere que la capa ya exista; usar `map.existLayer(mapId, layerId)` antes si hay duda.
5. **Enums de tema** â€” `themes.DARK = "DARK"`, `themes.LIGHT = "LIGHT"` (UPPERCASE). No usar strings CSS.
6. **Idiomas** â€” `SupportedLangs = "en" | "es" | "ca" | "fr" | "ro" | "it"` (minÃºsculas). Ver `SystemLanguages`.
7. **Proyecciones** â€” `projection` en `optionsView` es el cÃ³digo EPSG **sin prefijo** (`"3857"`, no `"EPSG:3857"`).
8. **`urlFolder`** de capa debe coincidir exactamente con `folderName` en `foldersOptions` del TOC (incluyendo la `/` final).

---

## Tabla de contenidos

1. [Estructura de un ejemplo](#1-estructura-de-un-ejemplo)
2. [Plantilla HTML base](#2-plantilla-html-base)
3. [InicializaciÃ³n del mapa](#3-inicializaciÃ³n-del-mapa)
   - [loadScripts](#3a-loadscripts-funciÃ³n-de-arranque)
   - [config.json â€” estructura raÃ­z](#3b-configjson--estructura-raÃ­z)
   - [optionView](#3c-optionview--configuraciÃ³n-de-vista)
   - [optionToc](#3d-optiontoc--tabla-de-contenidos)
   - [ui â€” botones del mapa](#3e-ui--botones-del-mapa)
   - [translations â€” i18n en config](#3f-translations--i18n-en-config)
4. [Patrones de capas](#4-patrones-de-capas)
   - [Mapas base](#4a-mapas-base)
   - [Capa Vectorial GeoServer](#4b-capa-vectorial-geoserver-go_vector_layer)
   - [Capa WMS](#4c-capa-wms-go_wms_layer)
   - [Capa WFS externa](#4d-capa-wfs-externa-go_wfs_layer)
   - [Capa GeoJSON](#4e-capa-geojson-go_geojson_layer)
   - [Capa ClÃºster](#4f-capa-clÃºster-go_cluster_layer)
   - [Capa HeatMap](#4g-capa-heatmap-go_heatmap_layer)
   - [Capa HexGrid](#4h-capa-hexgrid-go_hexgrid_layer)
   - [Capa IoT](#4i-capa-iot-go_iot_layer)
   - [Capa Truck / Fleet](#4j-capa-truck--fleet-go_truck_layer)
   - [Capa WebGL](#4k-capa-webgl-go_webgl_layer)
   - [Capa Vector Tiles](#4l-capa-vector-tiles-go_vector_tile)
   - [Remote Sensing](#4m-remote-sensing-go_remote_sensing)
5. [Patrones de estilo](#5-patrones-de-estilo)
   - [Icono SVG (styleSVG)](#5a-icono-svg-stylesvg)
   - [Icono SVG condicional](#5b-icono-svg-condicional-stylesvgconditional)
   - [Estilo GeoJSON temÃ¡tico](#5c-estilo-geojson-temÃ¡tico-stylegeojson)
   - [Estilo SLD (servidor)](#5d-estilo-sld-servidor-sldstyle)
6. [Patrones de interacciÃ³n](#6-patrones-de-interacciÃ³n)
   - [Clic en el mapa](#6a-clic-en-el-mapa)
   - [Highlight](#6b-highlight)
   - [Hover](#6c-hover)
   - [Buffer + IntersecciÃ³n](#6d-buffer--intersecciÃ³n)
   - [Filtro CQL (servidor)](#6e-filtro-cql-servidor)
   - [Filtro No-CQL (cliente)](#6f-filtro-no-cql-cliente)
   - [Editar features (WFS-T)](#6g-editar-features-wfs-t)
   - [Eliminar features (WFS-T)](#6h-eliminar-features-wfs-t)
   - [Consulta por capa (WFS)](#6i-consulta-por-capa-wfs)
   - [SelecciÃ³n por polÃ­gono](#6j-selecciÃ³n-por-polÃ­gono)
   - [MediciÃ³n](#6k-mediciÃ³n)
7. [Sistema de eventos](#7-sistema-de-eventos)
8. [Servicios](#8-servicios)
   - [GeocodificaciÃ³n HERE](#8a-geocodificaciÃ³n-here)
   - [Routing HERE](#8b-routing-here)
   - [GestiÃ³n de errores](#8c-gestiÃ³n-de-errores)
9. [TOC dinÃ¡mico](#9-toc-dinÃ¡mico)
10. [Visor 3D / Mapbox](#10-visor-3d--mapbox)
11. [Visor BIM / IFC](#11-visor-bim--ifc)
12. [Descarga de capas](#12-descarga-de-capas)
13. [Ejemplo mÃ­nimo funcional completo](#13-ejemplo-mÃ­nimo-funcional-completo)
14. [CatÃ¡logo de ejemplos (170+)](#14-catÃ¡logo-de-todos-los-ejemplos)
15. [GuÃ­a rÃ¡pida para agentes IA](#15-guÃ­a-rÃ¡pida-para-agentes-ia)

---

## 1. Estructura de un ejemplo

> **[ARCH Â§2 Arquitectura general]** La separaciÃ³n de `config.json` y `scripts.js` refleja la separaciÃ³n arquitectÃ³nica entre `configOptions` (JSON declarativo, pasado a `GoMapLoader.loadGoMap`) y la lÃ³gica imperativa sobre la instancia `GoMap` devuelta. `exampleCode.js` contiene Ãºnicamente strings de cÃ³digo para la UI de documentaciÃ³n y no tiene efecto sobre el mapa.

Cada ejemplo sigue la misma estructura de ficheros:

```
docs/examples/ex_<nombre>/
â”œâ”€â”€ config.json            â† ConfiguraciÃ³n completa del mapa y capas
â”œâ”€â”€ index.html             â† Shell HTML (sin JS inline)
â””â”€â”€ static/
    â””â”€â”€ js/
        â”œâ”€â”€ scripts.js     â† Punto de entrada principal (importa loadScripts y conecta UI)
        â”œâ”€â”€ exampleCode.js â† CÃ³digo mostrado en las pestaÃ±as de documentaciÃ³n
        â””â”€â”€ *.js           â† Helpers auxiliares (buffer.js, addFilter.js, ...)
```

---

## 2. Plantilla HTML base

> **[ARCH Â§9 UIMap â€” UIDisplay / MapLayerPreviewer]** El div `#go-map-loader` es el target de la animaciÃ³n Lottie de carga, gestionada por `UIDisplay` dentro de `GOStagingMap`. El `id` del div del mapa (ej: `id="mapa"`) debe coincidir **exactamente** con el campo `"div"` en `config.json` (campo `GOMapOptions.div`). El `data-theme="gistheme"` en `<html>` es el tema CSS base; se puede sobreescribir con `map.setThemeAllMaps(themes.DARK)` en runtime.

Todos los ejemplos comparten la misma estructura HTML:

```html
<!DOCTYPE html>
<html data-theme="gistheme">
<head>
    <meta charset="utf-8" />
    <title>Framework Go-Gis</title>
    <link rel="shortcut icon" type="image/png" href="../../static/img/logo-xylem-stacked.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="../../static/styles/docs.css" />
</head>
<body>
    <!-- Navbar inyectado en runtime (Bootstrap dark) -->
    <div class="go-navbar"></div>

    <div class="container-examples" id="wrap" class="clearfix">
        <div class="separador separador20px"></div>
        <h1 class="title-example">Nombre del Ejemplo</h1>
        <div class="separador separador20px"></div>

        <!-- CONTENEDOR DEL MAPA â€” el id debe coincidir con "div" en config.json -->
        <div id="mapa" class="container-map-example">
            <div id="go-map-loader"></div>  <!-- animaciÃ³n de carga Lottie -->
        </div>

        <!-- Controles UI opcionales -->
        <span id="mi-accion" class="button">Hacer algo</span>

        <!-- Visor de cÃ³digo fuente â€” tabs generados automÃ¡ticamente -->
        <div id="tabs"></div>
    </div>

    <!-- UN solo script de mÃ³dulo por pÃ¡gina -->
    <script type="module" src="./static/js/scripts.js"></script>
</body>
</html>
```

**Convenciones clave:**
- `data-theme="gistheme"` en `<html>`.
- `.go-navbar` se rellena en runtime por `base/main.js` mediante `navbar.html`.
- El div del mapa tiene el `id` que corresponde con `"div"` en `config.json`.
- `#go-map-loader` muestra la animaciÃ³n de carga Lottie hasta que el mapa se resuelve.
- `#tabs` es el target del visor de cÃ³digo (llenado por `loadExampleTabs`).
- Todo el scripting es un Ãºnico `<script type="module">` punto de entrada.

---

## 3. InicializaciÃ³n del mapa

> **[ARCH Â§4 GoMapLoader â†’ GoMap â†’ GOStagingMap]** El flujo completo de inicializaciÃ³n es: `loadScripts` â†’ `GoMapLoader.loadGoMap(MapConfigLoader)` â†’ autenticaciÃ³n Keycloak + `GoAuth.getGeoServerAuth` â†’ `GoStore.dispatch` (almacena token y enviroment) â†’ construcciÃ³n de `GoMap` â†’ construcciÃ³n de `GOStagingMap` por cada entrada en `GOMapOptions`. Al resolver la promesa, `GOStagingMap` ya contiene el `OlMap`, `GoView`, todas las capas y `GoOwnToc` listos para usar.

---

### 3a. `loadScripts` â€” funciÃ³n de arranque

> **[ARCH Â§4 GoMapLoader]** `loadScripts` es una funciÃ³n de conveniencia del entorno de ejemplos que llama internamente a `GoMapLoader.loadGoMap(MapConfigLoader)`. La firma exacta de `MapConfigLoader` es:
> ```typescript
> interface MapConfigLoader {
>   mapConfig:           configOptions;  // REQUIRED â€” estructura raÃ­z del JSON
>   enviroment:          string;         // REQUIRED â€” URL base del backend
>   userToken?:          string;         // Bearer token (Keycloak)
>   useBackendFeatures?: boolean;        // default: true
>   isExample?:          boolean;        // default: false
> }
> ```
> La promesa devuelve `GoMap` (fachada principal, ARCH Â§4 GoMap). Todos los mÃ©todos de `map.*` descritos en este documento son mÃ©todos de esa clase.

```javascript
// static/js/scripts.js
import { loadScripts } from "@baseScripts";
import config from "../../config.json";
import { exampleCode } from "./exampleCode";

window.onload = () => {
    loadScripts({ config, exampleCode }).then((map) => {
        // map â†’ instancia de GoMap, resuelta tras autenticaciÃ³n Keycloak + GoMapLoader.loadGoMap
        console.log("Mapa cargado:", map);
    });
};
```

**Opciones de `loadScripts`:**

| OpciÃ³n | Tipo | Por defecto | DescripciÃ³n |
|--------|------|-------------|-------------|
| `config` | `object` | **requerido** | El objeto `config.json` completo |
| `exampleCode` | `object` | opcional | Cadenas de cÃ³digo mostradas en la pestaÃ±a de docs |
| `shouldLoadMap` | `boolean` | `true` | Si `false`, resuelve solo el token sin cargar el mapa |
| `useBackendFeatures` | `boolean` | `true` | Si `false`, omite auth y carga el mapa sin token |
| `authenticate` | `boolean` | `true` | Activa/desactiva login Keycloak |

**Internamente llama a:**

```javascript
GoMapLoader.loadGoMap({
    mapConfig:   config,
    enviroment:  "https://gateway-proxy-gis-devtools.go-aigua.com",
    userToken:   token,      // token Bearer de Keycloak
    isExample:   true,
})
.then((map) => { /* instancia GoMap */ });
```

---

### 3b. `config.json` â€” Estructura raÃ­z

> **[ARCH Â§11 Tipos clave]** El JSON completo tiene el tipo TypeScript `configOptions = { GOMapOptions: Array<GOMapOptions> }`. Cada elemento de `GOMapOptions` tiene el tipo `GOMapOptions` con los campos obligatorios: `id`, `div`, `lang`, `ui`, `optionView`, `translations`. Los campos `optionToc`, `layer`, `interactions`, `projections` y `filter` son opcionales.

```json
{
  "GOMapOptions": [
    {
      "id":           "main",
      "div":          "mapa",
      "lang":         "es",
      "optionView":   { ... },
      "optionToc":    { ... },
      "ui":           { ... },
      "layer":        [ ... ],
      "interactions": { ... },
      "translations": { ... }
    }
  ]
}
```

> El array `GOMapOptions` permite definir **mÃºltiples mapas** en la misma pÃ¡gina.

---

### 3c. `optionView` â€” ConfiguraciÃ³n de vista

> **[ARCH Â§11 Tipos clave â€” `optionsView`]** Campos obligatorios: `id` (string), `center` ([x,y] en la proyecciÃ³n activa), `zoom` (number). El campo `projection` es el cÃ³digo EPSG **sin prefijo** (`"3857"` o `"4326"`; default `"3857"`). En cÃ³digo TypeScript, el tipo es `optionsView`. `GoView` (ARCH Â§4 GoView) extiende `OL.View` y expone `getMapsLinked()` para vistas sincronizadas (`map.bindViews`).

```json
"optionView": {
    "id":         "vista1",
    "center":     [-79850, 4789551],
    "zoom":       14,
    "projection": "3857",
    "maxZoom":    22,
    "minZoom":    5
}
```

| Campo | Tipo | DescripciÃ³n |
|-------|------|-------------|
| `id` | `string` | Identificador de la vista |
| `center` | `[x, y]` | Centro en la proyecciÃ³n configurada |
| `zoom` | `number` | Zoom inicial |
| `projection` | `string` | CÃ³digo EPSG (`"3857"` o `"4326"`) |
| `extent` | `[minX, minY, maxX, maxY]` | LÃ­mites de navegaciÃ³n |
| `rotation` | `number` | RotaciÃ³n inicial en radianes |
| `maxZoom` | `number` | Zoom mÃ¡ximo permitido |
| `minZoom` | `number` | Zoom mÃ­nimo permitido |
| `multiWorld` | `boolean` | Permite repeticiÃ³n horizontal del mundo |

---

### 3d. `optionToc` â€” Tabla de contenidos

> **[ARCH Â§8 TOC â€” `optionsToc` + `folderOptions`]** Campos obligatorios del tipo `optionsToc`: `id` (ID del elemento HTML), `active`, `maxHeight`, `legend`. Las carpetas se definen con el tipo `folderOptions`: `folderName` (string, **debe coincidir con `urlFolder` de la capa** incluyendo `/` final), `onlyOneVisibleLayer` (radio-button), `hasSwitcher`, `folderExpandedByDefault`. El componente real es `GoOwnToc` (ARCH Â§8), accesible via `map.getStaging(id).getOwnToc()`.

```json
"optionToc": {
    "id":        "toc-container",
    "active":    true,
    "maxHeight": 380,
    "legend":    true,
    "foldersOptions": [
        {
            "folderName":          "Capas Base/",
            "onlyOneVisibleLayer": true,
            "hasSwitcher":         false
        },
        {
            "folderName":                 "Redes/",
            "folderExpandedByDefault":    true
        }
    ]
}
```

- Las capas se agrupan en carpetas del TOC usando su propiedad `urlFolder`.
- `onlyOneVisibleLayer: true` crea un selector tipo radio (usado para mapas base).
- `toc: false` en una capa la oculta del TOC sin eliminarla del mapa.
- `toc_link` permite enlazar capas para que su toggle actÃºe sobre varias a la vez.

---

### 3e. `ui` â€” Botones del mapa

> **[ARCH Â§9 UIMap â€” `UIConfig` interface]** Todos los campos son `boolean` opcionales. El componente subyacente es `UIDisplay` dentro de `GOStagingMap`. La orientaciÃ³n de los botones se controla con `enum IOrientation { LEFT = "LEFT", RIGHT = "RIGHT" }`.

```json
"ui": {
    "showFullScreenButton":            true,
    "showLayersPreviewer":             true,
    "showAddressSearch":               true,
    "showUserLocationButton":          true,
    "showInfoButton":                  false,
    "showLinearMeasurementButton":     true,
    "showPolygonMeasurementButton":    true,
    "showFilterButton":                true,
    "showMap3DButton":                 false,
    "showExportButton":                true
}
```

---

### 3f. `translations` â€” i18n en config

> **[ARCH Â§10 i18nService]** El servicio singleton `i18nService` envuelve `i18next`. Los idiomas soportados son `SupportedLangs = "en" | "es" | "ca" | "fr" | "ro" | "it"` (siempre minÃºsculas). Los tokens `{{__key__}}` en `alias` y `urlFolder` se resuelven contra el objeto `translations` del JSON. En runtime, `addOrUpdateTranslations` de `GOStagingMap` permite inyectar traducciones sin recargar.

Los valores de `alias` y `urlFolder` de las capas pueden usar tokens `{{__key__}}`:

```json
"alias":    "{{__layers.base.dark__}}",
"urlFolder":"{{__folders.base__}}/"
```

```json
"translations": {
    "es": {
        "layers.base.dark": "Oscura",
        "folders.base":     "Capas Base"
    },
    "en": {
        "layers.base.dark": "Dark",
        "folders.base":     "Base Layers"
    }
}
```

**API de idioma en runtime:**

```javascript
map.setLanguage("en");
map.getLanguage();

// AÃ±adir traducciones dinÃ¡micamente
map.getStaging("main").addOrUpdateTranslations({
    myKey: { es: "Mi capa", en: "My layer" }
});
```

---

## 4. Patrones de capas

> **[ARCH Â§5 Sistema de Capas]** Toda capa hereda de `GOLayer` (abstracta) â€” o de `GOStyledLayer` cuando tiene estilos SVG/SLD/GeoJSON. El constructor recibe un objeto `layerOptions` (ARCH Â§5 `layerOptions`). Los tres campos **obligatorios** son siempre `id`, `alias` y `layerType`. El enum `GOLayerType` contiene los 19 valores vÃ¡lidos para `layerType`.
> 
> JerarquÃ­a de clases relevante (ARCH Â§5):
> ```
> GOLayer (abstracta)
> â”œâ”€â”€ GOStyledLayer (abstracta) â† capas con iconos/SLD/GeoJSON style
> â”‚   â”œâ”€â”€ GOVectorLayer   (GO_VECTOR_LAYER)  â† WFS/GeoServer bbox
> â”‚   â”œâ”€â”€ GOGeoJsonLayer  (GO_GEOJSON_LAYER)
> â”‚   â”œâ”€â”€ GOWFSLayer      (GO_WFS_LAYER)
> â”‚   â”œâ”€â”€ GOClusterLayer  (GO_CLUSTER_LAYER)
> â”‚   â”œâ”€â”€ GoHeatMapLayer  (GO_HEATMAP_LAYER)
> â”‚   â”œâ”€â”€ GoHexGridLayer  (GO_HEXGRID_LAYER)
> â”‚   â”œâ”€â”€ GoIOTLayer      (GO_IOT_LAYER)
> â”‚   â”œâ”€â”€ GoTruckLayer    (GO_TRUCK_LAYER)
> â”‚   â”œâ”€â”€ GoWebGlLayer    (GO_WEBGL_LAYER)
> â”‚   â””â”€â”€ GOVectorTiles   (GO_VECTOR_TILE)
> â”œâ”€â”€ GOWMSLayer      (GO_WMS_LAYER)   â† tile, no filtra CQL
> â”œâ”€â”€ GoXYZ           (GO_XYZ / OSM)
> â”œâ”€â”€ GoMapBoxLayer   (GO_MAPBOX)
> â””â”€â”€ GoEsriBaseMap   (GO_ESRI_BASEMAP)
> ```

Todos los objetos de capa comparten estos campos comunes:

| Campo | Tipo | DescripciÃ³n |
|-------|------|-------------|
| `id` | `string` | Identificador Ãºnico, usado para acceder a la capa por cÃ³digo |
| `alias` | `string` | Nombre visible (soporta tokens `{{__key__}}`) |
| `layerType` | `string` | Tipo de capa (ver lista) |
| `isBaseLayer` | `boolean` | `true` = capa base (solo una visible a la vez por carpeta) |
| `urlFolder` | `string` | Carpeta del TOC (ej: `"Assets/"`) |
| `defaultVisibility` | `boolean` | Visibilidad inicial |
| `opacity` | `number` | Opacidad `0â€“1` |
| `zIndex` | `number` | Orden de renderizado |
| `legend` | `boolean` | Mostrar en leyenda |
| `toc` | `boolean` | Mostrar en TOC |
| `minResolution` / `maxResolution` | `number` | Escala de visibilidad |
| `toc_link` | `string[]` | IDs de capas enlazadas en el TOC |

---

### 4a. Mapas base

> **[ARCH Â§5 layerOptions]** Campos usados: `id`, `layerType`, `isBaseLayer: true`, mÃ¡s los especÃ­ficos de cada driver: `style` + `token` (Mapbox), `url` (XYZ), `idEsri` + `esriApiKey` (ESRI). Los mapas base deben tener `isBaseLayer: true` para que el TOC los ponga en modo radio-button (requiere `onlyOneVisibleLayer: true` en su `folderOptions`).

**MapBox:**
```json
{
    "id":          "DarkTheme",
    "layerType":   "GO_MAPBOX",
    "isBaseLayer": true,
    "style":       "cljzp2t2r008b01qr8izjchch",
    "token":       "pk.eyJ1..."
}
```

**XYZ (Google Satellite / OSM / cualquier tile XYZ):**
```json
{
    "id":          "satelite",
    "layerType":   "GO_XYZ",
    "isBaseLayer": true,
    "url":         "https://mt0.google.com/vt/lyrs=s&hl=en&x={x}&y={y}&z={z}"
}
```

**ESRI Basemap:**
```json
{
    "id":          "LightTheme",
    "layerType":   "GO_ESRI_BASEMAP",
    "isBaseLayer": true,
    "idEsri":      "8ecc59437f824235b660af20cc1b08c8",
    "esriApiKey":  "AAPT85..."
}
```

---

### 4b. Capa Vectorial GeoServer (`GO_VECTOR_LAYER`)

> **[ARCH Â§5 GOVectorLayer]** Clase: `GOVectorLayer extends GOStyledLayer`. Estrategia de carga configurable: `useBboxFilter: true` carga solo el bbox visible (recomendado para capas grandes), `false` carga todas las features. Endpoints: WFS de GeoServer. MÃ©todos clave en runtime (sobre la instancia obtenida con `map.getStaging(id).getLayer(layerId)`): `addFilter(cql, reload?)`, `removeFilter()`, `addFilterNoCQL(GoFilter)`, `removeFilterNoCQL()`, `getFeatures()`, `changeStyle(styleFromUser)`, `isLoadedData(timeInterval, limit)`.

WFS desde GeoServer con estrategia bbox o all-features.

```json
{
    "id":               "tuberias",
    "alias":            "TuberÃ­as",
    "layerType":        "GO_VECTOR_LAYER",
    "url":              "https://my-geoserver.com/geoserver",
    "layerName":        "water:tuberias",
    "layerProjection":  "3857",
    "useBboxFilter":    true,
    "defaultVisibility": true,
    "urlFolder":        "Red/",
    "zIndex":           3,
    "legend":           true,
    "sldStyle": {
        "url":      "geoserver/rest/styles/network_main.sld",
        "mapUnits": true
    }
}
```

---

### 4c. Capa WMS (`GO_WMS_LAYER`)

> **[ARCH Â§5 GOWMSLayer]** Clase: `GOWMSLayer extends GOLayer`. Usa `AuthTileWMS` (extensiÃ³n personalizada de OL `TileWMS` con auth automÃ¡tica). `canFilter()` devuelve `false` â€” **no admite filtros CQL**. Soporta `geoparams`, `sldStyle`, `version`, `minResolution`, `maxResolution`. Para interrogaciÃ³n por clic usar `addWMSGetFeatureInfo` de `GoInteractions` (ARCH Â§6 ExternalsConnections).

```json
{
    "id":        "sismos",
    "alias":     "Estaciones sismolÃ³gicas",
    "layerType": "GO_WMS_LAYER",
    "url":       "https://www.ign.es/wms-inspire/geofisica",
    "layerName": "GE.Geophysics.seismologicalStation",
    "esri":      false,
    "urlFolder": "NGE/"
}
```

**AÃ±adir en runtime:**
```javascript
map.addNewLayer("main", layerConfig);
```

---

### 4d. Capa WFS externa (`GO_WFS_LAYER`)

> **[ARCH Â§5 GOWFSLayer]** Clase: `GOWFSLayer extends GOStyledLayer`. Diferencias frente a `GO_VECTOR_LAYER`: soporta ESRI FeatureServer (`esri: true`, `WFStoken`), autenticaciÃ³n bÃ¡sica (`user`/`password`), y no requiere GeoServer como backend. Campos especÃ­ficos: `esri`, `useBboxFilter`, `WFStoken`, `user`, `password`.

OGC WFS o ESRI FeatureServer sin autenticaciÃ³n de backend.

```json
{
    "id":               "redesgeodesicas",
    "alias":            "Redes geodÃ©sicas",
    "layerType":        "GO_WFS_LAYER",
    "url":              "https://www.ign.es/wfs/redes-geodesicas/wfs?",
    "layerName":        "RED_ERGNSS",
    "layerProjection":  "4326",
    "esri":             false,
    "useBboxFilter":    false
}
```

**ESRI FeatureServer:**
```json
{
    "id":          "esriLayer",
    "layerType":   "GO_WFS_LAYER",
    "url":         "https://services.arcgis.com/.../FeatureServer/0",
    "esri":        true,
    "WFStoken":    "my-esri-token"
}
```

---

### 4e. Capa GeoJSON (`GO_GEOJSON_LAYER`)

> **[ARCH Â§5 GOGeoJsonLayer]** Clase: `GOGeoJsonLayer extends GOStyledLayer`. Acepta `url` **O** `data` (nunca los dos). Usa `VectorImageLayer` por defecto; `VectorLayer` si `disableImageEffect: true`. Soporta reproyecciÃ³n `layerProjection` â†’ `toProjection`. MÃ©todos en runtime: `addFeatures(geojson)`, `refresh()`. Para inyecciÃ³n masiva de datos usa `map.injectDataGeoJson(mapId, layerId, clear, featureCollection)` (ARCH Â§4 GoMap mÃ©todos de capa).

Datos estÃ¡ticos en cliente (URL o inline).

**Desde URL:**
```json
{
    "id":          "geojson_poligonos",
    "alias":       "PolÃ­gonos",
    "layerType":   "GO_GEOJSON_LAYER",
    "url":         "https://my-server.com/data/polygons.geojson",
    "urlFolder":   "Datos/",
    "styleGeojson": { ... }
}
```

**Inline (datos cargados por cÃ³digo):**
```json
{
    "id":                  "geojson_points",
    "layerType":           "GO_GEOJSON_LAYER",
    "disableImageEffect":  true
}
```

```javascript
// AÃ±adir features tras la carga
const layer = map.getStaging("main").getLayer("geojson_points");
layer.addFeatures({
    default: {
        type: "FeatureCollection",
        features: [
            {
                type: "Feature",
                geometry: { type: "Point", coordinates: [-0.377, 39.469] },
                properties: { id: 1, name: "Punto 1" }
            }
        ]
    }
});
layer.refresh(); // recarga / limpia features
```

---

### 4f. Capa ClÃºster (`GO_CLUSTER_LAYER`)

> **[ARCH Â§5 GOClusterLayer]** Clase: `GOClusterLayer extends GOStyledLayer`. Tres niveles de icono: `styleSVG` (elemento suelto), `styleSVGClusterInd` (grupo de 1), `styleSVGCluster` (grupo mÃºltiple). Todos son de tipo `ISVGIcon` (ARCH Â§11). El campo `distance` (pÃ­xeles) controla el radio de agrupaciÃ³n. `maxResolutionCluster` permite degradar el clÃºster a puntos sueltos al hacer zoom. MÃ©todo clave: `getFeaturesAtPixel(pixel)`:  devuelve las features OL del clÃºster clicado.

```json
{
    "id":                  "sensores",
    "alias":               "Sensores",
    "layerType":           "GO_CLUSTER_LAYER",
    "url":                 "https://my-geoserver.com/geoserver",
    "layerName":           "water:sensors",
    "layerProjection":     "3857",
    "distance":            50,
    "disableImageEffect":  true,
    "envelope":            true,
    "styleSVG": {
        "iconSvg":         "007_01_supply_point",
        "shape":           "marker",
        "backgroundColor": "#ffffff",
        "mode":            "inactive",
        "scale":           1,
        "anchor":          [0.5, 0.5]
    },
    "styleSVGClusterInd": {
        "iconSvg":         "007_01_supply_point",
        "shape":           "stack",
        "backgroundColor": "#76b7d6",
        "mode":            "inactive",
        "scale":           1
    },
    "styleSVGCluster": {
        "iconSvg":         "007_01_supply_point",
        "shape":           "cluster",
        "backgroundColor": "#319ACB",
        "mode":            "inactive",
        "scale":           1,
        "fontColor":       "rgba(255,0,0,1)",
        "fontSize":        "10px"
    }
}
```

**Clic en clÃºster:**
```javascript
const olMap = map.getMap("main");
const clusterLayer = map.getStaging("main").getLayer("sensores");

olMap.on("singleclick", (evt) => {
    const features = clusterLayer.getFeaturesAtPixel(evt.pixel);
    if (features) {
        features.forEach(f => console.log(f.getProperties()));
    }
});
```

---

### 4g. Capa HeatMap (`GO_HEATMAP_LAYER`)

> **[ARCH Â§5 GoHeatMapLayer]** Clase: `GoHeatMapLayer extends GOStyledLayer`. Campos especÃ­ficos en `layerOptions`: `radiusHeatMap` (nÃºmero de pÃ­xeles del radio), `blurHeatMap` (desenfoque), `fieldHeatMap` (campo numÃ©rico de la feature usado como peso). Basado en `OL.HeatmapLayer`.

```json
{
    "id":              "heatmap",
    "alias":           "Densidad",
    "layerType":       "GO_HEATMAP_LAYER",
    "url":             "https://my-geoserver.com/geoserver",
    "layerName":       "water:incidents",
    "layerProjection": "3857",
    "radiusHeatMap":   15,
    "blurHeatMap":     20,
    "fieldHeatMap":    "weight"
}
```

---

### 4h. Capa HexGrid (`GO_HEXGRID_LAYER`)

> **[ARCH Â§5 GoHexGridLayer]** Clase: `GoHexGridLayer extends GOStyledLayer`. Campos especÃ­ficos: `hexSize` (tamaÃ±o del hexÃ¡gono en unidades de proyecciÃ³n), `hexGridResolution`, `styleHexBin` (tipo `hexGridColor` con `field` + `colorRanges`). Las condiciones de agrupaciÃ³n usan `enum HexConditions { AVG, SUM, COUNT }` (ARCH Â§11 Enums).

```json
{
    "id":                "hexgrid",
    "alias":             "Hexagonos",
    "layerType":         "GO_HEXGRID_LAYER",
    "hexSize":           500,
    "hexGridResolution": 3,
    "styleHexBin": {
        "field":          "count",
        "colorRanges": [
            { "min": 0,  "max": 5,  "color": "#fee5d9" },
            { "min": 5,  "max": 15, "color": "#fc9272" },
            { "min": 15, "max": 999,"color": "#de2d26" }
        ]
    }
}
```

---

### 4i. Capa IoT (`GO_IOT_LAYER`)

> **[ARCH Â§5 GoIOTLayer]** Clase: `GoIOTLayer extends GOStyledLayer`. Campos especÃ­ficos: `reloadFrequency` (intervalo en ms), `labelNameIOT` (campo de etiqueta), `iotUrl`, `iotToken`, `iotVersion`. El servicio subyacente es `GoIOTNexus` (ARCH Â§7 Services). Estado IoT gestionado via `GoStore.getState().iotConfig` (ARCH Â§7 GoState).

Capa vectorial con auto-recarga periÃ³dica.

```json
{
    "id":               "iot_sensors",
    "alias":            "Sensores IoT",
    "layerType":        "GO_IOT_LAYER",
    "url":              "https://my-backend.com/geoserver",
    "layerName":        "iot:sensors",
    "layerProjection":  "3857",
    "reloadFrequency":  30000
}
```

---

### 4j. Capa Truck / Fleet (`GO_TRUCK_LAYER`)

> **[ARCH Â§5 GoTruckLayer]** Clase: `GoTruckLayer extends GOStyledLayer`. Campos especÃ­ficos en `layerOptions`: `controlPoints` (`Array<controlPoints>`), `listener` (`Array<listenerControl>`), `timeNoSignal`, `timeWorking`, `timeNexusPosition`, `timeReSyncRoute`. Los eventos se escuchan sobre la instancia de capa con `.on(GoTruckLayerEvents.*, callback)`.

Seguimiento de vehÃ­culos en tiempo real.

```json
{
    "id":        "truck",
    "alias":     "Flota",
    "layerType": "GO_TRUCK_LAYER",
    "url":       "https://my-backend.com/geoserver",
    "layerName": "dmd:dmd_goaigua_truck",
    "controlPoints": [
        {
            "id":           "parking",
            "style":        "063_01_parking",
            "finishRoute":  true,
            "visibleTruck": false
        },
        {
            "id":            "actuaciones",
            "style":         "025_03_work_orders",
            "visibleTruck":  true,
            "fieldControl":  "truck",
            "sendEvent":     "true"
        }
    ]
}
```

**Eventos de la capa truck:**
```javascript
import { GoTruckLayerEvents } from "@/index";

const truckLayer = map.getStaging("main").getLayer("truck");

truckLayer.once(GoTruckLayerEvents.TRUCKS_LOADED, () => {
    console.log("Camiones cargados");
});

truckLayer.on(GoTruckLayerEvents.TRUCK_EXITING_FROM_INTERSECTION, ({ data }) => {
    console.log("CamiÃ³n saliendo de intersecciÃ³n:", data);
});
```

---

### 4k. Capa WebGL (`GO_WEBGL_LAYER`)

> **[ARCH Â§5 GoWebGlLayer]** Clase: `GoWebGlLayer extends GOStyledLayer`. Optimizado para datasets de miles de puntos. Soporta estilos condicionales via `styleSVGWebGLConditional` (tipo `styleSVGWebGLConditional` en `layerOptions`, ARCH Â§5). Las condiciones usan los mismos operadores que `GoFilterCondition` en formato string (ej: `"EQUAL TO"`). Para filtros en runtime, `addFilter(cql)` vÃ­a ARCH Â§6 tambiÃ©n funciona sobre esta capa.

Renderizado de puntos de alto rendimiento con WebGL.

```json
{
    "id":        "webgl_points",
    "alias":     "Puntos WebGL",
    "layerType": "GO_WEBGL_LAYER",
    "url":       "https://my-geoserver.com/geoserver",
    "layerName": "water:big_dataset",
    "styleSVGWebGLConditional": {
        "field": "status",
        "styleInfoSVG": [
            {
                "condition": { "type": "EQUAL TO", "condition": "active" },
                "styleSVG":  { "iconSvg": "008_01_valve", "mode": "inactive" }
            },
            {
                "condition": { "type": "EQUAL TO", "condition": "alarm" },
                "styleSVG":  { "iconSvg": "008_01_valve", "mode": "warning-yellow" }
            }
        ]
    }
}
```

---

### 4l. Capa Vector Tiles (`GO_VECTOR_TILE`)

> **[ARCH Â§5 GOVectorTiles]** Clases: `GOVectorTiles` (fuente externa) y `GOVectorTilesOwn` (fuente propia, `GO_VECTOR_TILE_OWN`). El campo `vt_type` discrimina el tipo de tile. Servicio de soporte para tiles propios: `GoVectorTileOwnServices` (ARCH Â§7 Services).

```json
{
    "id":        "vector_tiles",
    "alias":     "Tiles Vectoriales",
    "layerType": "GO_VECTOR_TILE",
    "url":       "https://my-server.com/tiles/{z}/{x}/{y}.pbf"
}
```

---

### 4m. Remote Sensing (`GO_REMOTE_SENSING`)

> **[ARCH Â§5 GoRemoteSensing]** Clases: `GoRemoteSensing`, `GoVerticalRemoteSensing` (`remoteSensingType: "vertical"`), `GoWaterLeakRemoteSensing` (`remoteSensingType: "waterLeak"`). Campos especÃ­ficos: `rsClientID`, `plotsIds`, `rsInitialDate`/`rsEndDate` (Unix timestamps), `vrsColors` (array de colores para RS vertical). Servicio backend: `GoRSService` (ARCH Â§7). URL base resuelta via `GoStore.getState().enviroment`.

```json
{
    "id":                "rs_layer",
    "alias":             "TeledetecciÃ³n",
    "layerType":         "GO_REMOTE_SENSING",
    "url":               "https://my-backend.com",
    "remoteSensingType": "NDVI"
}
```

---

## 5. Patrones de estilo

> **[ARCH Â§5 GOStyledLayer / Â§11 Tipos clave]** Los estilos se aplican como campos del objeto `layerOptions`. La clase `GOStyledLayer` gestiona el ciclo de vida del estilo (SLD, SVG, GeoJSON). La clase `GoStyle` (ARCH Â§11 Utils) envuelve `OL.Style` y aplica `styleFromUser` o `ISVGIcon`. `StyleUtils.editStyle(styleFromUser)` convierte el objeto de usuario a un `OL.Style` nativo. Los estilos se pueden cambiar en runtime via `layer.changeStyle(styleFromUser)` sobre cualquier `GOStyledLayer`.

---

### 5a. Icono SVG (`styleSVG`)

> **[ARCH Â§11 Tipos clave â€” `ISVGIcon` interface]** Todos los campos son opcionales excepto como mÃ­nimo uno de `iconSvg` o `iconUrl`. `shape` acepta el enum `IconShape { MARKER, PIN, CLUSTER, BADGE, STACK }`. `mode` acepta `IconMode { SELECTED, INACTIVE, WARNING_YELLOW, WARNING_ORANGE, ERROR }` o un color hex directamente. `rotationField` permite rotaciÃ³n dinÃ¡mica por campo de la feature, con `rotationUnits` (`"deg"` | `"grad"` | `"rad"`).

```json
"styleSVG": {
    "iconSvg":         "007_01_supply_point",
    "shape":           "marker",
    "backgroundColor": "#ffffff",
    "mode":            "inactive",
    "scale":           1,
    "anchor":          [0.5, 0.5],
    "elevation":       0,
    "iconColor":       "#000000"
}
```

| Campo | Valores | DescripciÃ³n |
|-------|---------|-------------|
| `iconSvg` | nombre de icono | Nombre del icono de la librerÃ­a (sin extensiÃ³n) |
| `iconUrl` | URL | URL externa de icono (alternativa a `iconSvg`) |
| `shape` | `"marker"` \| `"stack"` \| `"cluster"` | Forma del icono |
| `mode` | `"inactive"` \| `"selected"` \| `"warning-yellow"` \| `"success"` \| color hex | Estado/color del icono |
| `anchor` | `[x, y]` (0â€“1) | Punto de anclaje del icono |
| `scale` | `number` | Escala |
| `backgroundColor` | color hex | Color de fondo del marcador |
| `iconColor` | color hex | Color del icono SVG |
| `size` | `[width, height]` | TamaÃ±o personalizado en px |

---

### 5b. Icono SVG condicional (`styleSVGConditional`)

> **[ARCH Â§5 layerOptions â€” `styleSVGConditional`]** Tipo: `styleSVG` (array de entradas `{ legendAlias, condition, styleSVG: ISVGIcon }`). El campo `field` es el nombre del atributo de la feature. Las condiciones usan los operadores del enum `GoFilterCondition` (ARCH Â§11 Enums) en su valor string (`"EQUAL TO"`, `"GREATER THAN"`, `"IS IN"`, `"IS NULL"`, etc.). Para WebGL, usar `styleSVGWebGLConditional` con la misma estructura.

Cambia el icono en funciÃ³n del valor de un campo de atributo.

```json
"styleSVGConditional": {
    "field": "active",
    "styleInfoSVG": [
        {
            "legendAlias": "Sensor activo",
            "condition":   { "type": "EQUAL TO", "condition": true },
            "styleSVG":    {
                "iconSvg":         "008_01_valve",
                "shape":           "marker",
                "mode":            "inactive",
                "backgroundColor": "#ffffff",
                "scale":           1,
                "anchor":          [0.5, 0.5]
            }
        },
        {
            "legendAlias": "Sensor inactivo",
            "condition":   { "type": "EQUAL TO", "condition": false },
            "styleSVG":    {
                "iconSvg":         "008_01_valve",
                "shape":           "marker",
                "mode":            "warning-yellow",
                "backgroundColor": "#fff3cd",
                "scale":           1,
                "anchor":          [0.5, 0.5]
            }
        }
    ]
}
```

**Tipos de condiciÃ³n:**
- `"EQUAL TO"` â€” igual a un valor
- `"GREATER THAN"` / `"LESS THAN"` â€” comparaciÃ³n numÃ©rica
- `"IS IN"` â€” contenido en un array de valores
- `"IS NULL"` / `"IS NOT NULL"` â€” existencia del campo

---

### 5c. Estilo GeoJSON temÃ¡tico (`styleGeojson`)

> **[ARCH Â§5 layerOptions â€” `styleGeojson`]** Usar con `GO_GEOJSON_LAYER`, `GO_WFS_LAYER` y `GO_VECTOR_LAYER`. Cada entrada de `styleInfo` usa el tipo `styleFromUser` (ARCH Â§11): campos `type` (`GOGeometryType`: `"POINT"`, `"LINE_STRING"`, `"POLYGON"`, `"ALL"`), `stroke` (`{color, width?}`), `fill` (`{color}`), `graphic` (`{typePoint, size}`). El tipo `"ALL"` es el mÃ¡s seguro cuando la geometrÃ­a puede ser mixta.

Estilo basado en el valor de un campo, con soporte para puntos, lÃ­neas y polÃ­gonos.

```json
"styleGeojson": {
    "field": "estado",
    "styleInfo": [
        {
            "legendAlias": "Activo",
            "condition":   { "type": "EQUAL TO", "condition": "ok" },
            "style": {
                "type":    "POINT",
                "graphic": { "typePoint": "circle", "size": 10 },
                "stroke":  { "color": "#232323", "width": 0.5 },
                "fill":    { "color": "rgba(100,100,100,1)" }
            }
        },
        {
            "legendAlias": "Inactivo",
            "condition":   { "type": "EQUAL TO", "condition": "ko" },
            "style": {
                "type":    "POINT",
                "graphic": { "typePoint": "circle", "size": 10 },
                "stroke":  { "color": "#ff0000", "width": 0.5 },
                "fill":    { "color": "rgba(255,0,0,0.5)" }
            }
        }
    ]
}
```

**Para lÃ­neas:**
```json
"style": {
    "type":   "LINE_STRING",
    "stroke": { "color": "rgba(0,122,255,1)", "width": 2 }
}
```

**Para polÃ­gonos:**
```json
"style": {
    "type":   "POLYGON",
    "stroke": { "color": "#333333", "width": 1 },
    "fill":   { "color": "rgba(51,51,51,0.3)" }
}
```

**Tipo `"ALL"`** â€” aplica el mismo estilo a cualquier geometrÃ­a:
```json
"style": {
    "type":   "ALL",
    "stroke": { "color": "#FF5858", "width": 2 },
    "fill":   { "color": "rgba(255,88,88,0.2)" }
}
```

---

### 5d. Estilo SLD (servidor) (`sldStyle`)

> **[ARCH Â§5 GOStyledLayer â€” `sldLayer`, `sldObject`]** El campo `sldStyle` en `layerOptions` tiene el tipo `styleOptions`. La clase `GOStyledLayer` expone `sldLayer` y `sldObject` para acceso al SLD parseado. El parser SLD interno estÃ¡ en `src/goapi/Utils/sldreader` (ARCH Â§11 Utils). Usarlo con `GO_VECTOR_LAYER` o `GO_WMS_LAYER` (en WMS, junto con el parÃ¡metro `SLD_BODY` automÃ¡tico).

Estilo gestionado en GeoServer, cargado como SLD.

```json
"sldStyle": {
    "url":      "geoserver/rest/styles/mi_estilo.sld",
    "mapUnits": true
}
```

---

## 6. Patrones de interacciÃ³n

> **[ARCH Â§6 MÃ³dulo Interactions]** Todas las herramientas interactivas se obtienen via `map.getInteractions()` que devuelve la instancia compartida de `GoInteractions`. Las herramientas se indexan por `(idMap, idInteraction)` â€” ambos deben ser strings Ãºnicos. Los mÃ³dulos son: `Analysis`, `Features`, `Selection`, `ExternalsConnections`, `Hydraulics`, `Views`, `geoTools`. Las firmas completas de todos los mÃ©todos estÃ¡n en ARCH Â§6 Firmas de mÃ©todos clave.

---

### 6a. Clic en el mapa

> **[ARCH Â§4 GoMap â€” `onMap(idMap, type, callback)`]** El enum `typeOn` (ARCH Â§11) lista todos los eventos OL disponibles: `"click"`, `"singleclick"`, `"dblclick"`, `"moveend"`, `"movestart"`, `"pointerdrag"`, `"pointermove"`, `"postrender"`, `"rendercomplete"`. `map.onMap` delega en `OlMap.on()` del core de OpenLayers. `map.getMap(id)` devuelve el `OlMap` nativo si se necesita acceso directo.

**Con OpenLayers nativo:**
```javascript
const olMap = map.getMap("main");
olMap.on("singleclick", (evt) => {
    const features = olMap.getFeaturesAtPixel(evt.pixel);
    if (features.length > 0) {
        features.forEach(f => {
            const props = f.getProperties();
            // omitir el campo "geometry"
            Object.entries(props)
                .filter(([k]) => k !== "geometry")
                .forEach(([k, v]) => console.log(k, ":", v));
        });
    }
});
```

**Con el wrapper del mapa:**
```javascript
map.onMap("main", "singleclick", (evt) => {
    const features = map.getMap("main").getFeaturesAtPixel(evt.pixel);
    // ...
});
```

---

### 6b. Highlight

> **[ARCH Â§6 Features â€” `GoHighlight`]** Firma: `addHighlight({ idMap, idHighlight, features: Feature[], style?: styleFromUser, textFunction?: Function, styleSVG?: ISVGIcon, zIndex?: number }): GoHighlight`. Permite `changeStyleOfHighlight` para cambiar el estilo en vuelo sin recrear el highlight. `hasHighlight({ idMap, idHighlight })` comprueba existencia antes de operar. `features` es un array de `OL.Feature` â€” obtenerlas con `layer.getFeatures()` o `getFeaturesAtPixel`.

```javascript
const interactions = map.getInteractions();

// AÃ±adir resaltado
interactions.addHighlight({
    idMap:       "main",
    idHighlight: "resaltado1",
    features:    [feature],
    style: {
        type:   "ALL",
        stroke: { color: "#FF0000", width: 3 },
        fill:   { color: "rgba(255,0,0,0.2)" }
    }
});

// Comprobar existencia
interactions.hasHighlight({ idMap: "main", idHighlight: "resaltado1" });

// Cambiar estilo
interactions.changeStyleOfHighlight({
    idMap:       "main",
    idHighlight: "resaltado1",
    userStyle:   { type: "ALL", fill: { color: "rgba(0,255,0,0.2)" } }
});

// Eliminar
interactions.removeHighlight({ idMap: "main", idHighlight: "resaltado1" });
```

---

### 6c. Hover

> **[ARCH Â§6 Features â€” `GoHover`]** Firma: `mouseHover({ idMap, idHover, layer?: string, style?: styleFromUser, callback?: Function, styleSVG?: ISVGIcon, zindex?: number }): GoHover | void`. El campo `layer` es el `id` de la capa sobre la que activa el hover (si se omite, activa sobre todas). `addTooltipHoverFeature({ idMap, idHover, featuresGeom, style? })` aÃ±ade un tooltip HTML personalizado. Siempre usar `removeHover` al desmontar para evitar memory leaks.

```javascript
const interactions = map.getInteractions();

interactions.mouseHover({
    idMap:    "main",
    idHover:  "hover1",
    layer:    "tuberias",
    callback: (feature) => {
        const props = feature.getProperties();
        mostrarTooltip(props);
    },
    style: {
        type:   "ALL",
        stroke: { color: "#FFCC00", width: 4 },
        fill:   { color: "rgba(255,204,0,0.3)" }
    }
});

// Eliminar hover
interactions.removeHover({ idMap: "main", idHover: "hover1" });
```

---

### 6d. Buffer + IntersecciÃ³n

> **[ARCH Â§6 geoTools â€” `GoBuffer` + `GoIntersect`]** `addBuffer` firma: `({ idMap, idBuffer, dataBuffer: { bufferData: Feature|GeoJSON, distance: number } }): Promise<{ serviceBuffer: GeoJSON }>`. `addIntersect` firma: `({ idMap, idIntersect, intersectDataIn: { type: "JSON"|"LAYER", data: string }, intersectDataClip: { type: "JSON"|"LAYER", data: string }, style: styleFromUser }): CancelablePromise`. El tipo `"LAYER"` en `intersectDataClip` usa el `id` de la capa ya cargada en el mapa; `"JSON"` usa un string GeoJSON serializado.

```javascript
const interactions = map.getInteractions();

// 1. Crear buffer sobre una feature
interactions.addBuffer({
    idMap:      "main",
    idBuffer:   "buf1",
    dataBuffer: {
        bufferData: feature,   // OL Feature o GeoJSON
        distance:   500        // metros
    }
})
.then(({ serviceBuffer }) => {
    // serviceBuffer = GeoJSON del polÃ­gono resultante

    // 2. Calcular intersecciÃ³n con una capa
    interactions.addIntersect({
        idMap:             "main",
        idIntersect:       "int1",
        intersectDataIn:   { type: "JSON",  data: JSON.stringify(serviceBuffer) },
        intersectDataClip: { type: "LAYER", data: "tuberias" },
        style: {
            type:   "ALL",
            stroke: { color: "#FF5858", width: 2 },
            fill:   { color: "rgba(255,151,193,0.2)" }
        }
    });
});

// Limpiar
interactions.removeBuffer({ idMap: "main", idBuffer: "buf1" });
interactions.removeIntersect({ idMap: "main", idIntersect: "int1" });
```

---

### 6e. Filtro CQL (servidor)

> **[ARCH Â§5 GOVectorLayer â€” `addFilter(cql, reload?)`]** Solo funciona sobre capas que extienden `GOStyledLayer` y tienen `canFilter() = true` (todas las vectoriales salvo `GO_WMS_LAYER`). El parÃ¡metro `reload` (boolean) controla si se recarga la fuente inmediatamente. Sintaxis CQL: identificadores de campo en **comillas dobles**, strings en **comillas simples**. Si `useBboxFilter: true`, el filtro CQL se combina con el filtro bbox automÃ¡ticamente.

```javascript
const layer = map.getStaging("main").getLayer("tuberias");

// Aplicar filtro CQL al servidor (recarga datos)
layer.addFilter('"codigo" = 101', false);               // false = no recargar inmediatamente
layer.addFilter('"diametro" > 200 AND "activo" = true', true); // true = recargar fuente

// Eliminar filtro
layer.removeFilter();

// Filtro en doble comilla para campos (estÃ¡ndar CQL)
layer.addFilter('"municipio" IN (\'Valencia\', \'Madrid\')', true);
```

---

### 6f. Filtro No-CQL (cliente)

> **[ARCH Â§11 Tipos clave â€” `GoFilter` interface + `GoFilterCondition` enum]** `addFilterNoCQL` aplica el filtro en memoria sobre las features ya cargadas, sin hacer una nueva peticiÃ³n al servidor. El tipo `GoFilter` es: `{ field: string, condition: GoFilterCondition, value: string|number|Array<string|number>, or?: GoFilter[], and?: GoFilter[] }`. Los 16 operadores disponibles son: `EQUAL_TO, NOT_EQUAL_TO, BETWEEN, NOT_BEETWEEN, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, IS_NULL, IS_NOT_NULL, IS_EMPTY, IS_NOT_EMPTY, IS_IN, IS_NOT_IN, IS_LIKE, IS_NOT_LIKE`.

```javascript
import { GoFilterCondition } from "@utils/index";

const layer = map.getStaging("main").getLayer("sensores");

// Filtro simple de igualdad
layer.addFilterNoCQL({
    field:     "estado",
    condition: GoFilterCondition.EQUAL_TO,
    value:     "activo",
});

// IS_IN con OR
layer.addFilterNoCQL({
    field:     "id_sector",
    condition: GoFilterCondition.IS_IN,
    value:     [1, 3, 5, 7],
    or: [
        { field: "tipo", condition: GoFilterCondition.EQUAL_TO, value: "especial" }
    ]
});

// Entre valores
layer.addFilterNoCQL({
    field:     "presion",
    condition: GoFilterCondition.BETWEEN,
    value:     [2.5, 5.0],
});

// Eliminar filtro cliente
layer.removeFilterNoCQL();
```

---

### 6g. Editar features (WFS-T)

> **[ARCH Â§6 Features â€” `GoEditFeatures`]** Firmas: `addEditFeature({ idEditFeature, idMap, idLayer }): void`. `saveEditFeatures({ idEditFeature, idMap, idLayer, user, pass }): void`. Usa transacciones WFS-T (OGC). Requiere que la capa sea `GOVectorLayer` (`GO_VECTOR_LAYER`) con permisos de escritura en GeoServer.

```javascript
const interactions = map.getInteractions();

// Activar ediciÃ³n
interactions.addEditFeature({
    idEditFeature: "editFeature",
    idMap:         "main",
    idLayer:       "tuberias",
});

// Guardar cambios via WFS-T
interactions.saveEditFeatures({
    idEditFeature: "editFeature",
    idMap:         "main",
    idLayer:       "tuberias",
    user:          "admin",
    pass:          "password",
});
```

---

### 6h. Eliminar features (WFS-T)

> **[ARCH Â§6 Firmas â€” `deleteFeatures`]** Firma: `deleteFeatures({ idMap, idLayer, feature: Feature[], user, pass }): void`. `feature` es un array de `OL.Feature` â€” el array debe contener las features tal como fueron devueltas por la fuente OL. Mismos requisitos de permisos que `saveEditFeatures`.

```javascript
interactions.deleteFeatures({
    idMap:   "main",
    idLayer: "pozos",
    feature: [selectedFeature],
    user:    "admin",
    pass:    "password",
});
```

---

### 6i. Consulta por capa (WFS)

> **[ARCH Â§6 Firmas â€” `getFeaturesByLayer`]** Firma completa: `getFeaturesByLayer({ idMap: string, layerId?: string, layerName?: string, cqlFilter?: string, maxFeatures?: number, propertyName?: string[] }): Promise<GeoJSON.FeatureCollection>`. Usar `layerId` (id de capa registrada en el mapa) **o** `layerName` (nombre workspace:capa del servidor), no ambos. `propertyName` permite traer un subconjunto de atributos (optimizaciÃ³n de payload).

```javascript
interactions.getFeaturesByLayer({
    idMap:        "main",
    layerId:       "tuberias",          // OR layerName: "workspace:nombre"
    cqlFilter:    '"codigo" = 101',     // opcional
    maxFeatures:   100,                 // opcional
    propertyName:  ["id", "nombre", "diametro"], // opcional
})
.then((featureCollection) => {
    // featureCollection = GeoJSON FeatureCollection
    featureCollection.features.forEach(f => {
        console.log(f.properties);
    });
});
```

---

### 6j. SelecciÃ³n por polÃ­gono

> **[ARCH Â§6 Selection â€” `GoSelectByPolygon`]** El mÃ³dulo `Selection` exporta `GoSelectByPolygon`, `GoDrawSelectByPolygon`, `GoDrawSelectByPolygonInCluster`. `addSelectByPolygon` activa el modo de dibujo de polÃ­gono; el `callback` recibe `Feature[]` al completar el polÃ­gono. `clearSelectByPolygon` borra la selecciÃ³n actual sin desactivar la herramienta. `removeSelectByPolygon` desactiva la herramienta completamente.

```javascript
interactions.addSelectByPolygon({
    idMap:     "main",
    idSelect:  "sel1",
    idLayer:   "tuberias",
    callback:  (features) => {
        console.log(`${features.length} features seleccionadas`);
        interactions.addHighlight({
            idMap:       "main",
            idHighlight: "seleccionadas",
            features:    features,
        });
    }
});

// Limpiar selecciÃ³n
interactions.clearSelectByPolygon({ idMap: "main", idSelect: "sel1" });

// Desactivar herramienta
interactions.removeSelectByPolygon({ idMap: "main", idSelect: "sel1" });
```

---

### 6k. MediciÃ³n

> **[ARCH Â§6 Analysis â€” `GoMeasure`]** El tipo `unitSystem { METRIC = "metric", IMPERIAL = "imperial" }` (ARCH Â§11 Enums) controla las unidades. `type: "LineString"` mide distancias; `type: "Polygon"` mide Ã¡reas. Alternativamente, los botones de mediciÃ³n del mapa (`showLinearMeasurementButton`, `showPolygonMeasurementButton` en `UIConfig`) activan la misma herramienta desde la UI.

```javascript
// Activar mediciÃ³n lineal
interactions.addMeasure({
    idMap:      "main",
    idMeasure:  "med1",
    type:       "LineString",  // "LineString" | "Polygon"
    units:      "metric",      // "metric" | "imperial"
});

// Limpiar mediciÃ³n actual
interactions.clearMeasure({ idMap: "main", idMeasure: "med1" });

// Desactivar herramienta
interactions.removeMeasure({ idMap: "main", idMeasure: "med1" });
```

---

## 7. Sistema de eventos

> **[ARCH Â§11 Enums clave â€” `GOMapEvents`, `GOLayerEvents`, `GOFeaturesEvents`]** El bus de eventos centralizado es `MapEventManager.events` (clase `MapEventManager`, ARCH Â§11 Utils). Usa siempre los **enums** (nunca strings literales). AtenciÃ³n: `GOMapEvents.LAYER_VISIBILITY_CHANGED = "layer-visibility-changed"` usa kebab-case mientras que los demÃ¡s usan UPPER_SNAKE_CASE. Los eventos del mapa OL nativo se suscriben via `map.onMap(id, typeOn.*, callback)` (enum `typeOn`, ARCH Â§11). Los eventos de la librerÃ­a propios se suscriben via `map.on(eventName, callback)` / `map.once` / `map.off`.

```javascript
import { MapEventManager, GOMapEvents, GOLayerEvents, GOFeaturesEvents } from "go-gis";

// Eventos del mapa
MapEventManager.events.on(GOMapEvents.MAP_LOADING, (data) => {
    console.log("Mapa cargando...", data);
});

MapEventManager.events.on(GOMapEvents.MAP_LOADED, (data) => {
    console.log("Mapa cargado:", data);
});

// Eventos de capa
MapEventManager.events.on(GOLayerEvents.LAYER_LOADING, ({ layerId }) => {
    console.log(`Capa ${layerId} cargando...`);
});

MapEventManager.events.on(GOLayerEvents.LAYER_LOADED, ({ layerId }) => {
    console.log(`Capa ${layerId} cargada`);
});

MapEventManager.events.on(GOLayerEvents.LAYER_ERROR, ({ layerId, error }) => {
    console.error(`Error en capa ${layerId}:`, error);
});

// Eventos de features
MapEventManager.events.on(GOFeaturesEvents.FEATURES_LOAD_STARTED, (data) => {
    console.log("Cargando features:", data);
});

MapEventManager.events.on(GOFeaturesEvents.FEATURES_LOAD_ENDED, (data) => {
    console.log("Features cargadas:", data);
});
```

---

## 8. Servicios

> **[ARCH Â§7 MÃ³dulo Services]** Los servicios son singletons accesibles directamente. `GoStore` es el Ãºnico estado global (no Redux): `GoStore.getState(): GoState` y `GoStore.dispatch(payload): GoState`. `GoAxiosServices` es el wrapper HTTP con cabeceras de auth automÃ¡ticas (usa el token del store). `GoLocalStorageService` es un wrapper tipado de localStorage. Los servicios de datos externos (`GoCadastre`, `GoDMD`, `GoGeoServices`) se usan internamente por `GoInteractions`.

---

### 8a. GeocodificaciÃ³n HERE

> **[ARCH Â§6 ExternalsConnections â€” `GoGeocodingHERE`]** El registro de credenciales (`registerGeocodingHERE`) requiere: `environmentID` (enum `RegistedEnviroments`), `appID` (enum `RegistedApps`), `clientID` (enum `RegistedClients`), todos ARCH Â§11. Se debe llamar **una sola vez** antes de cualquier operaciÃ³n HERE. Los mÃ©todos disponibles: `getCoordinatesFromDirectionHERE`, `getDirectionFromCoordinatesHERE`, `suggestAddress` (autocompletado). `GoGeocoding` es la variante Google Maps (usa `GO_CONFIG.GOOGLE_MAPS_API`, ARCH Â§3).

```javascript
import { RegistedApps, RegistedClients, RegistedEnviroments } from "go-gis";

const interactions = map.getInteractions();

// 1. Registrar credenciales (una sola vez)
interactions.registerGeocodingHERE({
    environmentID: RegistedEnviroments.PRODUCTION,
    appID:         RegistedApps.GIS_API_DOCS,
    clientID:      RegistedClients.IDRICA,
    // apiKey:     "opcional-si-se-tiene"
});

// 2. DirecciÃ³n â†’ coordenadas (geocodificaciÃ³n directa)
interactions.getCoordinatesFromDirectionHERE({
    direction: "Calle Mayor 1, Valencia"
})
.then(({ lat, lng }) => {
    map.flyToCenter("main", [lng, lat], 3000);
});

// 3. Coordenadas â†’ direcciÃ³n (geocodificaciÃ³n inversa)
interactions.getDirectionFromCoordinatesHERE({ lat: 39.469, lng: -0.377 })
.then((address) => {
    console.log("DirecciÃ³n:", address);
});

// 4. Autosugerencia de direcciones
interactions.suggestAddress({ params: { q: "Calle Mayor" } })
.then((suggestions) => {
    suggestions.forEach(s => console.log(s.label));
});
```

---

### 8b. Routing HERE

> **[ARCH Â§6 ExternalsConnections â€” `GoRouting`]** `addRoutingHERE` requiere `registerGeocodingHERE` previo. `transportMode` acepta: `"car"`, `"truck"`, `"pedestrian"`, `"bicycle"`. `points` es un array de `{lat, lng}` que define el origen y destino (y waypoints intermedios). `addMultiRoutingHERE` permite rutas con mÃºltiples alternativas. Todas las rutas se visualizan en el mapa automÃ¡ticamente.

```javascript
// Ruta simple
interactions.addRoutingHERE({
    idMap:         "main",
    routingID:     "ruta1",
    transportMode: "car",    // "car" | "truck" | "pedestrian" | "bicycle"
    points:        [
        { lat: 39.469, lng: -0.377 },  // origen
        { lat: 39.480, lng: -0.390 }   // destino
    ],
    lang:          "es",
})
.then((result) => {
    console.log("Ruta calculada:", result);
});

// Eliminar ruta
interactions.removeRoutingHERE({ idMap: "main", routingID: "ruta1" });

// Multi-ruta
interactions.addMultiRoutingHERE({
    idMap:     "main",
    routingID: "multiRuta",
    // ... parÃ¡metros adicionales
});
```

---

### 8c. GestiÃ³n de errores

> **[ARCH Â§11 Utils â€” `GoError`, `ErrorSeverity`]** La clase `GoError` gestiona errores tipados con niveles de severidad. `internalErrorHandler.set(severity, errorCode, errorMsg)` crea errores normalizados. Los niveles son: `ErrorSeverity.Error` (crÃ­tico), `ErrorSeverity.Warning`, `ErrorSeverity.Info`. Los errores de capa tambiÃ©n se emiten como `GOLayerEvents.LAYER_ERROR` en el bus de `MapEventManager`.

```javascript
import * as src from "go-gis";

// Crear un error interno
const error = src.internalErrorHandler.set(
    src.ErrorSeverity.Error,
    src.internalErrors.INTERNAL_ERROR_CANT_CREATE_CLUSTER_CLUSTER_CALLBACK_NOT_A_STRING,
    src.internalErrosMsg.INTERNAL_ERROR_CANT_CREATE_CLUSTER_CLUSTER_CALLBACK_NOT_A_STRING
);
console.error(error.errorMsg);

// Niveles de severidad
// src.ErrorSeverity.Error      â†’ Error crÃ­tico
// src.ErrorSeverity.Warning    â†’ Advertencia
// src.ErrorSeverity.Info       â†’ InformaciÃ³n
```

---

## 9. TOC dinÃ¡mico

> **[ARCH Â§8 MÃ³dulo TOC â€” `GoOwnToc`]** Acceso: `map.getStaging(mapId).getOwnToc()`. `toc_link` en `layerOptions` define un array de IDs de capas cuya visibilidad se sincroniza con la capa principal (usada para desdoblar la misma dataset en dos capas con diferentes resoluciones: la del cluster a baja resoluciÃ³n y la de puntos sueltos a alta resoluciÃ³n). `toc: false` en una capa la excluye del TOC sin eliminarla del mapa. El `id` del elemento HTML del TOC debe coincidir con `optionToc.id` en la config.

**Enlazar capas en el TOC** (toggle actÃºa sobre ambas):

```json
{
    "id":           "sensores_full",
    "alias":        "Sensores",
    "toc":          true,
    "toc_link":     ["sensores_cluster"],
    "minResolution": 0,
    "maxResolution": 1
},
{
    "id":           "sensores_cluster",
    "toc":          false,
    "minResolution": 1,
    "maxResolution": 999
}
```

**ManipulaciÃ³n programÃ¡tica:**
```javascript
const toc = map.getStaging("main").getOwnToc();

// Cambiar idioma del TOC
toc.setLanguage("EN");

// Volar al centro desde el TOC (el TOC tiene botÃ³n de fit nativo)
map.flyToCenter("main", [-0.377, 39.469], 2000);
```

---

## 10. Visor 3D / Mapbox

> **[ARCH Â§1 Punto de entrada â€” `@mapbox/index`]** `MapboxViewer` vive en el mÃ³dulo `@mapbox/index` exportado desde `src/index.ts`. El visor 3D usa Mapbox GL JS directamente. Las interacciones disponibles son: `hover`, `highlight` (con filtros Mapbox GL), `tooltip`, `closeTool` (hidraulica sobre capas Mapbox). `buildings: true` activa la capa de edificios 3D de Mapbox. `geoserverBaseUrl` debe apuntar a la misma URL que `enviroment` del `GoMapLoader`.

```javascript
import { MapboxViewer } from "go-gis";

const viewer = new MapboxViewer(mapConfig, gisMaps, {
    mapbox: {
        token: "pk.eyJ1...",
    },
    geoserverBaseUrl: "https://gateway-proxy-gis-devtools.go-aigua.com",
    buildings:        true,
    interactions: {
        hover:     {},
        highlight: {
            filters: [
                {
                    id:       "initialHighlight",
                    layerId:  "WFS_swn_dma",
                    filter:   ["==", ["get", "id"], 18]
                }
            ]
        },
        tooltip: {},
        closeTool: {
            autoEnable:        true,
            pipeLayerIds:       ["niza_tuberias", "niza_acometidas"],
            valveLayerIds:      ["niza_valvulas"],
            filterLocationName: "munid",
            pipeStyle:          { color: "#FFEB3B", width: 3 },
            valveStyle:         { color: "#0000FF" },
        },
    },
});
```

---

## 11. Visor BIM / IFC

> **[ARCH Â§1 Punto de entrada â€” `@ifc/index`]** El servicio de modelos IFC es `GoIFCServices` (ARCH Â§7 Services). Los eventos del visor se reciben via `GoIFCComponentsEvents`. `automaticLinking.dmdPropName` define la propiedad IFC que se enlaza con los activos DMD. `customClassifications` agrega clasificaciones personalizadas al panel lateral. El campo `isExample: true` activa el modo demo (datos de muestra).

```javascript
import { GoIcons, GoIFCComponentsEvents } from "go-gis";

const ifcConfig = {
    mapId:        "main",
    ifcId:        "Barna",
    ifcContainer: ".ifc-model",
    modelKey:     "ex_bim_ifc_small-v6",
    appId:        "go-aigua-wo",
    models: [
        { id: "c1115682-...", fileName: "Modelo HidrÃ¡ulica" },
        { id: "9febc692-...", fileName: "Modelo Obra Civil" },
    ],
    automaticLinking: { dmdPropName: "GO_TAG" },
    isExample:    true,
    customClassifications: [
        { name: "AeasFluDes", field: "AeasFluDes", groupByEquals: true },
        { name: "AeasTAG",    field: "AeasTAG",    groupByEquals: true },
    ],
};
```

---

## 12. Descarga de capas

> **[ARCH Â§5 GOLayer â€” `getFeatures()`]** El mÃ©todo `layer.createGeoJson()` serializa las features OL actualmente cargadas en la fuente a un string GeoJSON. Solo estÃ¡ disponible en capas que extienden `GOStyledLayer`. Para capas WMS (que no cargan features en cliente), la descarga debe hacerse via request WFS directamente al servidor con `outputFormat=application/json` o `application/zip` (Shapefile).

```javascript
// Obtener la capa
const layer = map.getStaging("main").getLayer("tuberias");

// Serializar las features cargadas a string GeoJSON
const geojsonStr = layer.createGeoJson();

// Crear un enlace de descarga
const blob = new Blob([geojsonStr], { type: "application/json" });
const url  = URL.createObjectURL(blob);
const a    = document.createElement("a");
a.href     = url;
a.download = "tuberias.geojson";
a.click();
```

**Formatos soportados** (como request WFS `outputFormat`):
- GeoJSON
- Shapefile (ZIP)
- GML
- CSV

---

## 13. Ejemplo mÃ­nimo funcional completo

> **[ARCH Â§13 PatrÃ³n de uso tÃ­pico]** Este patrÃ³n reproduce â€” en formato de docs con `loadScripts` â€” el flujo completo descrito en ARCH Â§13. Los campos mÃ­nimos obligatorios de `GOMapOptions` son: `id`, `div`, `lang`, `ui`, `optionView`, `translations`. El array `layer` es tÃ©cnicamente opcional pero sin Ã©l el mapa aparece vacÃ­o. `optionToc` requiere `active`, `legend` y `maxHeight` como campos obligatorios de `optionsToc`.

Este es el patrÃ³n mÃ¡s pequeÃ±o posible para tener un mapa funcional con una capa vectorial.

**`config.json`:**
```json
{
  "GOMapOptions": [{
    "id":   "main",
    "div":  "mapa",
    "lang": "es",
    "optionView": {
      "id":         "vista1",
      "center":     [-79850, 4789551],
      "zoom":       14,
      "projection": "3857"
    },
    "optionToc": {
      "active": true,
      "legend": true
    },
    "ui": {
      "showFullScreenButton": true
    },
    "layer": [
      {
        "id":          "base",
        "layerType":   "GO_MAPBOX",
        "isBaseLayer": true,
        "urlFolder":   "Base/",
        "style":       "cljzp2t2r008b01qr8izjchch",
        "token":       "pk.eyJ1..."
      },
      {
        "id":               "valvulas",
        "alias":            "VÃ¡lvulas",
        "layerType":        "GO_VECTOR_LAYER",
        "url":              "https://my-geoserver.com/geoserver",
        "layerName":        "water:valvulas",
        "layerProjection":  "3857",
        "useBboxFilter":    true,
        "defaultVisibility": true,
        "urlFolder":        "Red/",
        "zIndex":           3,
        "legend":           true,
        "styleSVG": {
          "iconSvg":         "008_01_valve",
          "shape":           "marker",
          "backgroundColor": "#ffffff",
          "mode":            "inactive",
          "scale":           1,
          "anchor":          [0.5, 0.5]
        }
      }
    ],
    "translations": { "es": {}, "en": {} }
  }]
}
```

**`index.html`:**
```html
<!DOCTYPE html>
<html data-theme="gistheme">
<head>
    <meta charset="utf-8" />
    <title>Mi Mapa</title>
    <link rel="stylesheet" href="../../static/styles/docs.css" />
</head>
<body>
    <div class="go-navbar"></div>
    <div id="mapa" class="container-map-example">
        <div id="go-map-loader"></div>
    </div>
    <script type="module" src="./static/js/scripts.js"></script>
</body>
</html>
```

**`static/js/scripts.js`:**
```javascript
import { loadScripts } from "@baseScripts";
import config from "../../config.json";

window.onload = () => {
    loadScripts({ config }).then((map) => {
        // Clic para ver atributos
        const olMap = map.getMap("main");
        olMap.on("singleclick", (evt) => {
            const features = olMap.getFeaturesAtPixel(evt.pixel);
            if (features.length > 0) {
                const props = features[0].getProperties();
                delete props.geometry;
                console.log("Feature:", props);
            }
        });
    });
};
```

---

## 14. CatÃ¡logo de todos los ejemplos

| CategorÃ­a | Ejemplos |
|-----------|---------|
| **Tipos de capa** | `ex_add_layer`, `ex_add_new_layer_isolated`, `ex_basemap`, `ex_external_wfs`, `ex_external_wms`, `ex_wmts_layer`, `ex_wmts_layer_projections`, `ex_xyz_layer`, `ex_vector_tile_json`, `ex_vector_tile_pbf` |
| **GeoJSON** | `ex_geojson_load`, `ex_geojson_styling`, `ex_geojson_annotations`, `ex_geojson_annotations_conditions`, `ex_geojson_from_url`, `ex_geojson_data_projection`, `ex_geojson_extannotation`, `ex_isolated_styles_geojson` |
| **ClÃºster** | `ex_cluster`, `ex_cluster_json`, `ex_cluster_json_clear`, `ex_cluster_style`, `ex_cluster_styles_custom`, `ex_cluster_annotation_conditions`, `ex_cluster_bboxFilter`, `ex_cluster_resolution`, `ex_alarm_cluster` |
| **Filtros** | `ex_filter`, `ex_filter_cql`, `ex_filter_cql_default`, `ex_filter_cql_styles`, `ex_filter_cql_cluster_cancelable`, `ex_filter_cql_vector_cancelable`, `ex_filter_nocql_cluster`, `ex_filter_with_annotations`, `ex_webgl_filters`, `ex_webgl_cql_filter` |
| **Interacciones** | `ex_click_on_feature`, `ex_selection`, `ex_select_by_polygon`, `ex_default_hover`, `ex_popup`, `ex_overlay_features`, `ex_listeners`, `ex_get_all_properties`, `ex_get_all_properties_cluster` |
| **EdiciÃ³n de features** | `ex_edit_features`, `ex_delete_features` |
| **Buffer / GeometrÃ­a** | `ex_buffer`, `ex_intersect`, `ex_measure_polygon`, `ex_polygon_center` |
| **Estilos** | `ex_styles`, `ex_default_setstyles`, `ex_manage_styles`, `ex_manage_feature_styles`, `ex_manage_attr_styles`, `ex_manage_attr_svg_styles`, `ex_manage_layer_styles`, `ex_set_style_function`, `ex_animated_style`, `ex_feature_highlight_styles`, `ex_feature_highlight_change_styles_on_fly`, `ex_feature_hover_styles`, `ex_default_annotation_array`, `ex_default_annotation_condition`, `ex_default_annotation_pols_lines`, `ex_default_annotation_vectorlayer`, `ex_default_hover` |
| **Iconos** | `ex_icons_overlay`, `ex_icons_rotation`, `ex_icons_scale`, `ex_icon_url`, `ex_transparent_icons` |
| **Anotaciones / Etiquetas** | `ex_default_annotation_array`, `ex_vectorlayer_annotations`, `ex_geojson_annotations`, `ex_manage_cluster_annotation` |
| **Flota / Camiones** | `ex_fleet_monitoring`, `ex_fleet_monitoring_reproject` |
| **GeocodificaciÃ³n** | `ex_geocoding_here`, `ex_geocoding_here_autocomplete`, `ex_geocoding_here_autocomplete_city`, `ex_geocoding_here_autocomplete_properties`, `ex_geocoding_here_autosuggest`, `ex_geocoding_here_properties` |
| **Routing** | `ex_routing`, `ex_multi_routing`, `ex_multi_routing_lang` |
| **AnÃ¡lisis de red** | `ex_upstream`, `ex_downstream`, `ex_downstream_munic`, `ex_close_tool`, `ex_close_tool_munic`, `ex_sector_lines`, `ex_optical_fiber`, `ex_watershed` |
| **Mapas temÃ¡ticos** | `ex_thematic_map`, `ex_thematic_ai`, `ex_thematic_ai_playground`, `ex_heat_map`, `ex_hexgrid_layer`, `ex_density_tool` |
| **3D / BIM** | `ex_3d_mapbox_basic_map`, `ex_bim_ifc_small`, `ex_bim_ifc_medium`, `ex_bim_ifc_big`, `ex_bim_ifc_tempus`, `ex_bim_ifc_testperte`, `ex_map_perspective` |
| **TOC** | `ex_dynamic_toc`, `ex_manage_toc`, `ex_multiple_toc`, `ex_subfolders_in_toc` |
| **IoT / Tiempo real** | `ex_iot_layer`, `ex_fleet_monitoring`, `ex_isolines_iot`, `ex_ground_movement` |
| **Activos DMD** | `ex_dmd_assets_create`, `ex_dmd_assets_edit`, `ex_dmd_assets_delete`, `ex_dmd_assets_list`, `ex_dmd_template_load`, `ex_dmd_assets_callback` |
| **ImpresiÃ³n / ExportaciÃ³n** | `ex_print_pdf`, `ex_download_layers`, `ex_map_img` |
| **Controles de mapa** | `ex_basemap`, `ex_full_screen`, `ex_map_rotation`, `ex_map_zoom_center_fly`, `ex_custom_zoom_buttons`, `ex_custom_zoom_controls`, `ex_set_opacity`, `ex_disable_dbclick_zoom` |
| **Auth / Config / Servicios** | `ex_goErrorsHandlers`, `ex_goConfigValidator`, `ex_axiosServices`, `ex_localStorageService`, `ex_csp_compliance` |
| **WebGL** | `ex_webgl_geojson_json`, `ex_webgl_geojson_svg`, `ex_webgl_lines_simple_style`, `ex_webgl_filters`, `ex_webgl_performance`, `ex_webgl_select_by_polygon`, `ex_webgl_cql_filter` |
| **Vistas enlazadas** | `ex_bind_views` |
| **Custom features / params** | `ex_custom_geo_params`, `ex_custom_zoom_buttons` |
| **Catastro** | `ex_cadastres` |
| **Remote sensing** | `ex_remote_sensing` |
| **Consultas de features** | `ex_get_features_by_layer`, `ex_get_features_by_attribute` |

---

## 15. GuÃ­a rÃ¡pida para agentes IA

> Esta secciÃ³n estÃ¡ diseÃ±ada exclusivamente para que un agente IA pueda generar ejemplos go-gis correctos, completos y coherentes con la arquitectura. Siempre consultar `src/GO-GIS-ARCHITECTURE.md` como fuente de verdad de tipos, firmas y enums.

### 15a. Checklist obligatorio antes de generar un ejemplo

```
[ ] 1. Â¿El ejemplo usa `loadScripts({ config, exampleCode })`?
        â†’ config debe ser un objeto con la clave raÃ­z "GOMapOptions" (type configOptions)
        â†’ Toda lÃ³gica post-mapa DEBE ir dentro del .then(map => { ... })

[ ] 2. Â¿Cada GOMapOptions tiene los campos obligatorios?
        â†’ id (string Ãºnico), div (id HTML), lang (SupportedLangs),
          ui (UIConfig), optionView (optionsView), translations

[ ] 3. Â¿optionView tiene id, center, zoom?
        â†’ projection es EPSG SIN prefijo ("3857", no "EPSG:3857")

[ ] 4. Â¿Cada capa tiene id, alias (salvo capas base), layerType?
        â†’ layerType en JSON = string (ej: "GO_VECTOR_LAYER")
        â†’ layerType en TypeScript = GOLayerType.GO_VECTOR_LAYER

[ ] 5. Â¿urlFolder de la capa coincide con folderName en optionToc.foldersOptions?
        â†’ Incluir / al final: "Red/" â‰  "Red"

[ ] 6. Â¿Los enums de eventos/temas/idiomas usan los valores correctos?
        â†’ themes.DARK = "DARK" (no "dark")
        â†’ SupportedLangs = "es" (no "ES")
        â†’ typeOn.singleclick = "singleclick"

[ ] 7. Â¿Las interacciones tienen idMap e idInteraction Ãºnicos?
        â†’ map.getInteractions() devuelve GoInteractions compartido

[ ] 8. Â¿Se hace cleanup? (removeHighlight, removeHover, removeBuffer...)
        â†’ Siempre limpiar herramientas al terminar para evitar memory leaks
```

### 15b. Receta para generar cualquier tipo de ejemplo

**Ejemplo de capa vectorial con filtro y hover:**
```javascript
// PASO 1 â€” Cargar el mapa (siempre async)
loadScripts({ config }).then((map) => {

    // PASO 2 â€” Obtener el mapa OL y la instancia de interacciones
    const olMap        = map.getMap("main");                    // OlMap nativo
    const interactions = map.getInteractions();                  // GoInteractions
    const layer        = map.getStaging("main").getLayer("mi_capa"); // GOVectorLayer

    // PASO 3 â€” Activar hover sobre la capa
    // [ARCH Â§6 Features â€” GoHover]
    interactions.mouseHover({
        idMap:   "main",
        idHover: "hover_capa",
        layer:   "mi_capa",
        callback: (feature) => console.log(feature.getProperties()),
        style: { type: "ALL", stroke: { color: "#FFCC00", width: 3 } }
    });

    // PASO 4 â€” Filtro CQL en botÃ³n
    // [ARCH Â§5 GOVectorLayer â€” addFilter]
    document.getElementById("btn-filtrar").addEventListener("click", () => {
        layer.addFilter('"estado" = \'activo\'', true);
    });

    document.getElementById("btn-limpiar").addEventListener("click", () => {
        layer.removeFilter();
    });

    // PASO 5 â€” Escuchar eventos de capa
    // [ARCH Â§11 GOLayerEvents â€” MapEventManager]
    MapEventManager.events.on(GOLayerEvents.LAYER_LOADED, ({ layerId }) => {
        if (layerId === "mi_capa") console.log("Capa cargada");
    });
});
```

### 15c. Campos obligatorios por tipo de capa (resumen)

| `layerType` | Campos obligatorios adicionales |
|-------------|--------------------------------|
| `GO_VECTOR_LAYER` | `url`, `layerName`, `layerProjection` |
| `GO_WMS_LAYER` | `url`, `layerName` |
| `GO_WFS_LAYER` | `url`, `layerName`, `layerProjection` |
| `GO_GEOJSON_LAYER` | `url` **o** `data` (no ambos) |
| `GO_CLUSTER_LAYER` | `url`, `layerName`, `layerProjection`, `distance` |
| `GO_HEATMAP_LAYER` | `url`, `layerName`, `layerProjection`, `radiusHeatMap` |
| `GO_HEXGRID_LAYER` | `url`, `layerName`, `hexSize` |
| `GO_IOT_LAYER` | `url`, `layerName`, `reloadFrequency` |
| `GO_TRUCK_LAYER` | `url`, `layerName` |
| `GO_WEBGL_LAYER` | `url`, `layerName` |
| `GO_MAPBOX` | `style` (Mapbox style ID), `token` |
| `GO_XYZ` | `url` (con `{x}`, `{y}`, `{z}` o `{-y}`) |
| `GO_ESRI_BASEMAP` | `idEsri`, `esriApiKey` |
| `GO_VECTOR_TILE` | `url` (URL del servicio de tiles `.pbf`) |
| `GO_REMOTE_SENSING` | `url`, `remoteSensingType` |

### 15d. Errores comunes a evitar

| Error | Causa | CorrecciÃ³n |
|-------|-------|-----------|
| `map.getStaging(id)` devuelve `undefined` | El `id` de la config no coincide con el string pasado | Verificar `GOMapOptions[].id` |
| La capa no aparece / errores de CORS | `url` no incluye protocolo o es relativa | Usar URL absoluta (`https://`) |
| Estilo no se aplica | `styleSVG` en capa WMS | WMS no soporta `styleSVG`; usar `sldStyle` |
| Filtro CQL no funciona | Capa es `GO_WMS_LAYER` | `canFilter() = false` en WMS; usar `sldStyle` con CQL embebido |
| TOC no muestra la capa en la carpeta correcta | `urlFolder` no coincide exactamente con `folderName` | Deben ser idÃ©nticos incluyendo `/` final |
| `idInteraction` conflicto | Dos herramientas con el mismo `idInteraction` | Todos los IDs de interacciÃ³n deben ser Ãºnicos por `(idMap, idInteraction)` |
| Eventos no disparan | Usar string literal en lugar de enum | Usar `GOLayerEvents.LAYER_LOADED`, nunca `"LAYER_LOADED"` |
| `flyToCenter` no anima | ProyecciÃ³n del mapa es `"4326"` pero se pasa `[lng, lat]` invertido | En `"4326"`, `center = [longitude, latitude]`; en `"3857"`, usar coordenadas mÃ©tricas |

### 15e. Importaciones de rÃ©fÃ©rencia

```javascript
// En ejemplos (entorno docs â€” via alias de Vite)
import { loadScripts } from "@baseScripts";
import config from "../../config.json";
import {
    GoMapLoader, GoMap, GOLayerType,
    GoFilterCondition, themes, UpDownType,
    typeOn, MapEventManager,
    GOMapEvents, GOLayerEvents, GOFeaturesEvents,
    RegistedEnviroments, RegistedApps, RegistedClients,
    IconShape, IconMode,
    GoStore
} from "go-gis";
// â†’ Todos estos sÃ­mbolos estÃ¡n documentados en ARCH Â§11 Enums y Tipos
```

### 15f. SecciÃ³n de arquitectura â†’ secciÃ³n de ejemplos (Ã­ndice cruzado inverso)

| Si lees en ARCH... | El patrÃ³n de ejemplo estÃ¡ en EXAMPLES... |
|--------------------|-----------------------------------------|
| Â§4 GoMapLoader / MapConfigLoader | Â§3a loadScripts, Â§13 Ejemplo mÃ­nimo |
| Â§4 GoMap mÃ©todos vista | Â§3c optionView, Â§6a clic, Â§9 TOC |
| Â§4 GoMap mÃ©todos capa | Â§4 Patrones de capas (todas) |
| Â§4 GOStagingMap | Â§4bâ€“4m (getLayer), Â§6bâ€“6k (interacciones) |
| Â§5 GOLayerType enum | Â§4 (todos los tipos de capa) |
| Â§5 layerOptions | Â§4 campos comunes + cada sub-secciÃ³n 4aâ€“4m |
| Â§5 GOVectorLayer mÃ©todos | Â§6e Filtro CQL, Â§6g Editar WFS-T |
| Â§6 Features | Â§6b Highlight, Â§6c Hover, Â§6g Editar |
| Â§6 geoTools | Â§6d Buffer + IntersecciÃ³n |
| Â§6 Selection | Â§6j SelecciÃ³n por polÃ­gono |
| Â§6 Analysis | Â§6k MediciÃ³n |
| Â§6 ExternalsConnections | Â§8a GeocodificaciÃ³n, Â§8b Routing |
| Â§6 Hydraulics | Â§14 ex_upstream, ex_downstream, ex_close_tool |
| Â§7 GoStore / GoState | Â§3a loadScripts (isExample, useBackendFeatures) |
| Â§8 TOC | Â§3d optionToc, Â§9 TOC dinÃ¡mico |
| Â§9 UIConfig | Â§3e ui botones |
| Â§10 i18nService | Â§3f translations |
| Â§11 ISVGIcon | Â§5a styleSVG, Â§5b condicional |
| Â§11 styleFromUser | Â§5c styleGeojson, Â§6b Highlight, Â§6c Hover |
| Â§11 GoFilter / GoFilterCondition | Â§6f Filtro No-CQL |
| Â§11 GOMapEvents/GOLayerEvents | Â§7 Sistema de eventos |
| Â§11 UpDownType | Â§14 ex_upstream, ex_downstream |
| Â§11 themes | Â§2 Plantilla HTML, Â§13 Ejemplo mÃ­nimo |

---

## 16. Mapa maestro de conocimiento â€” Tri-documento

> Esta secciÃ³n provee el Ã­ndice de navegaciÃ³n definitivo para agentes IA. Para **cualquier concepto** de la librerÃ­a, esta tabla indica exactamente dÃ³nde encontrarlo en los tres documentos: este archivo (`EXAMPLES.md`), `GO-GIS-ARCHITECTURE.md` y `go-gis-VERBATIM.md`.

### 16a. Concepto â†’ ARCH + EXAMPLES + ALL_FILES

| Concepto / Clase / Tipo | ARCH (tipos+firmas) | EXAMPLES (patrÃ³n uso) | ALL_FILES (fuente real) |
|-------------------------|---------------------|-----------------------|------------------------|
| `GoMapLoader.loadGoMap` | Â§4 GoMapLoader | Â§3a, Â§13 | **[FILES L2609â€“L3331]** |
| `GoMap` (fachada principal) | Â§4 GoMap | Â§3â€“Â§9 | **[FILES L1374â€“L2608]** |
| `GOStagingMap` | Â§4 GOStagingMap | Â§4 getLayer, Â§6bâ€“6k | **[FILES L3332â€“L4129]** |
| `GOLayer` (base abstracta) | Â§5 GOLayer | Â§4 todas las capas | **[FILES L4130â€“L5131]** |
| `GOVectorLayer` | Â§5 GOVectorLayer | Â§4b, Â§6e, Â§6gâ€“6h | **[FILES L5132â€“L6096]** |
| `GOClusterLayer` | Â§5 GOClusterLayer | Â§4f | **[FILES L6097â€“L7310]** |
| `GOGeoJsonLayer` | Â§5 GOGeoJsonLayer | Â§4e, Â§12 | **[FILES L7311â€“L7915]** |
| `GOWMSLayer` | Â§5 GOWMSLayer | Â§4c | **[FILES L7916â€“L8214]** |
| `GOWFSLayer` | Â§5 GOWFSLayer | Â§4d | **[FILES L8215â€“L9012]** |
| `UIConfig` | Â§9 UIMap | Â§3e | **[FILES L9013â€“L9034]** |
| `GoHighlight` | Â§6 Features GoHighlight | Â§6b | **[FILES L9035â€“L9064]** |
| `GoHover` | Â§6 Features GoHover | Â§6c | **[FILES L9035â€“L9064]** |
| `getFeaturesByLayer` | Â§6 Firmas | Â§6i | **[FILES L9065â€“L9068]** + **[FILES L15459â€“L15462]** |
| `GoStore` / `GoState` | Â§7 Services | Â§3a, Â§8c | **[FILES L9069â€“L9103]** |
| `GOLayerType` enum | Â§5 Enum GOLayerType | Â§4 todos los tipos | **[FILES L9104â€“L9685]** |
| `layerOptions` (tipo completo) | Â§5 layerOptions | Â§4 campos comunes | **[FILES L9104â€“L9685]** |
| `GoFilterCondition` enum | Â§11 Enums clave | Â§6f | **[FILES L9104â€“L9685]** |
| `GOMapEvents` / `GOLayerEvents` | Â§11 Enums clave | Â§7 eventos | **[FILES L9104â€“L9685]** |
| `themes` / `SupportedLangs` / `typeOn` | Â§11 Enums clave | Â§3f, Â§15d | **[FILES L9104â€“L9685]** |
| `ISVGIcon` / `styleFromUser` | Â§11 Tipos clave | Â§5aâ€“5c | **[FILES L1â€“L1373]** |
| `configOptions` / JSON types | Â§11 Tipos clave | Â§3b, Â§4 | **[FILES L1â€“L1373]** |
| Paleta colores terreno | Â§5 capas raster | Â§4m | **[FILES L9686â€“L9696]** |
| `GoBuffer` / `GoIntersect` | Â§6 geoTools | Â§6d | **[FILES L9697â€“L9968]** |
| `GoInteractions` (todos los mÃ©todos) | Â§6 Firmas de mÃ©todos clave | Â§6 todos | **[FILES L9969â€“L13490]** |
| `GoMeasure` / `GoDensityTool` | Â§6 Analysis | Â§6k | **[FILES L9969â€“L13490]** |
| `GoSelectByPolygon` | Â§6 Selection | Â§6j | **[FILES L9969â€“L13490]** |
| `GoGeocodingHERE` / `GoRouting` | Â§6 ExternalsConnections | Â§8a, Â§8b | **[FILES L9969â€“L13490]** |
| `GoCloseTools` / `GoUpDown` | Â§6 Hydraulics | Â§14 ex_close_tool, ex_upstream | **[FILES L9969â€“L13490]** |
| `GoOverlay` / `GoPrint` | Â§6 Views | Â§14 ex_popup, ex_print_pdf | **[FILES L9969â€“L13490]** |
| `GoEditFeatures` (WFS-T) | Â§6 Features | Â§6g, Â§6h | **[FILES L9969â€“L13490]** |
| `GoOwnToc` | Â§8 TOC | Â§3d, Â§9 | **[FILES L13491â€“L15458]** |

### 16b. Protocolo de bÃºsqueda para agentes IA

**"Â¿CÃ³mo aÃ±ado la capa X?"** â†’ `EXAMPLES Â§4` (snippets) â†’ `ARCH Â§5` (tipos) â†’ `ALL_FILES` (implementaciÃ³n)

**"Â¿CÃ³mo funciona la interacciÃ³n X?"** â†’ `ARCH Â§6 Firmas` â†’ `EXAMPLES Â§6` (snippet) â†’ `ALL_FILES L9969â€“L13490`

**"Â¿CuÃ¡l es el tipo TS de X?"** â†’ `ARCH Â§11` â†’ `ALL_FILES L1â€“L1373` (types) o `L9104â€“L9685` (constants)

**"Hay un error"** â†’ `EXAMPLES Â§15d` â†’ `ARCH Â§5 layerOptions` â†’ `ALL_FILES` (lÃ­neas de la clase)

**"Â¿CÃ³mo funciona el TOC?"** â†’ `EXAMPLES Â§3d + Â§9` â†’ `ARCH Â§8` â†’ `ALL_FILES L13491â€“L15458`

### 16c. Grafo de dependencias entre los tres documentos

```
go-gis-VERBATIM.md                  GO-GIS-ARCHITECTURE.md         EXAMPLES.md
â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
L2609â€“L3331  GoMapLoader.ts  â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§4 GoMapLoader firma       â†â”€â”€ Â§3a loadScripts
L1374â€“L2608  GoMap.ts        â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§4 GoMap mÃ©todos           â†â”€â”€ Â§3bâ€“Â§9 uso
L3332â€“L4129  GoStagingMap.ts â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§4 GOStagingMap            â†â”€â”€ Â§6bâ€“6k
L4130â€“L5131  GoLayer/        â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOLayer abstracta       â†â”€â”€ Â§4 todas capas
L5132â€“L6096  GoVectorLayer/  â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOVectorLayer           â†â”€â”€ Â§4b, Â§6e, Â§6g
L6097â€“L7310  GoClusterLayer/ â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOClusterLayer          â†â”€â”€ Â§4f
L7311â€“L7915  GoGeoJsonLayer/ â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOGeoJsonLayer          â†â”€â”€ Â§4e, Â§12
L7916â€“L8214  GOWMS/          â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOWMSLayer              â†â”€â”€ Â§4c
L8215â€“L9012  GoWFS/          â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOWFSLayer              â†â”€â”€ Â§4d
L9013â€“L9034  UIMap/types.ts  â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§9 UIConfig                â†â”€â”€ Â§3e ui
L9035â€“L9064  Features/       â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§6 GoHighlight / GoHover   â†â”€â”€ Â§6b, Â§6c
L9069â€“L9103  GoStore/        â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§7 GoStore / GoState       â†â”€â”€ Â§3a
L9104â€“L9685  constants/      â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 GOLayerType/layerOptions â†â”€â”€ Â§4 layerType
L9686â€“L9696  terrainColors   â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§5 raster                  â†â”€â”€ Â§4m
L9697â€“L9968  geoTools/buffer â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§6 geoTools                â†â”€â”€ Â§6d
L9969â€“L13490 Interactions/   â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§6 Firmas de mÃ©todos       â†â”€â”€ Â§6 todos
L13491â€“L15458 GoOwnToc.ts    â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§8 TOC                     â†â”€â”€ Â§3d, Â§9
L1â€“L1373     types.ts        â†â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€ Â§11 Tipos clave            â†â”€â”€ Â§2â€“Â§5 config
L15459â€“L15462 GetFeaturesByLayer â†â”€â”€â”€â”€â”€â”€â”€ Â§6 Firmas                 â†â”€â”€ Â§6i
```
