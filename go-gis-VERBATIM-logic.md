FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Utils\types\types.ts
// Externals imports
/** @module types */
import OlMap from "ol/Map";
import View from "ol/View";
import * as olCoordinate from "ol/coordinate";
import { Extent } from "ol/extent";
import GeoJSON from "ol/format/GeoJSON";
import { Select } from "ol/interaction";
import VectorLayer from "ol/layer/Vector";
import { Size } from "ol/size";
import { Stroke, Style } from "ol/style";
import * as constants from "../constants";

// Internals imports
import VectorSource from "ol/source/Vector";
import { UIConfig } from "../../UIMap/types";
import { TextPlacement } from "ol/style/Text";
import { Feature } from "ol";
// TODO: Add the correct types
// eslint-disable-next-line @typescript-eslint/no-explicit-any
export type GoStyleData = any;

export interface GoStyleLogEntry {
	_id: number;
	_timestamp: number;
	goStyle: GoGlobalStyle | Array<GoGlobalStyle>;
	olStyle: Style | Array<Style>;
}

export interface GoFilter {
	field: string;
	condition: constants.GoFilterCondition;
	value: string | number | Array<string | number>;
	or?: Array<GoFilter>;
	and?: Array<GoFilter>;
}

export interface GoGlobalStyle {
	layerStyleType?: constants.GoLayerStyleType;
	styleType?: constants.GoStyleVariant;
	styleData?: GoStyleData;
	filter?: GoFilter;
	layerOpts?: layerOptions | LayerOptsMapbox;
	inheritsFrom?: number;
}

export interface HEREParams {
	environmentID: constants.RegistedEnviroments;
	appID: constants.RegistedApps;
	clientID: constants.RegistedClients;
	apiKey?: string;
}

export interface HEREParamsService extends HEREParams {
	isLocal: boolean;
}

export interface HERESuggestParams {
	textToSearch: string;
	city?: string;
	province?: string;
	country?: string;
	mapCenter?: string;
}

export interface LayerOptsMapbox {
	id: string;
	alias: string;
	layerType: string;
	isBaseLayer: boolean;
	urlFolder: string;
	style: string;
	token: string;
	layerProjection?: string;
	zIndex?: number;
	url?: string;
	visible?: boolean;
}

/**
 * @typedef optionsView
 * Tipos para opciones de las Vistas GOView.
 * @property {string} id
 * @property {Array<string>} maps
 * @property {Coordinate} center
 * @property {number} zoom
 * @property {Extent} extent
 * @property {number} rotation
 * @property {number} maxZoom
 * @property {number} minZoom
 * @property {boolean} multiWorld
 */
export type optionsView = {
	id: string;
	maps?: Array<string>;
	center: olCoordinate.Coordinate;
	zoom: number;
	projection?: string;
	extent?: Extent;
	rotation?: number;
	maxZoom?: number;
	minZoom?: number;
	multiWorld?: boolean;
};

/**
 * @typedef optionsToc
 * Tipos para opciones del Toc GoOwnToc.
 * @property {string} id Id del elemento GoOwnToc
 * @property {string} panelId Id del panel que renderiza el Toc
 * @property {string} mapId Id del map desde el config
 * @property {constants.languages} lang Idioma del TOC
 * @property {constants.themes} theme Thema del TOC
 * @property {boolean} active Estado del elemento GoOwnToc
 * @property {maxHeight} maxHeight Tamaño máximo que va a tomar la caja del TOC/Legend
 * @property {boolean} legend Indicador sobre si el GoOwnToc pintarña leyenda
 * @property {boolean} allFoldersExpanded Indica si las capas estarán expandidas por defecto.
 * @property {boolean} legendOpenByDefault Indica si la pestaña de leyenda estará abierta por defecto.
 * @property {boolean} tocAlwaysOpen Indica si el contenedor del TOC siempre debe permanecer abierto (sin botón de cierre).
 */
export type optionsToc = {
	id: string;
	panelId?: string;
	mapId?: string;
	theme?: constants.themes;
	active: boolean;
	maxHeight: number;
	legend: boolean;
	foldersOptions?: Array<folderOptions>;
	searchLayersFilter?: boolean;
	allFoldersExpanded?: boolean;
	legendOpenByDefault?: boolean;
	tocAlwaysOpen?: boolean;
};

export type folderOptions = {
	folderName: string;
	folderExpandedByDefault?: boolean;
	hasSwitcher?: boolean;
	onlyOneVisibleLayer?: boolean;
	folderAlwaysExpanded?: boolean;
};

export type LayerDataSource = {
	url: string;
	layerName: string;
	mun_filter?: string;
	mun_id?: string | number;
};

export type CloseToolResult = {
	affectedPipes: Array<{
		id: number;
		layerId: string;
		properties: Record<string, unknown>;
	}>;
	affectedValves: Array<{
		id: number;
		layerId: string;
		properties: Record<string, unknown>;
	}>;
	isolatedArea?: {
		type: string;
		coordinates: unknown;
	};
	metadata?: {
		pipeId: number;
		pipeLayerName: string;
		analysisTime: number;
		municipalityId?: string | number;
	};
};

/**
 * @typedef ownToc
 * Tipos para opciones del ownToc GoOwnToc.
 * @property {string} id Id del elemento GoOwnToc
 * @property {string} panelId Id del panel que renderiza el Toc
 * @property {string} mapId Id del mapa cargado desde el config
 * @property {constants.languages} lang Idioma del TOC
 * @property {constants.themes} theme Tema del TOC
 * @property {boolean} active Estado del elemento GoOwnToc
 * @property {maxHeight} maxHeight Tamaño máximo que va a tomar la caja del TOC/Legend
 * @property {Array<tocGroupLayer>} layers Grupo de layers dentro del GoOwnToc
 * @property {Array<tocGroupLegend>} legend Grupo de legend dentro del GoOwnToc
 * @property {boolean} searchLayersFilter Propiedad para activar search input en el TOC (default value true)
 */
export type ownToc = {
	id: string;
	panelId: string;
	mapId: string;
	lang: string;
	theme: constants.themes;
	active: boolean;
	maxHeight: number;
	layers: Array<tocGroupLayer>;
	groupLegend: Array<tocGroupLegend>;
	searchLayersFilter: boolean;
};

/**
 * @typedef tocGroupLayer
 * Tipo para grupo de Layers GoOwnToc.
 * @property {string} id Id del grupo de capas del GoOwnToc
 * @property {boolean} active Estado del grupo de capas del GoOwnToc
 * @property {boolean} visible Estado de visibilidad del grupo de capas del GoOwnToc
 * @property {string} header Nombre a mostrar del grupo de capas del GoOwnToc
 * @property {Array<tocLayer>} layer Elementos layer dentro del GoOwnToc
 */
export type tocGroupLayer = {
	id: string;
	active: boolean;
	visible: boolean;
	header: string;
	layer: Array<tocLayer>;
	switcher: boolean;
	onlyOneVisibleLayer?: boolean;
	folderExpandedByDefault?: boolean;
	folderAlwaysExpanded?: boolean;
};

/**
 * @typedef tocLayer
 * Tipo para Layer de GoOwnToc.
 * @property {string} id Id de una capa particular del GoOwnToc
 * @property {string} subfolder subcarpeta contenedora de la layer
 * @property {string} subfolderId Id de subcarpeta contenedora de la layer
 * @property {string} name Nombre de una capa particular del GoOwnToc
 * @property {boolean} active Estado de una capa particular del GoOwnToc
 * @property {boolean} visible Estado de una capa particular del GoOwnToc
 * @property {boolean} base_layer Capa base
 * @property {Array<string>} toc_link Capas linkadas
 */
export type tocLayer = {
	id: string;
	subfolder: string;
	subfolder_id: string;
	name: string;
	active: boolean;
	visible: boolean;
	base_layer: boolean;
	layerType: string;
	toc_link: Array<string>;
	filterTocSubmenu?: boolean;
};

/**
 * @typedef translation
 * Tipo para elemento de traducción.
 * @property {constants.languages} lang Identificador del idioma
 * @property {string} value Valor que adquiere la traducción
 */
export type translation = {
	lang: constants.languages;
	value: string;
};

/**
 * @typedef tocGroupLegend
 * Tipo para grupo de Legend GoOwnToc.
 * @property {string} id Id del grupo de leyenda del GoOwnToc
 * @property {string} name Nombre del grupo de leyenda del GoOwnToc
 * @property {boolean} active Estado del grupo de leyenda del GoOwnToc
 * @property {Array<tocElementLegend>} elements Elementos leyenda dentro del GoOwnToc
 */
export type tocGroupLegend = {
	id: string;
	name: string;
	active: boolean;
	legend: Array<tocElementLegend>;
};

/**
 * @typedef tocElementLegend
 * Tipo para elemento de Legend GoOwnToc.
 * @property {string} id Id del elemento de leyenda del GoOwnToc
 * @property {string} url Url del recurso del elemento de leyenda del GoOwnToc
 * @property {string} name Nombre del elemento de leyenda del GoOwnToc
 * @property {boolean} active Estado de la capa en la leyenda
 * @property {boolean} defaultVisibility Visibilidad por defecto de la capa
 */
export type tocElementLegend = {
	id: string;
	url: string;
	name: string;
	active: boolean;
	defaultVisibility: boolean;
};

/**
 * @typedef
 * Tipos para la creación del Staging GOStagingMap.
 */
export type staging = { map: OlMap; view: View; isLindek: boolean };

/**
 * @typedef layerOptions
 * Tipos para la creación de las capas que heredan de GOLayer.
 * @property {string} id identificador de la capa.
 * @property {string} alias Nombre de la capa en el TOC
 * @property {constants.GOLayerType} layerType Enumerado para indicar que tipo de capa deseamos instanciar
 * * GO_VECTOR_LAYER: Capa vectorial estandar OGC WFS.
 * * OSM: Capa WMTS de OSM.
 * * GO_MAPBOX: Capa de WMTS MapBox
 * * GO_WMS_LAYER: Capa TileLayer estandar OGC WMS.
 * * GO_WMTS_LAYER: Capa TileLayer estandar WMTS.
 * * GO_HEXGRID_LAYER : Capa Vectorial que agrupa en una malla hexagonal
 * * GO_HEATMAP_LAYER : Capa Vectorial con mapa de calor
 * * GO_CLUSTER_LAYER : Capa Vectorial que agrupa en Clusters
 * * GO_GEOJSON_LAYER : Capa Vectorial a la que se le puede añadir un  GeoJson de datos
 * * GO_XYZ : Capa Vector tiles de un externo.
 * * GO_VECTOR_TILE : Capa Vector Tiles propia
 * @property {string} [layerName] Nombre de la capa en el servidor de mapas donde se encuentra desplegada.(obligatorio para GO_VECTOR_LAYER, GO_WMS_LAYER y GO_WMTS_LAYER)
 * @property {boolean} [defaultVisibility] Visibilidad por defecto de la capa
 * @property {boolean} [useBboxFilter] Establece si se define filtro por BBOX
 * @property {string} [url] Ubicación del servidor de mapas donde se encuentra desplegada la capa (obligatorio para GO_VECTOR_LAYER, GO_WMS_LAYER y GO_WMTS_LAYER)
 * @property {boolean} [isBaseLayer] Indica si la capa se va a cargar como capa de fondo.
 * @property {string} [urlFolder] Estructura lógica de nodos en los que se agrupa una capa en el TOC eg: Folder1/Folder2/
 * @property {float} [minResolution] La mínima resolución (inclusiva) a la que la capa será visible.
 * @property {float} [maxResolution] La máxima resolución (exclusiva) por debajo de la cual, la capa será visible.
 * @property {styleOptions} [sldStyle] Ubicación y opciones de carga del archivo de estilos SLD
 * @property {number}[zIndex] Nivel de visualización de la capa
 * @property {number}[distance] Distancia que se usa para la calcular el Cluster (Solo valido para GO_CLUSTER_LAYER)
 * @property {string}[filter] Filtro a añadir para la capa (Solo valido para GO_VECTOR_LAYER y Capas que cuelgan de Geoserver)
 * @property {string}[filterOverload] Filtro que superpone al filtro inicial del mapa
 * @property {number}[opacity] Opacidad de la capa rangos [0-1]
 * @property {Array<clusterMode>}[styleCluster] Estilo para el cluster * (Solo valido para GO_CLUSTER_LAYER))
 * @property {ISVGIcon}[styleSVG] Estilo para las capas que precisen de un SVG (Solo valido para GO_VECTOR_LAYER)
 * @property {ISVGIcon}[styleSVGCluster] Estilo para la capa cluster (Solo valido para GO_CLUSTER_LAYER)
 * @property {ISVGIcon}[styleSVGClusterInd] Estilo para la capa cluster, con elementos apilados (Solo valido para GO_CLUSTER_LAYER)
 * @property {styleSVG}[styleSVGConditional] Estilo condicional
 * @property {stylbooleaneSVG}[filterTocSubmenu] Mostrar filtros en TOC
 * @property {styleGeojson}[styleGeojson] Estilo para la capa GeoJSON (Solo valido para GO_GEOJSON_LAYER)
 * @property {hexGridColor}[styleHexBin] Estilo para la capa HexGrid (Solo valido para GO_HEXGRID_LAYER)
 * @property {number}[radiusHeatMap] Radio del punto a representar en el heatmap (Solo valido para GO_HEATMAP_LAYER)
 * @property {number}[blurHeatMap] Blur del punto a representar en el heatmap (Solo valido para GO_HEATMAP_LAYER)
 * @property {string}[fieldHeatMap] Campo a representar en el heatmap. Valores entre 0 y 1. (Solo valido para GO_HEATMAP_LAYER)
 * @property {string}[clusterField] Valor por el cual se va a agrupar el Cluster (Solo valido para GO_CLUSTER_LAYER)
 * @property {GeoJSON}[data] Datos para añadir a la capa GEOJSON * (Solo valido para GO_GEOJSON_LAYER)
 * @property {boolean}[defaultClusterFunction] Uso por defecto del estilo para la capa Cluster (Solo valido para GO_CLUSTER_LAYER)
 * @property {number}[hexSize] Tamaño del mallado del Hexgrid. (Solo valido para GO_HEXGRID_LAYER)
 * @property {hexGridResolution}[hexGridResolution] Resolución del HexGrid (Solo valido para GO_HEXGRID_LAYER)
 * @property {boolean}[legend] Indicador para que se muestre o no la leyenda de esta capa
 * @property {boolean}[toc] Indicador para que se muestre o no la capa en el toc
 * @property {boolean}[envelope] Permite calcular la ubicación de los datos mostrados con el Cluster (Solo valido para GO_WFS_LAYER)
 * @property {boolean} [esri] La capa proviene de un servicio WFS de ESRI (Solo valido para GO_WFS_LAYER)
 * @property {string} [user] Usuario necesario para aquellos servicios WFS de Esri necesario un token (Solo valido para GO_WFS_LAYER)
 * @property {string} [password] Contraseña necesario para aquellos servicios WFS de Esri necesario un token (Solo valido para GO_WFS_LAYER)
 * @property {string} [WFStoken] Token necesario para algunas capas de Esri si viene dado por el cliente (Solo valido para GO_WFS_LAYER)
 * @property {styleWFS} [styleWFS] (Solo valido para GO_WFS_LAYER)
 * @property {string} [layerProjection] Sistema de referencia origen de la capa (Solo válido para GO_GEOJSON_LAYER o GO_CLUSTER_LAYER con origen de datos GeoJSON)
 * @property {string} [toProjection] Sistema de referencia destino de la capa (Solo válido para GO_GEOJSON_LAYER o GO_CLUSTER_LAYER con origen de datos GeoJSON)
 * @property {boolean} [visibilityAnnotationDef] Visibilidad de la anotación por defecto
 * @property {number} [offsetXAnnotationDef] Desplazamiento horizontal de la anotación por defecto
 * @property {number} [offsetYAnnotationDef] Desplazamiento vertical de la anotación por defecto
 * @property {string} [fontSizeAnnotationDef] Tamaño de fuente de la anotación por defecto
 * @property {string} [fontColorAnnotationDef] Color de fuente de la anotación por defecto
 * @property {number} [maxResolutionCluster] Resolución máxima hasta la que se aplica el cluster (Solo válido para GO_CLUSTER_LAYER)
 * @property {LayerInteractions} [interactions] Interacciones por defecto de la capa
 * @property {number} [reloadFrequency] Tiempo de frecuencia de recarga de capa  (Solo válido para GO_IOT_LAYER)
 * @property {styleSVGWebGLConditional} [styleSVGWebGLConditional] Estilo SVG condicional de capa renderizada con webGl (sólo válido para capas de puntos no clusterizados)
 * @property {string} [version] Versión de WMS por defecto 1.3.0 (optativo y sólo válido para capas GO_WMS_LAYER)
 * @property {string} [showTextOnUnique] Muestra etiqueta "1" para elementos cluster individuales (optativo y sólo válido para capas GO_CLUSTER_LAYER)
 * @property {boolean} [disableImageEffect] instancia VectorLayer en lugar de VectorImageLayer. Recomendado para capas pequeñas (optativo y sólo válido para capas GO_VECTOR_LAYER, GO_GEOJSON_LAYER, GO_CLUSTER_LAYER)
 * @property {Array<string>} [propertyName] Lista de nombres de atributos a recuperar en las consultas WFS (opcional y sólo válido para capas que tienen source originada en Geoserver)
 */
export type layerOptions = {
	id: string;
	alias: string;
	layerType: constants.GOLayerType;
	layerName?: string;
	defaultVisibility?: boolean;
	useBboxFilter?: boolean;
	url?: string;
	isBaseLayer?: boolean;
	urlFolder?: string;
	minResolution?: number;
	maxResolution?: number;
	sldStyle?: styleOptions;
	zIndex?: number;
	distance?: number;
	filter?: string;
	filterOverload?: string;
	opacity?: number;
	geoparams?: string;
	rsClientID?: string;
	rsInitialDate?: number;
	rsEndDate?: number;
	styleCluster?: Array<clusterMode>;
	styleSVG?: ISVGIcon;
	styleSVGCluster?: ISVGIcon;
	styleSVGClusterInd?: ISVGIcon;
	styleSVGConditional?: styleSVG;
	filterTocSubmenu?: boolean;
	styleGeojson?: styleGeojson;
	styleHexBin?: hexGridColor;
	radiusHeatMap?: number;
	blurHeatMap?: number;
	fieldHeatMap?: string;
	clusterField?: string;
	data?: any; // eslint-disable-line
	defaultClusterFunction?: boolean;
	hexSize?: number;
	hexGridResolution?: hexGridResolution;
	legend?: boolean;
	toc?: boolean;
	envelope?: boolean;
	esri?: boolean;
	user?: string;
	password?: string;
	WFStoken?: string;
	styleWFS?: styleWFS;
	layerProjection?: string;
	toProjection?: string;
	visibilityAnnotationDef?: boolean;
	offsetXAnnotationDef?: number;
	offsetYAnnotationDef?: number;
	fontSizeAnnotationDef?: string;
	fontColorAnnotationDef?: string;
	maxResolutionCluster?: number;
	interactions?: LayerInteractions;
	reloadFrequency?: number;
	labelNameIOT?: string;
	styleSVGWebGLConditional?: styleSVGWebGLConditional;
	version?: string;
	iotUrl?: string;
	iotToken?: string;
	iotVersion?: string;
	originData?: string;
	vt_type?: string;
	idEsri?: string;
	esriApiKey?: string;
	watermark?: string;
	showTextOnUnique?: boolean;
	sourceType?: constants.GoLayerSourceType;
	controlPoints?: Array<controlPoints>;
	listener?: Array<listenerControl>;
	timeNoSignal?: number;
	timeWorking?: number;
	timeNexusPosition?: number;
	timeReSyncRoute?: number;
	remoteSensingType?: string;
	vrsColors?: Array<string>;
	disableImageEffect?: boolean;
	defaultAnnotations?: Array<any>;
	style?: any;
	token?: string;
	propertyName?: Array<string>;
	geometryType?: LayerGeometryType;
	geometryField?: string;
};

export interface controlPoints {
	id: string;
	style: string;
	finishRoute?: boolean;
	visibleTruck?: boolean;
	fieldControl?: string;
}

export interface listenerControl {
	type: string;
	event: string;
	consoleText: string;
}

export interface LayerInteractions {
	hover?: HoverInteraction;
}

export type styleWFS = {
	radius: number;
	fillcolor: string;
	strokecolor: string;
};

export type paramsWebGLRenderer = {
	refresh?: boolean;
	cqlFilter?: string;
	useBboxFilter?: boolean;
};

export type symbolWebGLRenderer = {
	symbolType: constants.symbolType;
	size: Array<any> | number; // eslint-disable-line
	color: Array<any> | string; // eslint-disable-line
	rotateWithView?: boolean;
	opacity?: Array<any> | number; // eslint-disable-line
	src?: string;
	offset?: Array<any>; // eslint-disable-line
	textureCoord?: Array<number>; // eslint-disable-line
};

export type styleSVGWebGLConditional = {
	type: constants.webGLConditionType;
	conditions: Array<conditionSVGWebGl>;
	providedSprite?: string;
	filter?: any; // eslint-disable-line
	variables?: any; // eslint-disable-line
};

export type conditionSVGWebGl = {
	value: Array<any> | string; // eslint-disable-line
	icon: ISVGIcon;
};

/**
 * @typedef styleOptions
 * Tipos para dotar de estilo a capas que hereden de GOtyleLayer.
 * @property {string} [url] Ubicación del fichero de estilo SLD.
 * @property {boolean} mapUnits Indica si el tamaño de la geometría representada es real (en metros) o relativo a pantalla ( en pixels). Solo para líneas y puntos.
 * @property {string} [origin] Indica donde se aloja el fichero de estilos. GEOSERVER si está en geoserver u OTHER en otro caso
 */
export type styleOptions = {
	url: string;
	mapUnits?: boolean;
	origin?: string;
	geometryFilter?: filterNoCQL;
};
/**
 * Creates a new type named 'mapOptionsBasic'
 * @typedef mapOptionsBasic
 * @property {string} id - Identifier this Map
 * @property {string} div - Map location in your HTML file
 * @property {module:types~optionsView} optionView - View of map
 */
export type mapOptionsBasic = {
	id: string;
	div: string;
	optionView: optionsView;
	interactions?: LayerInteractions;
};

/**
 * Creates a new type named 'mapOptionsBasic'
 * @typedef interactions
 * @property {CloseTool} closeTool - CloseTool identifier
 * @property {boolean} rotation - Allow rotation in map
 * @property {boolean} perspective - Allow perspective in map
 */
export type interactions = {
	closeTool?: CloseTool;
	updownStream?: UpDown;
	rotation?: basicInteraction;
	perspective?: basicInteraction;
	hover?: basicInteraction;
};

export interface IconRotationOptions {
	rotationField?: string;
	rotationUnits?: AngleUnits;
	rotationAdd?: number;
	rotationInverse?: boolean;
}

export interface ISVGIcon extends IconRotationOptions {
	iconSvg?: string;
	iconUrl?: string;
	shape?: constants.IconShape;
	mode?: constants.IconMode | string;
	anchor?: [number, number];
	scale?: number;
	rotate?: number;
	backgroundColor?: string;
	iconColor?: string;
	highlight?: string;
	x?: number;
	y?: number;
	size?: number[];
	elevation?: number;
	haloSize?: number;
	fontColor?: string;
	fontSize?: string;
}

export interface HoverInteraction {
	active: boolean;
	layers?: Array<string>;
	style?: styleFromUser;
	styleSVG?: ISVGIcon;
	prefix?: string;
	zIndex?: number;
}

export interface ToolBase {
	active: boolean;
	filterLocationName: string;
	dataPipes: string[];
}

/**
 * Creates a new type named 'mapOptionsBasic'
 * @typedef CloseTool
 * @property {boolean} active - CloseTool activation
 * @property {Array<string>} dataPipes - Array of layers include in pipes selection
 * @property {Array<string>} dataValves - Array of layers include in valve selection
 */
export type CloseTool = ToolBase & {
	active: boolean;
	filterLocationName: string;
	dataPipes: Array<string>;
	dataValves: Array<string>;
};

/**
 * Creates a new type named 'mapOptionsBasic'
 * @typedef UpDown
 * @property {boolean} active - UpDown activation
 * @property {Array<string>} dataPipes - Array of layers include in pipes selection
 * @property {Array<string>} dataWells - Array of layers include in valve selection
 */
export type UpDown = ToolBase & {
	active: boolean;
	dataPipes: Array<string>;
	dataWells: Array<string>;
	filterLocationName: string;
};

/**
 * Creates a new type named 'mapOptionsBasic'
 * @typedef basicInteraction
 * @property {boolean} active - CloseTool activation
 * @property {string} idInteraction - Array of layers include in pipes selection
 */
export type basicInteraction = {
	active: boolean;
	idInteraction: string;
};

/**
 * Creates a new type named 'intersect'
 * @typedef intersect
 * @property {constants.intersectTypes.JSON |constants.intersectTypes.LAYER} type - Array of layers include in pipes selection
 * @property {GeoJSON | string} dataValves - Array of layers include in valve selection
 */
export type dataTools = {
	type: constants.dataToolTypes;
	data: GeoJSON | string;
};

/**
 * @typedef projOptions
 * Tipo para la creacion de una nueva proyección
 * @property {string} code - identificador de la proyección.
 * @property {string} [def] - definición Proj4 de la proyección.
 * @property {string} units - los valores validos son los siguientes: `m: metros` `degrees: grados` `pixels: pixels` `tile-pixels` `us-ft: pies`
 * @property {string} extent - el extent valido para la proyección
 * @property {string} [axisOrientation] - la orientacion del eje como se especifica en Proj4
 * @property {string} [global] - si la proyección es válida para todo el globo terrestre
 * @property {string} [metersPerUnit] - los metros por unidad para la proyección
 * @property {string} [worldExtent] - la extensión mundial de la proyección
 */
export type projOptions = {
	code: string;
	units: string;
	extent?: Extent;
	axisOrientation?: string;
	global?: boolean;
	metersPerUnit?: number;
	worldExtent?: Extent;
	def?: string;
};
/**
 * @typedef configOptions
 * Tipos para la creación un mapa completo viniendo de una configuración GOMap.
 */
export type configOptions = {
	GOMapOptions: Array<GOMapOptions>;
};

/**
 * Creates a new type named 'GOMapOptions'
 * @typedef GOMapOptions
 * @property {string} id - Identifier this Map
 * @property {string} div - Map location in your HTML file
 * @property {module:types~optionToc} optioEXnToc - TOC and Legends configuration
 * @property {module:types~optionsView} optionView - View of map
 * @property {string} filter - Filter
 * @property {module:types~layerOptions} layer - Array of layers to load in map
 * @property {module:types~interactions} interactions - Interactions of map
 * @property {Array<module:types~projOptions>} projections - Array of projections to load in map
 */
export type GOMapOptions = {
	id: string;
	div: string;
	lang: SupportedLangs;
	ui: UIConfig;
	optionToc?: optionsToc;
	optionView: optionsView;
	filter?: string;
	layer?: Array<layerOptions>;
	interactions?: interactions;
	projections?: Array<projOptions>;
	translations: ConfigTranslations;
};

export type ConfigTranslations = {
	[lang in SupportedLangs]: {
		[key: string]: string;
	};
};

export type SupportedLangs =
	(typeof SystemLanguages)[keyof typeof SystemLanguages];
export const SystemLanguages = {
	EN: "en",
	ES: "es",
	CA: "ca",
	FR: "fr",
	RO: "ro",
	IT: "it",
} as const;
export const supportedLangList: SupportedLangs[] =
	Object.values(SystemLanguages);

/**
 * Tipos para la creación de funciones de estilos GOStyleLayer.
 */
export type sldStyleFunction = {
	styleFunction: any; // eslint-disable-line
	styleType: constants.GOGeometryType;
};

/**
 * Tipo para la obtención de objeto SLD de estilo de Geoserver.
 */
export type sldConfiguration = {
	sldObject: any; // eslint-disable-line
	idLayer: string;
};

type CanvasLineCap = "butt" | "round" | "square";
type CanvasLineJoin = "round" | "bevel" | "miter";
type typePoint =
	| "triangle"
	| "star"
	| "cross"
	| "circle"
	| "hexagon"
	| "octagon"
	| "x";
type AngleUnits = "deg" | "grad" | "rad";

export interface StyleFromUserText {
	fillColor?: string;
	strokeColor?: string;
	defaultText?: string;
	defaultTextType?: constants.GoTextType;
}

export interface StyleFromUserGraphic {
	size: number;
	radiousSize?: string;
	typePoint?: typePoint;
	rotation?: number;
}

export interface StrokeOptions {
	color?: string;
	width?: number;
	lineCap?: CanvasLineCap;
	lineJoin?: CanvasLineJoin;
	lineDash?: number[];
	lineDashOffset?: number;
	miterLimit?: number;
}

export interface StrokeOptionsFlowLines {
	color: string;
	width?: number;
	lineCap?: CanvasLineCap;
	lineJoin?: CanvasLineJoin;
	lineDash?: number[];
}

export interface FillOptions {
	color?: string;
}

export interface StyleFromUserImg {
	img?: HTMLImageElement | HTMLCanvasElement;
	imgSize?: Size;
	anchor?: Array<number>;
	anchorOrigin?: any; // eslint-disable-line
	anchorXUnits?: any; // eslint-disable-line
	anchorYUnits?: any; // eslint-disable-line
	color?: string;
	crossOrigin?: string;
	offset?: Array<number>;
	offsetOrigin?: any; // eslint-disable-line
	opacity?: number;
	rotation?: number;
	size?: Size;
	src: string;
	scale?: number;
}

/**
 * @typedef styleFromUser
 * Tipo para la creación de un objeto de estilos dinamicos que el usuario podra generar al vuelo en una capa,
 * estos no se almacenaran en ningun caso en las propiedades de las capas, una vez cerrada la aplicación.
 *
 *  @property text: para dotar al estilo de un etiquetado
 *  @property fillColor: color de relleno del texto.
 *  @property strokeColor: color del borde del texto.
 *  @property defaultText: texto por defecto. Depende del valor de defaultTextType:
 *  - Si defaultTextType es LITERAL, el campo defaultText será la cadena de texto que se quiere representar.
 *  - Si defaultTextType es FIELDBASED, el campo defaultText será el nombre del campo de la capa donde se encuentran los textos a representar.
 *  @property defaultTextType:  LITERAL o FIELDBASED
 */
export type styleFromUser = {
	type?: constants.GOGeometryType;
	stroke?: StrokeOptions;
	fill?: FillOptions;
	icon?: boolean;
	img?: StyleFromUserImg;
	graphic?: StyleFromUserGraphic;
	text?: StyleFromUserText;
	geometryFilter?: [filterGeometry];
	zIndex?: number;
};

export type attrValue = { attr: string; value: string };

export type searchpropertys = {
	attrValues: Array<attrValue>;
	forceRequest?: boolean;
	searchScope?: constants.GOSearchScope;
	caseSensitive?: boolean;
	completeWords?: boolean;
};

export type selectValues = {
	select: Select;
	layerOwn: VectorLayer<VectorSource>;
};
export type sqlSearchpropertys = {
	sql: string;
	searchScope?: constants.GOSearchScope;
};

export interface BasicStyle {
	radius: number;
	fill: string;
	stroke: string;
}

export interface styleSVG extends IconRotationOptions {
	field: string;
	styleInfoSVG: [styleConditionSVG];
	geometryFilter?: filterNoCQL;
}

export type clusterStyle = BasicStyle & {
	labelFieldColor?: string;
	labelColor?: string;
	geometryFilter?: [filterGeometry];
};

export type clusterStyleCode = BasicStyle & {
	labelFieldColor?: string;
	labelColor?: string;
	geometryFilter?: [filterGeometry];
};

export type clusterMode = {
	size: number;
	style: clusterStyle;
	labelColor?: string;
	geometryFilter?: [filterGeometry];
};

export type styleConditionSVG = {
	condition: condition;
	styleSVG: ISVGIcon;
	geometryFilter?: filterNoCQL;
	legendAlias?: string | Array<translation>;
};

export type styleGeojson = {
	field?: string;
	styleInfo: [stylecondition];
	geometryFilter?: filterNoCQL;
};

export type stylecondition = {
	legendAlias?: string | Array<translation>;
	condition?: condition;
	style: styleFromUser;
};

export type condition = {
	type: constants.GoFilterCondition;
	condition:
		| Array<[number, number]>
		| number
		| string
		| Array<number>
		| Array<string>;
};

export type overlayData = {
	element: HTMLElement;
	position: olCoordinate.Coordinate;
};

export type allFeaturesProperties = Array<featuresProperties>;

export type featuresProperties = {
	layerId: string;
	features: Array<Feature>;
};

export type hexGridColor = {
	color: Array<Array<number>>;
	colorThersholds: Array<number>;
	fieldAgrupation?: string;
	border?: hexStyleBorder;
};

export type hexGridResolution = {
	resolution: Array<number>;
	size: Array<number>;
};

export type hexGridResolutionLevel = {
	min: number;
	max: number;
	resolution: string;
};

export type hexGridResolutionLevels = Array<hexGridResolutionLevel>;

export type hexStyleColors = {
	color?: Array<Array<number>>;
	colorThersholds?: Array<number>;
	border?: hexStyleBorder;
};

export type hexStyleBorder = {
	borderColor?: string | Array<number>;
	borderWidth?: number;
	lineDash?: Array<number> | undefined;
};

export type styleFlowLines = [
	{ stroke: StrokeOptionsFlowLines },
	{ stroke: StrokeOptionsFlowLines }
];

export type closeToolType = {
	valves: GeoJSON;
	pipes: GeoJSON;
};

export type nearDataAttributesToolType = {
	data: GeoJSON;
};

export type upDownType = {
	wells: GeoJSON;
	pipes: GeoJSON;
};

export type dataGeo = {
	url: string;
	layerName: string;
	mun_filter: string;
	mun_id: number | string;
};
export type dataRestGeo = Array<dataGeo>;

export type dataLayer = {
	url: string;
	layerName: string;
};

export type dataRestLayer = Array<dataLayer>;

export interface BasePipeParams {
	pipeId: number;
	pipeLayerName: string;
	dataPipes: Array<any> | dataRestGeo; // eslint-disable-line
	pipesLayerNames: Array<string>;
}

export type UpDownParams = BasePipeParams & {
	type: string;
	dataWells: Array<any> | dataRestGeo; // eslint-disable-line
	wellsLayerNames: Array<string>;
};

export type closeToolParams = BasePipeParams & {
	dataValves: Array<any>; // eslint-disable-line
	valvesLayerNames: Array<string>;
	noStudyValve?: Array<number>;
};

export type intersectParams = {
	dataIN: Array<any>; // eslint-disable-line
	InLayerNames: Array<string>;
	dataClip: Array<any>; // eslint-disable-line
	ClipLayerNames: Array<string>;
	zIndex?: number;
};

export type nearDataParams = {
	pointCoordinate: [number, number]; // eslint-disable-line
	lookingForData: Array<any>; // eslint-disable-line
	lookingForDataNames: Array<string>;
};

export type nearDataAttributesParams = {
	coordinates: [number, number];
	layerName: string;
	attribute: string;
	condition: constants.GoFilterCondition;
	value: any; // eslint-disable-line
	municipalityField?: string;
	municipalityId?: string | number;
};

export type bufferParams = {
	bufferData: Array<any>; // eslint-disable-line
	distance: number;
};

export type sectorColorParams = {
	attribute: string;
	palette_size: number;
};

/**
 * @typedef polygonSectorParams
 */
export type polygonSectorParams = {
	idMap: string;
	polygonSectorId: string;
	gisApiService: string;
	polygonSectorParams: polygonSectorDataParams;
};

/**
 * @typedef polygonSectorDataParams
 */
export type polygonSectorDataParams = {
	data: GeoJSON | string | dataLayer;
	grid_size: number;
	attribute: string;
};

export type interpolationParams = {
	idMap: string;
	idInterpolation: string;
	params: interpolationDataParams;
};

export type interpolationDataParams = {
	layer: {
		type: "GEOSERVER" | "GEOJSON";
		value: interpolationValueFormat;
		crs: string;
	};
	analysis_data: {
		attribute: string;
		start_date: string;
		finish_date: string;
		data_process: string;
		npoints: number;
		resolution: number;
		power: number;
		interval_line: number;
	};
};

export type isolinesParams = {
	idMap: string;
	idIsoline: string;
	params: isolinesDataParams;
};

export type isolinesDataParams = {
	layer: {
		type: string;
		value: isolineValueFormat;
		crs: string;
	};
	analysis_data: {
		attribute: string;
		start_date: string;
		finish_date: string;
		data_process: "MIN" | "MAX" | "AVG" | "SUM";
		npoints: number;
		resolution: number;
		power: number;
		interval_line: number;
	};
};

export type interpolationValueFormat = {
	url: string;
	layerName: string;
};

export type isolineValueFormat = {
	url: string;
	layer_name: string;
};

/**
 * @typedef opticalFberParams
 */
export type opticalFiberParams = {
	idMap: string;
	idOpticalFiber: string;
	params: opticalFiberDataParams;
};

/**
 * @typedef opticalFiberDataParams
 */
export type opticalFiberDataParams = {
	data: {
		pipes_layer: {
			type: string;
			value: any;
		};
		emisor_data: {
			definition_method: string;
			definition_values: Array<fiberParameters>;
		};
	};
};

/**
 * @typedef fiberParameters
 */
export type fiberParameters = {
	emisor_geometry: Array<number>;
	initial_offset: number;
	final_reading: number;
	step_distance: number;
	line_geometry: Array<Array<number>>;
	obstacles_data: Array<fiberObstacles>;
};

/**
 * @typedef fiberObstacles
 */
export type fiberObstacles = {
	element_tye: string;
	definition_type: string;
	value: Array<Array<number>>;
	offset_default: number;
};

/**
 * @typedef watershedParams
 */
export type watershedParams = {
	idMap: string;
	idWatershed: string;
	gisApiService: string;
	dataWatershed: watershedDataParams;
};

export type watershedDataParams = {
	originLayer: GeoJSON | string | dataLayer;
	dme: boolean;
	visible?: boolean;
	style?: styleFromUser;
};

/**
 * @typedef filterNoCQL
 */
export type filterNoCQL = { AND: boolean; filter: [filterGeometry] };

/**
 * @typedef filterGeometry
 */
export type filterGeometry = {
	field: string;
	type: constants.GoFilterCondition;
	conditional:
		| Array<[number, number]>
		| number
		| string
		| Array<number>
		| Array<string>;
};
/**
 * @typedef Annotation
 */
export interface Annotation {
	visibility: boolean;
	type?: string;
	attribute: string;
	condition?: constants.annotationCondition;
	value?:
		| Array<[number, number]>
		| number
		| string
		| Array<number>
		| Array<string>;
	text?: textAnnotation;
	image?: StyleFromUserImg;
	minResolution?: number;
	maxResolution?: number;
	showTextOnUnique?: boolean;
}

export interface BaseClusterAnnotation {
	showOnZero: boolean;
	showOnUnique?: boolean;
}

/**
 * @typedef clusterAnnotation
 */
export interface clusterAnnotation extends Annotation, BaseClusterAnnotation {}

/**
 * @typedef clusterExtAnnotation
 */
export interface clusterExtAnnotation
	extends ExtAnnotation,
		BaseClusterAnnotation {}

/**
 * @typedef ExtAnnotation
 */
export interface ExtAnnotation extends Annotation {
	source: GeoJSON;
	layerId?: string;
	layerValue?: number;
	fieldId: string;
	fieldIdExt: string;
}

/**
 * @typedef textAnnotation
 */
export type textAnnotation = {
	label?: string;
	offsetX?: number;
	offsetY?: number;
	fontColor?: string;
	fontSize?: string;
	stroke?: Stroke;
	textPlacement?: TextPlacement;
};

/**
 * @typedef geodesicCoordinates
 */
export type geodesicCoordinates = {
	lat: number;
	lng: number;
};

export interface IFCStructureProps {
	expressID: number;
	type: string;
	properties: IFCPropertiesOpts;
	children: Array<IFCStructureProps>;
}

export interface IFCPropertiesOpts {
	expressID: number;
	type: number;
	CompositionType?: {
		type: number;
		value: string | number;
	};
	GlobalId: {
		type: number;
		value: string | number;
	};
	OwnerHistory: {
		type: number;
		value: string | number;
	};
	Name: {
		type: number;
		value: string | number;
	};
	Description?: {
		type: number;
		value: string | number;
	};
	ObjectType: {
		type: number;
		value: string | number;
	};
	ObjectPlacement: {
		type: number;
		value: string | number;
	};

	Representation: {
		type: number;
		value: string | number;
	};
	Tag?: {
		type: number;
		value: string | number;
	};
	PredefinedType: {
		type: number;
		value: string | number;
	};
}

export interface GetFeaturesByLayerParams {
	idMap: string;
	layerId?: string;
	layerName?: string;
	cqlFilter?: string;
	maxFeatures?: number;
	propertyName?: string[];
}

export interface GeoJSONFeature {
	type: "Feature";
	geometry: {
		type: string;
		coordinates: any;
	};
	properties: Record<string, any>;
	id?: string | number;
}

export interface GeoJSONFeatureCollection {
	type: "FeatureCollection";
	features: GeoJSONFeature[];
	totalFeatures?: number;
	numberMatched?: number;
	numberReturned?: number;
	timeStamp?: string;
	crs?: {
		type: string;
		properties: {
			name: string;
		};
	};
}

/**
 * Enum for layer geometry types used in layer configuration.
 */
export enum LayerGeometryType {
	POINT = "POINT",
	LINE = "LINE",
	POLYGON = "POLYGON"
}

export type MeasureCallbackStart = (measurement: string) => void;
export type MeasureCallbackEnd = (feature: Feature) => void;

export interface AddMeasureParams {
	idMap: string;
	idMeasure: string;
	geometryType: constants.GOGeometryType;
	callbackStart: MeasureCallbackStart;
	callbackEnd?: MeasureCallbackEnd;
	style?: styleFromUser;
	defaultTooltip?: boolean;
	unitSystemSelected?: constants.unitSystem;
}

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Map\GoStagingMap.ts
import { GOMapOptions, LayerInteractions } from "@goTypes/index";
import GoLayerInteractions from "@interactions/GoLayerInteractions";
import { GoInteractions } from "@interactions/index";
import { GOLayer } from "@layers/GoLayer";
import { GOVectorLayer } from "@layers/GoVectorLayer";
import i18nService, { II18nService } from "@localization/index";
import GoOwnToc from "@toc/GoOwnToc";
import * as constants from "@utils/classes/GoError/constants";
import * as mapsErrorsMsg from "@utils/classes/GoError/errrosMsg/mapsErrors.msg";
import { mapsErrorHandler } from "@utils/classes/GoError/goErrorsHandlers";
import { themes } from "@utils/constants";
import OlMap from "ol/Map";
import BaseLayer from "ol/layer/Base";
import { GoIFCComponents } from "../Interactions/IFC";
import {
	GoRemoteSensing,
	GoVerticalRemoteSensing,
	GoWaterLeakRemoteSensing,
} from "../Layers";
import { MapLayerPreviewer } from "../UIMap/MapLayerPreviewer";
import { UIDisplay } from "../UIMap/UIDisplay";
import { SupportedLangs, SystemLanguages } from "../Utils/types/types";
import { GoView } from "./GoView";

declare global {
	interface Window {
		_langStorageListenerRegistered?: boolean;
	}
}
/**
 * Main staging map class that handles OpenLayers map management with additional features
 * such as layer management, theming, interactions, and localization support.
 * @class GOStagingMap
 * @param  {OlMap} map - OpenLayers map instance
 * @param  {GoView} view - Custom view wrapper for map view management
 * @param  {GoOwnToc} [ownToc] - Table of contents component for layer management
 */
export class GOStagingMap {
	/** Unique identifier for this map instance */
	private id: string;

	/** Table of contents component for managing layer visibility and order */
	private ownToc: GoOwnToc | undefined;

	/** Component for previewing map layers before adding them */
	private mapLayerPreviewer: MapLayerPreviewer | undefined;

	/** OpenLayers map instance - core map functionality */
	private map: OlMap;

	/** Current theme applied to the map (DARK/LIGHT) */
	private theme: themes = themes.DARK;

	/** Collection of all layers added to this map, indexed by layer ID */
	private layers: Map<string, GOLayer | BaseLayer>;

	/** Collection of filters applied to layers, indexed by layer ID */
	private filters: Map<string, string>;

	/** Custom view wrapper that extends OpenLayers view with additional functionality */
	private view: GoView;

	/** Map-level interactions manager (zoom, rotate, etc.) */
	private mapInteractions: GoInteractions;

	/** Layer-specific interactions manager (hover, click, etc.) per layer */
	private layerInteractions: Map<string, GoLayerInteractions>;

	/** UI display component for showing map controls and overlays */
	private uiDisplay: UIDisplay | undefined;

	/** Flag to track if theme has been set for the first time */
	private firstTimeSetTheme = true;

	/** Map configuration options including language, div container, etc. */
	private _mapOpts: GOMapOptions;

	/**
	 * Creates a new GOStagingMap instance with all necessary components
	 * @param {string} id - Unique identifier for this map instance
	 * @param {OlMap} map - OpenLayers map instance
	 * @param {GoView} view - Custom view wrapper for map view management
	 * @param {GoInteractions} mapInteractions - Map-level interactions manager
	 * @param {GOMapOptions} mapOpts - Configuration options for the map
	 */
	constructor(
		id: string,
		map: OlMap,
		view: GoView,
		mapInteractions: GoInteractions,
		mapOpts: GOMapOptions
	) {
		// Initialize core properties
		this.id = id;
		this.map = map;
		this._mapOpts = mapOpts;
		this.view = view;

		// Initialize collections for layers, filters, and interactions
		this.layers = new Map<string, GOLayer>();
		this.filters = new Map<string, string>();
		this.mapInteractions = mapInteractions;
		this.layerInteractions = new Map<string, GoLayerInteractions>();

		// Set up initial language configuration
		this.setLanguageFirstRender();
	}

	/**
	 * Completely unmounts and cleans up the map instance to prevent memory leaks.
	 * This method performs comprehensive cleanup of all map components, layers,
	 * interactions, and event listeners. Should be called when the map is no longer needed.
	 * @method
	 */
	public unmount() {
		// 1. Disable and remove all map interactions to prevent continued event handling
		const interactions = this.map.getInteractions?.();
		if (interactions) {
			interactions.forEach((interaction) => {
				interaction.setActive(false);
			});
			interactions.clear();
		}

		// 2. Clear overlays and controls to remove DOM elements
		this.map.getOverlayContainer()?.replaceChildren?.(); // Directly clear the DOM
		this.map.getOverlays?.().clear();
		this.map.getControls?.().clear();

		// 3. Unmount all custom layers if they have cleanup methods
		for (const goLayer of this.layers.values()) {
			if (goLayer instanceof GOLayer) {
				goLayer.cancelActiveRequests();
			}
			if (
				"unmount" in goLayer &&
				typeof (goLayer as any).unmount === "function"
			) {
				(goLayer as any).unmount();
			}
		}

		// 4. Remove layers, clear sources and call dispose() to free memory
		for (const layer of this.map.getLayers().getArray()) {
			if (!layer) continue;

			// 4.1. Clear listeners if using EventEmitter to prevent memory leaks
			if ((layer as any).off) {
				(layer as any).off(); // e.g., @billjs/event-emitter
			}

			// 4.2. Clear source if applicable to free vector data and tiles
			const source = (layer as any).getSource?.();
			if (source) {
				source.clear?.();
				source.dispose?.(); // e.g., ol.source.Vector
			}

			// 4.3. Dispose of the layer itself and remove from map
			(layer as any).dispose?.();
			this.map.removeLayer(layer);
		}

		this.map.getLayers().clear();

		// 5. Clear internal components that may have their own cleanup methods
		this.ownToc?.unmount?.();
		this.ownToc = undefined;

		this.mapInteractions?.unmount?.();
		this.mapInteractions = undefined as unknown as GoInteractions;

		this.mapLayerPreviewer = undefined;
		this.uiDisplay = undefined as unknown as UIDisplay;

		// 6. Remove references to view and options to break circular references
		this.view = undefined as unknown as GoView;
		this._mapOpts = undefined as unknown as GOMapOptions;
		this.id = undefined as unknown as string;
		this.theme = undefined as unknown as themes;
		this.firstTimeSetTheme = undefined as unknown as boolean;

		// 7. Fully dispose of the OpenLayers map instance
		this.map.setTarget?.(undefined);
		this.map.setView?.(null);
		this.map.dispose?.();
		this.map = undefined as unknown as OlMap;

		// 8. Clear all collections to free references
		this.layers?.clear?.();
		this.filters?.clear?.();
		this.layerInteractions?.clear?.();
	}

	public get mapOpts() {
		return this._mapOpts;
	}

	/**
	 * This function allow to get the id of the map. It is used to identify the map.
	 * @returns {string} Return the id of the map.
	 * @method
	 */
	public getId(): string {
		return this.id;
	}

	/**
	 * This function return the theme of this map
	 * @method
	 */
	public getTheme() {
		return this.theme;
	}

	/**
	 * This function allow to add a layer to the map.
	 * @param {string} idLayer - The id of the layer.
	 * @param {import('types').GOLayer} layer - The layer to add.
	 * @returns {void} Return nothing
	 * @method
	 */
	public addLayer(idLayer: string, layer: GOLayer | BaseLayer): void {
		if (!layer || !idLayer) {
			// Error Handler
			mapsErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.mapsErrors.MAPS_ERROR_NO_LAYER_LOADED,
				mapsErrorsMsg.MAPS_ERROR_NO_LAYER_LOADED + idLayer
			);
		}
		this.layers.set(idLayer, layer);
	}

	/**
	 * This function allow to remove the layer from the map.
	 * @param {string} idLayer - The id of the layer.
	 * @returns {void} Return nothing
	 * @method
	 */
	/**
	 * Removes a layer from both the map's layer collection and the OpenLayers map instance.
	 * This will make the layer invisible and remove it from rendering.
	 * @param {string} idLayer - The unique identifier of the layer to remove
	 * @returns {void} Return nothing
	 * @method
	 */
	public removeLayer(idLayer: string): void {
		// Get the layer reference before deletion
		const layer = this.layers.get(idLayer);

		// Remove from internal layer collection
		this.layers.delete(idLayer);

		// Remove from OpenLayers map instance to stop rendering
		this.map.removeLayer((layer as GOLayer).getLayer());
	}

	/**
	 * Gets the GoView instance associated with this map.
	 * The view controls the map's center, zoom level, rotation, and projection.
	 * @returns {GoView} The map's view instance for controlling viewport
	 * @method
	 */
	public getView(): GoView {
		return this.view;
	}

	/**
	 * Gets the underlying OpenLayers map instance.
	 * Provides access to the core map functionality and OpenLayers API.
	 * @returns {OlMap} The OpenLayers map instance
	 * @method
	 */
	public getMap(): OlMap {
		return this.map;
	}

	/**
	 * Sets the table of contents (TOC) component for this map.
	 * The TOC manages layer visibility, ordering, and provides layer controls.
	 * @param {GoOwnToc} ownToc - The table of contents component instance
	 * @returns {void} Return nothing
	 * @method
	 */
	public setOwnToc(ownToc: GoOwnToc): void {
		this.ownToc = ownToc;
	}

	/**
	 * This function allow to set the theme of the map.
	 * @param {themes} theme - The theme of the map
	 * @method
	 */
	public setTheme(theme: themes) {
		this.theme = theme;
		document
			.getElementById(this._mapOpts.div)
			?.setAttribute("ui-theme", theme.toLowerCase());

		//Change theme of the TOC and legend
		if (this.getOwnToc()) {
			this.getOwnToc()?.setTheme(theme);
		}

		const id = this.getId();
		this.mapInteractions.removeAllCustomZoom({ idMap: id });

		//zoom buttons
		this.mapInteractions.addCustomZoom({ idMap: id });

		//Rotate in case dont exist UIDisplay
		if (!this.uiDisplay) {
			this.mapInteractions?.addCustomRotateCompass({ idMap: this.id });
		}

		this.getLayers().forEach((layer) => {
			if (
				layer instanceof GoRemoteSensing ||
				layer instanceof GoVerticalRemoteSensing
			) {
				const ui = layer.getUI();
				ui.getTheme().setTheme(theme.toLowerCase() as never);
			}
		});
	}

	/**
	 * This function allow to get the Own toc of the map.
	 * @returns {import('types').GoOwnToc} Return the Own toc of the map.
	 * @method
	 */
	public getOwnToc() {
		return this.ownToc;
	}

	/**
	 * Initializes language configuration for the map on first render.
	 * Sets up cross-tab language synchronization and determines the initial language
	 * from localStorage, map config, or default system language.
	 * @private
	 * @method
	 */
	private setLanguageFirstRender() {
		// Set up event listener for cross-tab language synchronization (only once globally)
		if (!window._langStorageListenerRegistered) {
			window.addEventListener("storage", (event) => {
				// Listen for language changes in localStorage from other tabs
				if (event.key === "gis-lang" && event.newValue) {
					// Update i18n service with new language
					i18nService.setLanguage(event.newValue);

					// Update TOC component language if it exists
					this.getOwnToc()?.setLanguage(event.newValue);

					// Update UI display translations if it exists
					this.getUIDisplay()?.updateTranslation();

					// Prepare IFC registry reference (was missing)
					const ifcMap = this.getInteractions().getIFCRegistry();

					// Update language for remote sensing layers that have their own UI
					this.getLayers().forEach((layer) => {
						if (
							layer instanceof GoRemoteSensing ||
							layer instanceof GoVerticalRemoteSensing
						) {
							// Guard because layer UI initialization is async
							const ui =
								(layer as any).getUI && (layer as any).getUI();
							if (ui && typeof ui.getLang === "function") {
								const langUIRemoteSensing = ui.getLang();
								if (
									langUIRemoteSensing &&
									typeof langUIRemoteSensing.updateLang ===
										"function"
								) {
									langUIRemoteSensing.updateLang();
								}
							}
						}
					});

					// Update IFC components language (outside the layer loop)
					if (ifcMap) {
						ifcMap.forEach((innerMap) => {
							if (innerMap instanceof Map) {
								innerMap.forEach((value) => {
									const ifcc = value as GoIFCComponents;
									ifcc.updateLang();
								});
							}
						});
					}
				}
			});
			// Mark listener as registered to prevent duplicate listeners
			window._langStorageListenerRegistered = true;
		}

		// Determine language priority: localStorage > config > default
		const configLang = this._mapOpts?.lang;
		const LangLocalStorage = localStorage.getItem("gis-lang");

		let lang: string;
		const defaultLanguage = SystemLanguages.EN;

		if (LangLocalStorage) {
			// First priority: localStorage (persisted user preference)
			lang = LangLocalStorage;
		} else if (configLang) {
			// Second priority: map configuration language
			lang = configLang;
		} else {
			// Fallback: default system language
			lang = defaultLanguage;
		}

		// Validate if the language is supported, fallback to English if not
		if (!Object.values(SystemLanguages).includes(lang as SupportedLangs)) {
			console.error(`Unsupported language: '${lang}', applied (en).`);
			lang = defaultLanguage;
			localStorage.setItem("gis-lang", defaultLanguage);
		} else {
			localStorage.setItem("gis-lang", lang);
		}

		// Apply the determined language to the i18n service
		i18nService.setLanguage(lang);
	}

	/**
	 * Sets the map layer previewer component for this map.
	 * @param {MapLayerPreviewer} mapLayerPreviewer - The layer previewer component instance
	 * @returns {void} Return nothing
	 * @method
	 */
	public setMapLayerPreviewer(mapLayerPreviewer: MapLayerPreviewer): void {
		this.mapLayerPreviewer = mapLayerPreviewer;
	}

	/**
	 * Gets the map layer previewer component associated with this map.
	 * Returns undefined if no previewer has been set.
	 * @returns {MapLayerPreviewer|undefined} The layer previewer component instance
	 * @method
	 */
	public getMapLayerPreviewer() {
		return this.mapLayerPreviewer;
	}

	/**
	 * Gets the UI display component associated with this map.
	 * The UI display manages map controls, overlays, and user interface elements.
	 * @returns {UIDisplay} The UI display component instance
	 * @method
	 */
	public getUIDisplay() {
		return this.uiDisplay;
	}

	/**
	 * Sets the UI display component for this map.
	 * The UI display manages map controls, overlays, and user interface elements.
	 * @param {UIDisplay} uiDisplay - The UI display component instance
	 * @returns {void} Return nothing
	 * @method
	 */
	public setUIDisplay(uiDisplay: UIDisplay): void {
		this.uiDisplay = uiDisplay;
	}

	/**
	 * Retrieves a specific layer from the map by its unique identifier.
	 * Validates that the layer ID is provided and handles errors appropriately.
	 * @param {string} idLayer - The unique identifier of the layer to retrieve
	 * @returns {GOLayer|BaseLayer|undefined} The requested layer instance
	 * @method
	 */
	// eslint-disable-next-line
	public getLayer(idLayer: string): any {
		// Validate that layer ID is provided
		if (!idLayer) {
			// Error Handler: Missing layer ID
			mapsErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.mapsErrors.MAPS_ERROR_NO_LAYER_LOADED,
				mapsErrorsMsg.MAPS_ERROR_NO_LAYER_LOADED + idLayer
			);
		}
		return this.layers.get(idLayer);
	}

	/**
	 * This function allow to get all the layers in the map.
	 * @returns {import('types').GOLayer} Return all the layers in the map.
	 * @method
	 */
	public getLayers(): Map<string, GOLayer | BaseLayer> {
		return this.layers;
	}

	/**
	 * This function allow to get the number of layers in the map.
	 * @returns {number} Return the number of layers in the map.
	 * @method
	 */
	public getNumLayers(): number {
		return this.layers.size;
	}

	/**
	 * Adds a spatial or attribute filter to a specific vector layer.
	 * Only vector layers support filtering. The filter will be applied to the layer's features
	 * and can optionally use bounding box optimization for spatial queries.
	 * @param {string} idLayer - The unique identifier of the layer to filter
	 * @param {string} filter - The filter expression (CQL, OGC Filter, or custom format)
	 * @param {boolean} bbox - Whether to use bounding box optimization for spatial filtering
	 * @returns {void} Return nothing
	 * @method
	 */
	public addFilter(idLayer: string, filter: string, bbox: boolean): void {
		// Validate that the layer exists in the map
		if (!this.layers.has(idLayer)) {
			// Error Handler: Layer not found
			mapsErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.mapsErrors.MAPS_ERROR_NO_LAYER_LOADED,
				mapsErrorsMsg.MAPS_ERROR_NO_LAYER_LOADED + idLayer
			);
		}

		// Validate that the layer is a vector layer (only vector layers can be filtered)
		if (!(this.layers.get(idLayer) instanceof GOVectorLayer)) {
			// Error Handler: Layer is not filterable (not a vector layer)
			mapsErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.mapsErrors.MAPS_ERROR_LAYER_CANT_BY_FILTERED,
				mapsErrorsMsg.MAPS_ERROR_LAYER_CANT_BY_FILTERED + idLayer
			);
		}

		// Store the filter in the filters collection for reference
		this.filters.set(idLayer, filter);

		// Apply the filter to the vector layer with bbox optimization if specified
		(this.layers.get(idLayer) as GOVectorLayer).addFilter(filter, bbox);
	}

	/**
	 * Removes an existing filter from a specific vector layer.
	 * This will restore the layer to show all features without filtering.
	 * @param {string} idLayer - The unique identifier of the layer to remove filter from
	 * @returns {void} Return nothing
	 * @method
	 */
	public removeFilter(idLayer: string): void {
		// Validate that the layer exists in the map
		if (!this.layers.has(idLayer)) {
			// Error Handler: Layer not found
			mapsErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.mapsErrors.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS,
				mapsErrorsMsg.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS + idLayer
			);
		}

		// Validate that the layer is a vector layer (only vector layers can be filtered)
		if (!(this.layers.get(idLayer) instanceof GOVectorLayer)) {
			// Error Handler: Layer is not filterable (not a vector layer)
			mapsErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.mapsErrors.MAPS_ERROR_LAYER_CANT_BY_FILTERED,
				mapsErrorsMsg.MAPS_ERROR_LAYER_CANT_BY_FILTERED + idLayer
			);
		}

		// Remove the filter from the filters collection
		this.filters.delete(idLayer);

		// Remove the filter from the vector layer
		(this.layers.get(idLayer) as GOVectorLayer).removeFilter();
	}

	/**
	 * Configures layer-specific interactions such as hover effects for specified layers.
	 * This method sets up interactive behaviors that respond to user input on map layers.
	 * Only creates hover interactions if they are specified in the options.
	 * @param {LayerInteractions} interactionsOptions - Configuration object defining interaction behaviors
	 * @method
	 */
	public setInteractions(interactionsOptions: LayerInteractions): void {
		// Only proceed if hover interactions are configured
		if (!interactionsOptions.hover) {
			return;
		}

		// Apply hover interactions to each specified layer
		interactionsOptions.hover.layers?.forEach((layer: string) => {
			// Verify the layer exists in the map before adding interactions
			if (this.layers.has(layer)) {
				const layerInteract = (
					this.layers.get(layer) as GOLayer
				).addInteractions(interactionsOptions);

				// Store the interaction reference for later management
				this.layerInteractions.set(layer, layerInteract);
			}
		});
	}

	/**
	 * Gets the map-level interactions manager for this map instance.
	 * Provides access to zoom, rotation, and other map-wide interactive behaviors.
	 * @returns {GoInteractions} The interactions manager instance
	 * @method
	 */
	public getInteractions() {
		return this.mapInteractions;
	}

	/**
	 * Generates an image export of the current map view by compositing all visible layer canvases.
	 * Supports both PNG download and base64 string output for different use cases.
	 * This method preserves layer opacity, transformations, and background colors.
	 * @param {string} format - Export format: "base64" returns data URL, anything else triggers PNG download
	 * @param {string} [fileName] - Optional filename for download (without extension)
	 * @returns {string|void} Returns base64 data URL if format is "base64", otherwise initiates download
	 * @method
	 */
	public getMapImage(format: string, fileName?: string) {
		// Create a new canvas element to composite all map layers
		const mapCanvas = document.createElement("canvas");
		const size = this.map.getSize();

		// Validate that map has a defined size
		if (!size) {
			throw new ReferenceError(`Map size not found`);
		}

		// Set canvas dimensions to match map size
		mapCanvas.width = size[0];
		mapCanvas.height = size[1];
		const mapContext = mapCanvas.getContext("2d");

		// Validate that canvas context is available
		if (!mapContext) {
			throw new ReferenceError(`Canvas context not found`);
		}

		// Iterate through all OpenLayers canvas elements in the map viewport
		Array.prototype.forEach.call(
			this.map
				.getViewport()
				.querySelectorAll(".ol-layer canvas, canvas.ol-layer"),
			function (canvas) {
				// Skip invalid canvases with negative width
				if (canvas.width < 0) {
					return;
				}

				// Preserve layer opacity from CSS styles
				const opacity =
					canvas.parentNode.style.opacity || canvas.style.opacity;
				mapContext.globalAlpha = opacity === "" ? 1 : Number(opacity);

				let matrix;
				const transform = canvas.style.transform;

				if (transform) {
					// Extract transformation matrix from CSS transform property
					// Handles cases where layers are transformed (rotated, scaled, translated)
					matrix = transform
						.match(/^matrix\(([^(]*)\)$/)[1]
						.split(",")
						.map(Number);
				} else {
					// Create default scaling matrix based on CSS dimensions vs canvas dimensions
					matrix = [
						parseFloat(canvas.style.width) / canvas.width,
						0,
						0,
						parseFloat(canvas.style.height) / canvas.height,
						0,
						0,
					];
				}

				// Apply the transformation matrix to the export canvas context
				CanvasRenderingContext2D.prototype.setTransform.apply(
					mapContext,
					matrix
				);

				// Apply background color if specified
				const backgroundColor = canvas.parentNode.style.backgroundColor;
				if (backgroundColor) {
					mapContext.fillStyle = backgroundColor;
					mapContext.fillRect(0, 0, canvas.width, canvas.height);
				}

				// Draw the layer canvas onto the export canvas
				mapContext.drawImage(canvas, 0, 0);
			}
		);

		// Reset canvas context to default state
		mapContext.globalAlpha = 1;
		mapContext.setTransform(1, 0, 0, 1, 0, 0);

		// Handle filename: use provided name or default to "mymap"
		if (!fileName) {
			fileName = "mymap";
		} else {
			// Remove file extension if provided (we'll add .png)
			fileName = fileName.split(".")[0];
		}

		// Return base64 data URL if requested, otherwise trigger download
		if (format == "base64") {
			return mapCanvas.toDataURL();
		}

		// Create download link and trigger automatic download
		const link = document.createElement("a");
		link.setAttribute("download", fileName + ".png");
		link.href = mapCanvas.toDataURL();
		link.click();
	}

	/**
	 * Adds or updates a single translation entry in the local map instance configuration.
	 * If the key already exists for a language, it preserves existing translations for other languages.
	 * This method ensures that translation updates are isolated to this specific map instance.
	 * @param {Record<string, Record<SupportedLangs, string>>} translation - Object containing the key and its translations by language
	 * @method
	 */
	public addOrUpdateTranslations(
		translation: Record<string, Record<SupportedLangs, string>>
	): void {
		// Ensure translations object exists in map options
		if (!this._mapOpts.translations) {
			this._mapOpts.translations = {
				en: {},
				es: {},
				ca: {},
				fr: {},
				ro: {},
				it: {},
			};
		}

		// Process each translation key provided
		Object.keys(translation).forEach((key) => {
			const translationsByLanguage = translation[key];

			// For each language in the provided translations
			Object.keys(translationsByLanguage).forEach((lang) => {
				const langKey = lang as SupportedLangs;

				// Ensure the language object exists in the translations structure
				if (!this._mapOpts.translations?.[langKey]) {
					if (this._mapOpts.translations) {
						this._mapOpts.translations[langKey] = {};
					}
				}

				// Set the translation for this key and language
				if (this._mapOpts.translations?.[langKey]) {
					this._mapOpts.translations[langKey][key] =
						translationsByLanguage[langKey];
				}
			});
		});
	}

	public getConfigTranslation(key: string): string {
		if (!key) return "(empty text)";

		const lang = i18nService.getLanguage();
		const cleanedKey = key.replace(/^\{\{__/, "").replace(/__\}\}$/, "");
		const options = this._mapOpts?.translations?.[lang];

		if (options && options[cleanedKey]) {
			return options[cleanedKey];
		} else {
			// If translation not found, return the key itself as a fallback
			return cleanedKey;
		}
	}

	public getI18nService(): II18nService {
		return i18nService;
	}
}

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\UIMap\types.ts
export interface UIConfig {
	showUserLocationButton?: boolean;
	showFullScreenButton?: boolean;
	showAddressSearch?: boolean;
	showInfoButton?: boolean;
	showLayersPreviewer?: boolean;
	showPointSelectionButton?: boolean;
	showLinearMeasurementButton?: boolean;
	showPolygonMeasurementButton?: boolean;
	showMeasurementByPolygonsButton?: boolean;
	showFilterButton?: boolean;
	showMap3DButton?: boolean;
	showExportButton?: boolean;
}

export enum IOrientation {
	LEFT = "LEFT",
	RIGHT = "RIGHT",
}

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Interactions\Features\index.ts
export { GoEditFeatures } from "./interact/GoEditFeatures";
export * from "./interact/getAllFeaturesProperties.methods";
export {
	GoHighlight,
	addHighlight,
	hasHighlight,
	getHighlight,
	changeHighlightZIndex,
	changeStyleOfHighlight,
	removeHighlightIfExist,
	restoreHighlightStyles,
	removeHighlight,
} from "./interact/Highlight";
export {
	GoHoverFeature,
	GoHover,
	hasHoverFeature,
	getLayerHover,
	mouseHover,
	removeHover,
	hasGetFeature,
	addTooltipHoverFeature,
	hasToolTipHover,
	getTooltipHoverFeature,
	removeTooltipHoverFeature,
} from "./interact/Hover";
export * from "./interact/GetFeaturesByLayer";

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Interactions\functions\getFeaturesByLayer.ts
export { getFeaturesByLayer } from "../Features/interact/GetFeaturesByLayer";

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Services\GoStore\index.ts
import { GoState } from "../interfaces";

/**
 * This class allow to store all the current state of the map.
 * @class GoStore
 */

export class GoStore {
	public static state: GoState = {};

	/**
	 * Store the current state of the project
	 * 
	 * @returns {GoState} The current state of the project.
	 */
	public static getState = (): GoState => {
		// return the current state of the project
		return GoStore.state;
	};

	/**
	 * Dispatch changes to the current state of the project
	 * 
	 * @param {Type} payload - The value to store in the state
	 * @returns  {Record<string, Type>}  - Return the state of the project
	 * @method
	 */
	public static dispatch = (payload): GoState => {
		GoStore.state = { ...GoStore.state, ...payload };
		return GoStore.state;
	};
}

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Utils\constants\index.ts
/**
 * @module constants
 * This file contains enums and constants used throughout the GIS application.
 */
/**
 * Enum for layer types used in the application.
 */
export enum GOLayerType {
	GO_HEXGRID_LAYER = "GO_HEXGRID_LAYER",
	GO_HEXGRID = "GO_HEXGRID",
	GO_HEATMAP_LAYER = "GO_HEATMAP_LAYER",
	GO_VECTOR_LAYER = "GO_VECTOR_LAYER",
	GO_IOT_LAYER = "GO_IOT_LAYER",
	OSM = "OSM",
	GO_WMS_LAYER = "GO_WMS_LAYER",
	GO_WMTS_LAYER = "GO_WMTS_LAYER",
	GO_CLUSTER_LAYER = "GO_CLUSTER_LAYER",
	GO_GEOJSON_LAYER = "GO_GEOJSON_LAYER",
	GO_MAPBOX = "GO_MAPBOX",
	GO_XYZ = "GO_XYZ",
	GO_VECTOR_TILE = "GO_VECTOR_TILE",
	GO_WFS_LAYER = "GO_WFS_LAYER",
	GO_REMOTE_SENSING = "GO_REMOTE_SENSING",
	GO_VECTOR_TILE_OWN = "GO_VECTOR_TILE_OWN",
	GO_ESRI_BASEMAP = "GO_ESRI_BASEMAP",
	GO_WEBGL_LAYER = "GO_WEBGL_LAYER",
	GO_TRUCK_LAYER = "GO_TRUCK_LAYER",
}

/**
 * Enum for layer style types.
 */
export enum GoLayerStyleType {
	VECTOR = "VECTOR",
	GEOJSON = "GEOJSON",
	CLUSTER = "CLUSTER",
}

/**
 * Enum for layer source types.
 */
export enum GoLayerSourceType {
	GEOSERVER = "GEOSERVER",
	GEOJSON = "GEOJSON",
}

/**
 * Enum for style variants.
 */
export enum GoStyleVariant {
	JSON = "JSON",
	JSON_CONDITIONAL = "JSON_CONDITIONAL",
	SVG = "SVG",
	SVG_CONDITIONAL = "SVG_CONDITIONAL",
	// TODO: Pending implement of style history for this cases
	WEBGL_JSON = "WEBGL_JSON",
	WEBGL_JSON_CONDITIONAL = "WEBGL_JSON_CONDITIONAL",
	WEBGL_SVG = "WEBGL_SVG",
	WEBGL_SVG_CONDITIONAL = "WEBGL_SVG_CONDITIONAL",
	// END TODO
	SLD = "SLD",
}

/**
 * Enum for registered environments.
 */
export enum RegistedEnviroments {
	PRODUCTION = "PRODUCTION",
	DEVELOPMENT = "DEVELOPMENT",
	TEST = "TEST",
}

/**
 * Enum for registered applications.
 */
export enum RegistedApps {
	WORK_ORDERS = "WORK_ORDERS",
	GIS_API_DOCS = "GIS_API_DOCS",
}

/**
 * Enum for registered clients.
 */
export enum RegistedClients {
	HOUSTON = "HOUSTON",
	IDRICA = "IDRICA",
}

/**
 * Enum for hexagon aggregation conditions.
 */
export enum HexConditions {
	AVG = "AVG",
	SUM = "SUM",
	COUNT = "COUNT",
}

/**
 * Enum for layer events.
 */
export enum GOLayerEvents {
	LAYER_ADDED = "LAYER_ADDED",
	LAYER_ERROR = "LAYER_ERROR",
	LAYER_LOADED = "LAYER_LOADED",
	LAYER_LOADING = "LAYER_LOADING",
	LAYER_REMOVED = "LAYER_REMOVED",
	LAYER_CHANGED = "LAYER_CHANGED",
	LAYER_STYLE_CHANGED = "LAYER_STYLE_CHANGED",
}

/**
 * Enum for feature events.
 */
export enum GOFeaturesEvents {
	FEATURES_LOAD_STARTED = "FEATURES_LOAD_STARTED",
	FEATURES_LOAD_ENDED = "FEATURES_LOAD_ENDED",
	FEATURES_LOAD_ERROR = "FEATURES_LOAD_ERROR",
}

/**
 * Enum for TOC (Table of Contents) events.
 */
export enum GoOwnTocEvents {
	TOC_ADDED = "TOC_ADDED",
	TOC_ERROR = "TOC_ERROR",
	TOC_LOADED = "TOC_LOADED",
	LAYER_CHANGE_VISIBILITY = "LAYER_CHANGE_VISIBILITY",
	TOC_LOADING = "TOC_LOADING",
}

/**
 * Enum for map events.
 */
export enum GOMapEvents {
	MAP_ERROR = "MAP_ERROR",
	MAP_LOADED = "MAP_LOADED",
	MAP_LOADING = "MAP_LOADING",
	BASE_LAYER_CHANGED = "BASE_LAYER_CHANGED",
	LAYER_VISIBILITY_CHANGED = "layer-visibility-changed",
}

/**
 * Enum for interaction events.
 */
export enum GoInteractionEvents {
	PRINT = "PRINT",
	PRINT_ERROR = "PRINT_ERROR",
	ENTER_FULLSCREEN = "ENTER_FULLSCREEN",
	EXIT_FULLSCREEN = "EXIT_FULLSCREEN",
}

/**
 * Enum for map controls.
 */
export enum GOControl {
	ATTRIBUTION = "ATTRIBUTION",
	FULL_SCREEN = "FULL_SCREEN",
	MOUSE_POSITION = "MOUSE_POSITION",
	OVERVIEW_MAP = "OVERVIEW_MAP",
	ROTATE = "ROTATE",
	SCALE_LINE = "SCALE_LINE",
	ZOOM_SLIDER = "ZOOM_SLIDER",
	ZOOM_TO_EXTENT = "ZOOM_TO_EXTENT",
	ZOOM = "ZOOM",
}

/**
 * Enum for data tool types.
 */
export enum dataToolTypes {
	JSON = "JSON",
	LAYER = "LAYER",
}

/* Enumerado para el tipo de geometrias */
/**
 * Enum for geometry types.
 */
export enum GOGeometryType {
	POINT = "POINT",
	LINE_STRING = "LINE_STRING",
	POLYGON = "POLYGON",
	ALL = "ALL",
	NOTGEOM = "NOT_GEOM",
}

/**
 * Enum for label text types.
 */
export enum GoTextType {
	LITERAL = "LITERAL",
	FIELDBASED = "FIELDBASED",
}

/* Enumerado para definir el tipo de capas */
/**
 * Enum for layer set types.
 */
export enum GOLayerSet {
	ALL,
	VISIBLE,
	NO_VISIBLE,
	FILTERLAYERS,
	NOFILTERLAYERS,
}

/**
 * Enum for upstream/downstream types.
 */
export enum UpDownType {
	upstream = "upstream",
	downstream = "downstream",
}

/* Enumerado para la clase de centrado */
/**
 * Enum for extent factors (used for map centering).
 */
export enum ExtentFactor {
	NULL = 0.0,
	FACTOR1 = 100000,
	FACTOR2 = 50000,
	FACTOR3 = 25000,
	FACTOR4 = 10000,
	FACTOR5 = 5000,
}

/* Enumerado para definir las opciones de filtrado */
/**
 * Enum for filter options in overload scenarios.
 */
export enum GOOverloadFilterOption {
	OVERLOAD = "OVERLOAD",
	ANDFILTER = "AND",
	ORFILTER = "OR",
}

/* Enumerado para la definición del ambito de búsqueda */
/**
 * Enum for search scope options.
 */
export enum GOSearchScope {
	VISIBLE_EXTENT,
	COMPLETE_EXTENT,
}

/* Enumerado para los tipos de selección */
/**
 * Enum for selection types.
 */
export enum GOTypeSelect {
	SELECT,
	DRAGBOX,
}

/**
 * Enumerado para los Proveedores de servicios de geocodificación
 */
/**
 * Enum for geocoder providers.
 */
export enum GOGeocoderProvider {
	OSM = "OSM",
	IGN = "IGN",
}

/**
 * Enum for geocoder service types.
 */
export enum GOGeocoderServiceType {
	CANDIDATES = "CANDIDATES",
	FIND = "FIND",
}

/**
 * Enum for style origin.
 */
export enum GOStyleOrigin {
	GEOSERVER = "GEOSERVER",
	OTHER = "OTHER",
}

/**
 * Constant for today's date loader (used as a placeholder).
 */
export const GODateTodayLoader = "#TODAY#";

/**
 * Constant for today's date (used as a placeholder).
 */
export const GO = "#TODAY#";

/**
 * Enum for hydraulic element types.
 */
export enum typeHidraulic {
	MANUALVALVE = "manualValve",
	METER = "meter",
	JUNCTION = "junction",
	FLOWMETER = "flowmeter",
	BOMBA = "rebombeo",
	CAUDALIMETRO = "caudalimetro-potable",
	CONTADOR = "contador-potable",
	HIDRANTE = "hidrante",
	MANANTIAL = "manantial",
	MEDICIONCAUDAL = "medicion-caudal",
	MEDICIONCAUDALFO = "medicion-caudal-fo",
	MEDICIONPRESION = "estacion_medicion_presion",
	PLANTACLORADORA = "planta-cloradora",
	PLANTACLORADORAFO = "planta-cloradora-fo",
	PLANTADEPURADORA = "planta_tratadora_aguas_residuales",
	PLANTAPOTABILIZADORA = "planta-potabilizadora",
	PLANTAPOTABILIZADORAFO = "planta-potabilizadora-fo",
	POZO = "pozo",
	PUMP = "pump",
	SENSORGENERAL = "sensor-general",
	SENSORWATER = "sensor-water",
	TANK = "tank",
	TANQUE = "tanque",
	TREATMENTPLANT = "treatment-plant",
	TRIFURCACION = "trifurcacion_bifurcacion",
	VALVE = "valve",
	VALVECLOSE = "valve-close",
	VALVULA = "valvula",
	VALVULACERRADA = "valvula-cerrada",
	VALVULAREVISION = "valvula_rev",
	VARIOUS = "various-potable",
	WELL = "well",
}

/**
 * Enum for filter conditions.
 */
export enum GoFilterCondition {
	EQUAL_TO = "EQUAL TO",
	NOT_EQUAL_TO = "NOT EQUAL TO",
	BETWEEN = "BETWEEN",
	NOT_BEETWEEN = "NOT BETWEEN",
	GREATER_THAN = "GREATER THAN",
	GREATER_THAN_OR_EQUAL_TO = "GREATER THAN OR EQUAL TO",
	LESS_THAN = "LESS THAN",
	LESS_THAN_OR_EQUAL_TO = "LESS THAN OR EQUAL TO",
	IS_NULL = "IS NULL",
	IS_NOT_NULL = "IS NOT NULL",
	IS_EMPTY = "IS EMPTY",
	IS_NOT_EMPTY = "IS NOT EMPTY",
	IS_IN = "IS IN",
	IS_NOT_IN = "IS NOT IN",
	IS_LIKE = "IS LIKE",
	IS_NOT_LIKE = "IS NOT LIKE",
}

/**
 * Enum for annotation conditions.
 */
export enum annotationCondition {
	EQUAL_TO = "EQUAL TO",
	NOT_EQUAL_TO = "NOT EQUAL TO",
	BETWEEN = "BETWEEN",
	NOT_BEETWEEN = "NOT BETWEEN",
	GREATER_THAN = "GREATER THAN",
	GREATER_THAN_OR_EQUAL_TO = "GREATER THAN OR EQUAL TO",
	LESS_THAN = "LESS THAN",
	LESS_THAN_OR_EQUAL_TO = "LESS THAN OR EQUAL TO",
	IS_NULL = "IS NULL",
	IS_NOT_NULL = "IS NOT NULL",
	IS_EMPTY = "IS EMPTY",
	IS_NOT_EMPTY = "IS NOT EMPTY",
	IS_IN = "IS IN",
	IS_NOT_IN = "IS NOT IN",
	IS_LIKE = "IS LIKE",
	IS_NOT_LIKE = "IS NOT LIKE",
	SUM = "SUM",
}

/**
 * Enum for geometry types used in measurement tools.
 */
export enum measureGeometry {
	lineString = "LINESTRING",
	polygon = "POLYGON",
}

/**
 * Enum for event types on map objects.
 */
export enum typeOn {
	"change" = "change",
	"change:layerGroup" = "change:layerGroup",
	"change:size" = "change:size",
	"change:target" = "change:target",
	"change:view" = "change:view",
	"click" = "click",
	"dblclick" = "dblclick",
	"error" = "error",
	"moveend" = "moveend",
	"movestart" = "movestart",
	"pointerdrag" = "pointerdrag",
	"pointermove" = "pointermove",
	"postcompose" = "postcompose",
	"postrender" = "postrender",
	"precompose" = "precompose",
	"propertychange" = "propertychange",
	"rendercomplete" = "rendercomplete",
	"singleclick" = "singleclick",
}

/**
 * Enum for event types on overlays.
 */
export enum typeOnOverlay {
	"change" = "change",
	"change:element" = "change:element",
	"change:map" = "change:map",
	"change:offset" = "change:offset",
	"change:position" = "change:position",
	"change:positioning" = "change:positioning",
	"propertychange" = "propertychange",
}

/**
 * Enum for density tool types.
 */
export enum densityToolType {
	HEATMAP = "HEATMAP",
	HEXGRID = "HEXGRID",
	ISOLINE = "ISOLINE",
	ISOBAND = "ISOBAND",
}

/**
 * Enum for supported languages.
 */
export enum languages {
	ES = "ES",
	EN = "EN",
}

/**
 * Enum for supported themes.
 */
export enum themes {
	DARK = "DARK",
	LIGHT = "LIGHT",
}
/**
 * Enum for IFC (Industry Foundation Classes) events.
 */
export enum IFCEvents {
	CHANGE = "CHANGE",
	CLICK = "CLICK",
	DBLCLICK = "DBLCLICK",
	ERROR = "ERROR",
	MOVE = "MOVE",
	MOVEEND = "MOVEEND",
	MOVESTART = "MOVESTART",
	POINTERDRAG = "POINTERDRAG",
	POINTERMOVE = "POINTERMOVE",
	MODEL_LOADED = "MODEL_LOADED",
	LOADING = "LOADING",
	LOADED = "LOADED",
}

/**
 * Enum for Remote Sensing UI events.
 */
export enum VRSRSUIEvents {
	RANGE_SLIDER_VALUES_CHANGED = "RANGE_SLIDER_VALUES_CHANGED",
}

/**
 * Enum for supported page sizes.
 */
export enum pageSize {
	A5 = "a5",
	A4 = "a4",
	A3 = "a3",
	A2 = "a2",
	A1 = "a1",
	A0 = "a0",
}

/**
 * Enum for A5 page dimensions (mm).
 */
export enum pageDimensionA5 {
	X = 148,
	Y = 210,
}

/**
 * Enum for A4 page dimensions (mm).
 */
export enum pageDimensionA4 {
	X = 210,
	Y = 297,
}

/**
 * Enum for A3 page dimensions (mm).
 */
export enum pageDimensionA3 {
	X = 297,
	Y = 420,
}

/**
 * Enum for A2 page dimensions (mm).
 */
export enum pageDimensionA2 {
	X = 420,
	Y = 594,
}

/**
 * Enum for A1 page dimensions (mm).
 */
export enum pageDimensionA1 {
	X = 594,
	Y = 841,
}

/**
 * Enum for A0 page dimensions (mm).
 */
export enum pageDimensionA0 {
	X = 841,
	Y = 1189,
}

/**
 * Enum for icon modes (used for status indication).
 */
export enum IconMode {
	SELECTED = "selected",
	INACTIVE = "inactive",
	WARNING_YELLOW = "warning-yellow",
	WARNING_ORANGE = "warning-orange",
	ERROR = "error",
}

/**
 * Enum for icon shapes.
 */
export enum IconShape {
	MARKER = "marker",
	PIN = "pin",
	CLUSTER = "cluster",
	BADGE = "badge",
	STACK = "stack",
}

/**
 * Enum for symbol types.
 */
export enum symbolType {
	IMAGE = "image",
	TRIANGLE = "triangle",
	CIRCLE = "circle",
	SQUARE = "square",
}

/**
 * Enum for WebGL condition types.
 */
export enum webGLConditionType {
	CASE = "case",
	MATCH = "match",
}

/**
 * Enum for unit systems.
 */
export enum unitSystem {
	METRIC = "metric",
	IMPERIAL = "imperial",
}

export default {};

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\common\terrainColors.ts
export const TERRAIN_RANGE_COLORS = {
	main: {
		high: "rgb(163, 1, 1)",
		medium: "rgb(136, 68, 68)",
		low: "rgb(75, 75, 75)",
		default: "rgb(128, 128, 128)",
	},
} as const;

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Interactions\geoTools\buffer\index.ts
import { Feature, Geometry, GOStagingMap } from "@map/index";
import GoServiceBuffer from "@services/GeoTools/buffer";
import {
	addIdToObj,
	bufferParams,
	gisColor,
	ISVGIcon,
	styleFromUser,
	StyleUtils,
} from "@utils/index";
import { CancelablePromise } from "cancelable-promise";
import GeoJSON from "ol/format/GeoJSON";
import VectorLayer from "ol/layer/Vector";
import VectorSource from "ol/source/Vector";
import { Circle, Fill, Stroke, Style } from "ol/style";

/**
 * Class representing a GoBuffer.
 */
export class GoBuffer {
	/**
	 * @private
	 * @type {VectorLayer<Feature<Geometry>>}
	 */
	private __bufferLayer: VectorLayer<Feature<Geometry>>;
	/**
	 * @private
	 * @type {GeoJSON | undefined}
	 */
	private __serviceBuffer: GeoJSON | undefined;
	/**
	 * @private
	 * @type {bufferParams}
	 */
	private __paramService: bufferParams;
	/**
	 * @private
	 * @type {Array<unknown>}
	 */
	private __dataBuffer: Array<unknown>;
	/**
	 * @private
	 * @type {number}
	 */
	private __distance: number;

	/**
	 * @type {Style | styleFromUser}
	 */
	public style: Style | styleFromUser | undefined;
	/**
	 * @type {Array<string>}
	 */
	public lookingForDataNames: Array<string>;
	/**
	 * @type {string}
	 */
	public pipeLayerName: string;
	/**
	 * @type {ISVGIcon}
	 */
	public stylePoint: ISVGIcon;

	/**
	 * Fill style for the buffer.
	 * @type {Fill}
	 * @readonly
	 */
	public static readonly fill = new Fill({
		color: gisColor.BUFFER_COLOR_FILL,
	});
	/**
	 * Stroke style for the buffer.
	 * @type {Stroke}
	 * @readonly
	 */
	public static readonly stroke = new Stroke({
		color: gisColor.BUFFER_COLOR_STROKE,
		width: 1,
		lineDash: [5, 5],
	});

	/**
	 * Default style for the buffer.
	 * @type {Style}
	 * @readonly
	 */
	public static readonly defaultStyle = new Style({
		image: new Circle({
			radius: 6,
			fill: GoBuffer.fill,
			stroke: GoBuffer.stroke,
		}),
		fill: GoBuffer.fill,
		stroke: GoBuffer.stroke,
	});

	/**
	 * Checks if a string is valid JSON.
	 * @param {string} str - The string to check.
	 * @returns {boolean} True if the string is valid JSON, otherwise false.
	 */
	public static is_json(str) {
		try {
			JSON.parse(str);
		} catch (e) {
			return false;
		}
		return true;
	}

	/**
	 * Creates an instance of GoBuffer.
	 * @param {bufferParams} paramService - The parameters for the buffer service.
	 * @param {Array<unknown>} dataBuffer - The data to be buffered.
	 * @param {number} distance - The distance for the buffer.
	 * @param {Style | styleFromUser} style - The style for the buffer.
	 * @param {Array<string>} lookingForDataNames - The names of the data to look for.
	 * @param {string} pipeLayerName - The name of the pipe layer.
	 * @param {ISVGIcon} stylePoint - The style for the points.
	 */
	constructor() {
		this.__paramService = {
			bufferData: [],
			distance: 0,
		};

		this.__dataBuffer = [];
		this.__distance = 0;
		this.style = new Style();
		this.lookingForDataNames = [];
		this.pipeLayerName = "";
		this.stylePoint = {};

		this.__bufferLayer = new VectorLayer({
			source: new VectorSource({
				format: new GeoJSON(),
			}),
		});
	}

	public get serviceBuffer() {
		return this.__serviceBuffer;
	}

	public get paramsService() {
		return this.__paramService;
	}

	public get dataBuffer() {
		return this.__dataBuffer;
	}

	public get distance() {
		return this.__distance;
	}

	/**
	 * Initializes the buffer with the given parameters.
	 * @param {string} idBuffer - The ID of the buffer layer.
	 * @param {bufferParams} dataBuffer - The data for the buffer.
	 * @param {styleFromUser} styleFromUser - The style from the user.
	 * @param {GOStagingMap} map - The map instance.
	 * @returns {CancelablePromise<GeoJSON | undefined>} The promise that resolves to the buffer data.
	 */
	public init(
		idBuffer: string,
		dataBuffer: bufferParams,
		styleFromUser: styleFromUser | undefined,
		map: GOStagingMap
	): CancelablePromise<GeoJSON | undefined> {
		return new CancelablePromise((resolve) => {
			const { bufferData, distance } = dataBuffer;

			this.__dataBuffer = bufferData;
			this.__distance = distance;
			this.style = styleFromUser;

			const isJson = GoBuffer.is_json(bufferData);
			if (!isJson) {
				const writer = new GeoJSON();
				const jsonData = writer.writeFeatures([
					bufferData,
				] as unknown as Array<Feature<Geometry>>);
				this.__dataBuffer = JSON.parse(jsonData);
			} else {
				this.__dataBuffer = bufferData;
			}
			if (!map.getLayer(idBuffer)) {
				map.getMap().addLayer(this.__bufferLayer);
				addIdToObj(idBuffer, this.__bufferLayer);
				map.addLayer(idBuffer, this.__bufferLayer);
			} else {
				this.__bufferLayer = map.getLayer(idBuffer);
			}
			if (!this.style) {
				this.__bufferLayer.setStyle(GoBuffer.defaultStyle);
			} else {
				const styleUser = StyleUtils.newStyle(this.style);
				this.__bufferLayer.setStyle(styleUser);
			}
			this.__paramService = {
				bufferData: this.__dataBuffer,
				distance: this.__distance,
			};

			return resolve(this.getBufferService(this.__paramService));
		});
	}

	/**
	 * Get the layer of buffer
	 * @returns {VectorLayer<VectorSource> | undefined} Vector layer of the buffer
	 * @method
	 */
	public getLayerBufferData(): VectorLayer<Feature<Geometry>> {
		return this.__bufferLayer;
	}

	/**
	 * Gets the buffer layer.
	 * @returns {VectorLayer<Feature<Geometry>>} The buffer layer.
	 */
	public get bufferLayer(): VectorLayer<Feature<Geometry>> {
		return this.__bufferLayer;
	}

	/**
	 * Retrieves the buffer service based on the provided parameters.
	 * @private
	 * @param {bufferParams} paramsService - The parameters for the buffer service.
	 * @returns {Promise<GeoJSON | undefined>} The promise that resolves to the buffer data.
	 */
	private async getBufferService(paramsService: bufferParams) {
		const __serviceBuffer = new GoServiceBuffer();

		const bufferSource = this.__bufferLayer.getSource();

		if (!bufferSource) {
			return;
		}

		bufferSource.clear();
		bufferSource.refresh();
		const data = await __serviceBuffer.getBufferService(paramsService);

		if (!data) {
			return this.__serviceBuffer;
		}

		data.features.forEach((element) => {
			element.type = "Feature";
		});
		this.__serviceBuffer = data;
		this.__bufferLayer.setZIndex(99);
		const featuresBuffer = new GeoJSON()
			.readFeatures(this.__serviceBuffer)
			.filter((feat) => feat.getGeometry());
		bufferSource.addFeatures(featuresBuffer);
	}

	/**
	 * Gets the parameters for the buffer layer.
	 * @returns {bufferParams} The parameters of the buffer layer.
	 */
	public getLayerParams() {
		return this.__paramService;
	}
}

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Interactions\index.ts
export * from "./utils"; // Don't remove this line from this position

export * as Analysis from "./Analysis";
export * as ExternalsConnections from "./ExternalsConnections";
export * as Features from "./Features";
export * as Hydraulics from "./Hydraulics";
// export * from "./IFC";
export * as Selection from "./Selection";
export * as Views from "./Views";
export * as Utils from "./utils";
export { GoInteractions };
import { EventEmitter } from "@billjs/event-emitter";
import "@goTypes/index";
import * as types from "@goTypes/index";
import { GOGeoJsonLayer } from "@layers/GoGeoJsonLayer";
import { GOVectorLayer } from "@layers/GoVectorLayer";
import { GOStagingMap } from "@map/GoStagingMap";
import { GoCadastre, GoDMD } from "@services/index";
import {
	GOGeometryType,
	GoFilterCondition,
	HexConditions,
	RegistedApps,
	RegistedClients,
	RegistedEnviroments,
	UpDownType,
	densityToolType,
	unitSystem,
} from "@utils/constants";
import { StyleUtils } from "@utils/index";
import { CancelablePromise } from "cancelable-promise";
import Feature from "ol/Feature";
import OlMap from "ol/Map";
import * as olCoordinate from "ol/coordinate";
import { Extent } from "ol/extent";
import Geometry from "ol/geom/Geometry";
import Polygon from "ol/geom/Polygon";
import DoubleClickZoom from "ol/interaction/DoubleClickZoom";
import VectorLayer from "ol/layer/Vector";
import { transformExtent } from "ol/proj";
import VectorSource from "ol/source/Vector";
import { Style } from "ol/style";
import { ErrorSeverity, goErrorsHandlers } from "../Utils/classes/GoError";
import { mapsErrors } from "../Utils/classes/GoError/constants";
import { mapsErrorsMsg } from "../Utils/classes/GoError/errrosMsg";
import { GoDensityTool, GoIntersect, GoMeasure } from "./Analysis";
import {
	GoGeocoding,
	GoGeocodingHERE,
	GoNexusInfo,
	GoWMSGetFeatureInfo,
} from "./ExternalsConnections";
import { GoRouting } from "./ExternalsConnections/interact/Geocoding";
import { GoEditFeatures, GoHighlight, GoHover } from "./Features";
import { GoHoverFeature } from "./Features/interact/Hover/GoHoverFeature";
import { GoCloseTools, GoflowLinesMove } from "./Hydraulics";
import { GoUpDown } from "./Hydraulics/interact/upDownStream";
import { GoDrawSelectByPolygon, GoSelectByPolygon } from "./Selection";
import { GoOverlay, GoPrint, GoRotation } from "./Views";
import { GoFullScreen } from "./Views/interact/FullScreen";
import MouseEventInteraction from "./Views/interact/Rotation/MouseEventInteraction";
import * as functions from "./functions";
import { GoBuffer } from "./geoTools/buffer";
import { GOInterpolation } from "./geoTools/interpolation";
import { GOIsolines } from "./geoTools/isolines";
import { GoNearData } from "./geoTools/nearData";
import { GoNearDataAttributes } from "./geoTools/nearDataAttributes";
import { GoOpticalFiber } from "./geoTools/opticalFiber";
import { GoPolygonSector } from "./geoTools/polygonSector";
import { GoSectorColor } from "./geoTools/sectorColor";
import { GoWatershed } from "./geoTools/watershed";
import { GOInteractionTool } from "./utils";
import { GoIFCComponents, IFCConfig } from "./IFC";
import { LayerGroupIA } from "./thematicMap/types";
import { GoThematicMap } from "./thematicMap";
import { GoMap } from "@map/GoMap";

/**
 * This class allow to interact with the map.
 * @class GoInteractions
 * @augments GOInteractionTool
 * @constructor
 * @param { Map<string, GOStagingMap>} stagingMap - The map of all layers.
 */
class GoInteractions extends GOInteractionTool {
	/**
	 * Callback for hover interactions.
	 *
	 * @private
	 */
	private callbackHover;
	/**
	 * Map of close tools organized by map ID and interaction ID.
	 *
	 * @private
	 */
	private closeTool: Map<string, Map<string, GoCloseTools>> = new Map<
		string,
		Map<string, GoCloseTools>
	>();
	/**
	 * Map of control scroll zoom settings by map ID.
	 *
	 * @private
	 */
	private ctrlScroollZoom: Map<string, boolean> = new Map();
	/**
	 * Map of density tools organized by map ID and interaction ID.
	 *
	 * @private
	 */
	private densityTool: Map<string, Map<string, GoDensityTool>> = new Map<
		string,
		Map<string, GoDensityTool>
	>();
	/**
	 * Map of DMDs by map ID.
	 *
	 * @private
	 */
	private dmds: Map<string, GoDMD> = new Map<string, GoDMD>();
	/**
	 * Map of Cadastre data by map ID.
	 *
	 * @private
	 */
	private __cadastres: Map<string, GoCadastre> = new Map();
	/**
	 *  Map of draw-select-by-polygon tools organized by map ID and interaction ID.
	 *
	 * @private
	 */
	private drawSelectByPolygon: Map<
		string,
		Map<string, GoDrawSelectByPolygon>
	> = new Map<string, Map<string, GoDrawSelectByPolygon>>();
	/**
	 * Map of draw-select-by-polygon-in-cluster tools organized by map ID and interaction ID.
	 *
	 * @private
	 */
	private drawSelectByPolygonInCluster: Map<
		string,
		Map<string, GoDrawSelectByPolygon>
	> = new Map();
	/**
	 * Map of edit features tools organized by map ID and interaction ID.
	 *
	 * @private
	 */
	private editFeature: Map<string, Map<string, GoEditFeatures>> = new Map<
		string,
		Map<string, GoEditFeatures>
	>();
	/**
	 * Event emitter for handling map events.	 *	 *	 * @private
	 */
	private eventEmitter = new EventEmitter();
	/**
	 * Map of flow lines tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private flowLines: Map<string, Map<string, GoflowLinesMove>> = new Map<
		string,
		Map<string, GoflowLinesMove>
	>();
	/**
	 * Map of full screen tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private fullScreen: Map<string, Map<string, GoFullScreen>> = new Map<
		string,
		Map<string, GoFullScreen>
	>();
	/**
	 * Instance of the geocoding service.	 *	 *	 * @private
	 */
	private geocoding: GoGeocoding = new GoGeocoding();
	/**
	 * Instance of the HERE geocoding service.	 *	 *	 * @private
	 */
	private geocodingHERE: GoGeocodingHERE = new GoGeocodingHERE();
	/**
	 * Map of get feature tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private getFeature: Map<string, Map<string, string>> = new Map<
		string,
		Map<string, string>
	>();

	/**
	 * Map of highlights organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private highlights: Map<string, Map<string, GoHighlight>> = new Map<
		string,
		Map<string, GoHighlight>
	>();
	/**
	 * Map of hover tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private hover: Map<string, Map<string, GoHover>> = new Map<
		string,
		Map<string, GoHover>
	>();
	/**
	 * ID for the hover interaction.	 *	 *	 * @private
	 * */
	private idHover = "";
	/**
	 * ID for the map hover interaction.	 *	 *	 * @private
	 * */
	private idMapHover = "";
	/**
	 * Map of IFC tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	// eslint-disable-next-line
	private ifc: Map<string, Map<string, any>> = new Map<
		string,
		Map<string, any> //eslint-disable-line
	>();
	/**
	 * Flag indicating inclination.	 *	 *	 * @private
	 */
	private inclination = false;
	/**
	 * Map of intersect tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private intersect: Map<string, Map<string, GoIntersect>> = new Map<
		string,
		Map<string, GoIntersect>
	>();
	/**
	 * Map layer for vector data.	 *	 *	 * @private
	 */
	private layerMap: GOVectorLayer | GOGeoJsonLayer | undefined;
	/**
	 * Map layer for vector data.	 *	 *	 * @private
	 */
	private layerOwn: VectorLayer<VectorSource> | undefined;
	/**
	 * Map for hover interactions.	 *	 *	 * @private
	 */
	private mapHover: OlMap | undefined;

	/**
	 * Map of measure tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private measure: Map<string, Map<string, GoMeasure>> = new Map<
		string,
		Map<string, GoMeasure>
	>();
	/**
	 * Instance of the Nexus Info service.	 *	 *	 * @private
	 */
	private nexusInfo: GoNexusInfo = new GoNexusInfo();

	/**
	 * Map of overlays organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private overlays: Map<string, Map<string, GoOverlay>> = new Map<
		string,
		Map<string, GoOverlay>
	>();

	/**
	 * Map of print tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private print: Map<string, Map<string, GoPrint>> = new Map<
		string,
		Map<string, GoPrint>
	>();
	/**
	 * Map of remove zoom organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private removedZoomInteractions: Map<string, DoubleClickZoom> = new Map();
	/**
	 * Map of remove zoom organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private rotation: Map<string, Map<string, GoRotation>> = new Map<
		string,
		Map<string, GoRotation>
	>();
	/**
	 * Map of routing tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private routingHERE: Map<string, Map<string, GoRouting>> = new Map<
		string,
		Map<string, GoRouting>
	>();
	/**
	 * Map of sector color tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private sectorColor: Map<string, Map<string, GoSectorColor>> = new Map<
		string,
		Map<string, GoSectorColor>
	>();
	/**
	 * Map of sector polygon Sector organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private polygonSector: Map<string, Map<string, GoPolygonSector>> = new Map<
		string,
		Map<string, GoPolygonSector>
	>();
	/**
	 * Map of watershed tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private watershed: Map<string, Map<string, GoWatershed>> = new Map<
		string,
		Map<string, GoWatershed>
	>();
	/**
	 * Map of optical fiber tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private opticalFiber: Map<string, Map<string, GoOpticalFiber>> = new Map<
		string,
		Map<string, GoOpticalFiber>
	>();

	/**
	 * Map of interpolation tools organized by map ID and interaction ID.
	 * @type {Map<string, Map<string, GOInterpolation>>}
	 * @private
	 */
	private interpolation: Map<string, Map<string, GOInterpolation>> = new Map<
		string,
		Map<string, GOInterpolation>
	>();

	/**
	 * Map of isolines tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private isolines: Map<string, Map<string, GOIsolines>> = new Map<
		string,
		Map<string, GOIsolines>
	>();
	/**
	 * Map of select by polygon tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private selectByPolygon: Map<string, Map<string, GoSelectByPolygon>> =
		new Map<string, Map<string, GoSelectByPolygon>>();
	/**
	 * Map of staging maps by map ID.	 *	 *	 * @private
	 */
	public stagingMap: Map<string, GOStagingMap>;
	/**
	 * Style for hover interactions from user input.	 *	 *	 * @private
	 */
	private styleHover: types.styleFromUser | undefined;
	/**
	 * Map of tooltips hover features organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private toolTipHover: Map<string, Map<string, GoHoverFeature>> = new Map<
		string,
		Map<string, GoHoverFeature>
	>();
	/**
	 * Map of up down tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private upDown: Map<string, Map<string, GoUpDown>> = new Map<
		string,
		Map<string, GoUpDown>
	>();
	/**
	 * Map of WMS (Web Map Service) GetFeatureInfo tools organized by map ID and interaction ID.	 *	 *	 * @private
	 */
	private wmsGetFeatureInfo: Map<string, Map<string, GoWMSGetFeatureInfo>> =
		new Map<string, Map<string, GoWMSGetFeatureInfo>>();
	/**
	 * Map of buffer tools organized by map ID and interaction ID.
	 *
	 * @type {Map<string, Map<string, GoBuffer>>}
	 * @public
	 */
	public buffer: Map<string, Map<string, GoBuffer>> = new Map<
		string,
		Map<string, GoBuffer>
	>();
	/**
	 * Data for close tool interactions.
	 *
	 * @type {types.CloseTool}
	 * @public
	 */
	public closeToolData: types.CloseTool = {
		active: false,
		dataPipes: [],
		dataValves: [],
		filterLocationName: "",
	};
	/**
	 * Data for intersect tools.
	 *
	 * @type {types.dataTools}
	 * @public
	 */
	public intersectData: types.dataTools;
	/**
	 * Map layer for hover interactions.
	 *
	 * @type {Map<string, Map<string, GoLayerColor>>}
	 * @public
	 */
	public layerHover: VectorLayer<VectorSource> | undefined;
	/**
	 * Map of near data tools organized by map ID and interaction ID.
	 *
	 * @type {Map<string, Map<string, GoNearData>>}
	 * @public
	 */
	public nearData: Map<string, Map<string, GoNearData>> = new Map<
		string,
		Map<string, GoNearData>
	>();
	/**
	 * Map of near data attributes tools organized by map ID and interaction ID.
	 *
	 * @type {Map<string, Map<string, GoNearDataAttributes>>}
	 * @public
	 */
	public nearDataAttributes: Map<string, Map<string, GoNearDataAttributes>> =
		new Map<string, Map<string, GoNearDataAttributes>>();
	/**
	 * Data for up-down tools.
	 *
	 * @type {types.UpDown}
	 * @public
	 */
	public upDownToolData: types.UpDown = {
		active: false,
		dataPipes: [],
		dataWells: [],
		filterLocationName: "",
	};
	/**
	 * Map of thematic maps organized by map ID and thematic map ID.
	 *
	 * @type {Map<string, Map<string, GoThematicMap>>}
	 * @public
	 */
	public thematicMap: Map<string, Map<string, GoThematicMap>> = new Map<
		string,
		Map<string, GoThematicMap>
	>();

	/**
	 * Creates an instance of GoInteractions.
	 *
	 * @param {Map<string, GOStagingMap>} stagingMap - The map of all layers.
	 */
	constructor(stagingMap: Map<string, GOStagingMap>) {
		super();
		this.stagingMap = stagingMap;
	}

	public getIFCRegistry(): Map<string, Map<string, GoIFCComponents>> {
		return this.ifc;
	}

	public unmount(): void {
		this.stagingMap.forEach((map) => {
			map.getMap().removeInteraction(this.mapHover as any);
			this.mapHover = undefined;
		});
		this.stagingMap.clear();
		this.dmds.clear();
		this.__cadastres.clear();
		this.intersect.clear();
		this.drawSelectByPolygon.clear();
		this.drawSelectByPolygonInCluster.clear();
		this.editFeature.clear();
		this.overlays.clear();
		this.measure.clear();
		this.sectorColor.clear();
		this.polygonSector.clear();
		this.nearData.clear();
		this.nearDataAttributes.clear();
		this.watershed.clear();
		this.opticalFiber.clear();
		this.interpolation.clear();
		this.isolines.clear();
	}

	// Others methods -----------------------------------------------------------
	/**
	 * This function allow to transform to Extent coordinates.
	 * @param {import('types').Extent} extent - The extent.
	 * @param {string} layer_projection_origin - The layer projection origin.
	 * @param {string} layer_projection_destiny - The layer projection destiny.
	 * @returns {import('types').Extent} Return the extent transformed.
	 * @method
	 */
	public transfromExtentCoordinates(
		extent: Extent,
		layer_projection_origin: string,
		layer_projection_destiny: string
	): Extent {
		return transformExtent(
			extent,
			"EPSG:" + layer_projection_origin,
			"EPSG:" + layer_projection_destiny
		);
	}

	/**
	 * This function allow to create the styles for the layers
	 * @param {import('types').styleFromUser} style - The style that you want to create
	 * @returns {Style} Return the style created
	 * @method
	 */
	public createStyle(style: types.styleFromUser): Style {
		return StyleUtils.editStyle(style);
	}

	private mapExists(idMap: string, errorMethod: string): boolean {
		if (!this.stagingMap.has(idMap)) {
			// Error Handler
			goErrorsHandlers.mapsErrorHandler.set(
				ErrorSeverity.Error,
				mapsErrors.MAPS_ERROR_MAP_DONT_EXIST,
				`Map ID ${idMap}. Error on "${errorMethod}":` +
					mapsErrorsMsg.MAPS_ERROR_MAP_DONT_EXIST
			);
			return false;
		}
		return true;
	}

	private interactionHasMap(
		idMap: string,
		errorMethod: string,
		interaction: Map<string, any>
	): boolean {
		if (!interaction.has(idMap)) {
			// Error Handler
			goErrorsHandlers.mapsErrorHandler.set(
				ErrorSeverity.Error,
				mapsErrors.MAPS_ERROR_MAP_DONT_EXIST,
				`Map ID ${idMap}. Error on "${errorMethod}":` +
					mapsErrorsMsg.MAPS_ERROR_MAP_DONT_EXIST
			);
			return false;
		}
		return true;
	}

	private interactionExists(
		idMap: string,
		errorMethod: string,
		idInteraction: string,
		interaction: Map<string, any>
	): boolean {
		if (!interaction.get(idMap).has(idInteraction)) {
			// Error Handler
			goErrorsHandlers.mapsErrorHandler.set(
				ErrorSeverity.Error,
				mapsErrors.MAPS_ERROR_INTERACTION_DONT_EXIST,
				`Map ID ${idMap}. Error on "${errorMethod}":` +
					mapsErrorsMsg.MAPS_ERROR_INTERACTION_DONT_EXIST
			);
			return false;
		}
		return true;
	}

	public getOlMap(idMap: string): OlMap | undefined {
		return this.stagingMap.get(idMap)?.getMap();
	}
}

interface GoInteractions {
	// Routing methods ----------------------------------------------------------
	addRoutingHERE({
		idMap,
		routingID,
		transportMode,
		points,
		lang,
	}: {
		idMap: string;
		routingID: string;
		transportMode: string;
		points: types.geodesicCoordinates[];
		lang?: string;
	}): Promise<any>;
	addMultiRoutingHERE({
		idMap,
		routingID,
		transportMode,
		points,
		stops,
		lang,
		order_by_distance,
	}: {
		idMap: string;
		routingID: string;
		transportMode: string;
		points: Array<types.geodesicCoordinates>;
		stops: Array<types.geodesicCoordinates>;
		lang?: string;
		order_by_distance?: boolean;
	}): Promise<any>;
	// Geocoding methods --------------------------------------------------------
	registerGeocodingHERE({
		environmentID,
		appID,
		clientID,
		apiKey,
	}: {
		environmentID: RegistedEnviroments;
		appID: RegistedApps;
		clientID: RegistedClients;
		apiKey?: string;
	}): Promise<void>;
	getDirectionFromCoordinates({
		lat,
		lng,
	}: {
		lat: number;
		lng: number;
	}): Promise<unknown>;
	// eslint-disable-next-line @typescript-eslint/no-explicit-any
	getDirectionFromCoordinatesProperties({
		lat,
		lng,
	}: {
		lat: number;
		lng: number;
	}): Promise<any>;
	getDirectionFromCoordinatesHERE({
		lat,
		lng,
	}: {
		lat: number;
		lng: number;
	}): Promise<unknown>;
	getCoordinatesFromDirection({
		direction,
	}: {
		direction: string;
	}): Promise<unknown>;
	// eslint-disable-next-line @typescript-eslint/no-explicit-any
	getCoordinatesFromDirectionProperties({
		direction,
	}: {
		direction: string;
	}): Promise<any>;
	getCoordinatesFromDirectionHERE({
		direction,
	}: {
		direction: string;
	}): Promise<any>;
	suggestAddress({
		params,
	}: {
		params: types.HERESuggestParams;
	}): Promise<any>;
	autoSuggestAddress({
		params,
	}: {
		params: types.HERESuggestParams;
	}): Promise<unknown>;
	// Nexus info methods ------------------------------------------------------
	getAllsensorsFromNexus({
		location,
		token,
		bindingUrl,
	}: {
		location: string;
		token: string;
		bindingUrl: string;
	}): Promise<unknown>;
	getHistoricInfoFromNexus({
		sensorUID,
		token,
		bindingUrl,
	}: {
		sensorUID: string;
		token: string;
		bindingUrl: string;
	}): Promise<unknown>;
	// Overlay methods ---------------------------------------------------------
	addOverlay({
		idMap,
		idOverlay,
		overlayData,
	}: {
		idMap: string;
		idOverlay: string;
		overlayData: types.overlayData;
	}): GoOverlay;
	hasOverlay({
		idMap,
		idOverlay,
	}: {
		idMap: string;
		idOverlay: string;
	}): boolean;
	removeOverlay({
		idMap,
		idOverlay,
	}: {
		idMap: string;
		idOverlay: string;
	}): void;
	// GetFeaturesByLayer methods -----------------------------------------------
	getFeaturesByLayer(
		params: types.GetFeaturesByLayerParams
	): Promise<types.GeoJSONFeatureCollection>;
	// GetProperties methods ----------------------------------------------------
	getAllFeaturesProperties({
		idMap,
		layerList,
	}: {
		idMap: string;
		layerList: types.allFeaturesProperties;
	}): Array<unknown>;
	// Highlight methods -------------------------------------------------------
	addHighlight({
		idMap,
		idHighlight,
		features,
		style,
		textFunction,
		styleSVG,
		zIndex,
	}: {
		idMap: string;
		idHighlight: string;
		features: string | Feature[];
		style?: types.styleFromUser;
		textFunction?: (...rest: unknown[]) => unknown;
		styleSVG?: types.ISVGIcon;
		zIndex?: number;
	}): GoHighlight;
	hasHighlight({
		idMap,
		idHighlight,
	}: {
		idMap: string;
		idHighlight: string;
	}): boolean;
	getHighlight({
		idMap,
		idHighlight,
	}: {
		idMap: string;
		idHighlight: string;
	}): GoHighlight;
	removeHighlight({
		idMap,
		idHighlight,
	}: {
		idMap: string;
		idHighlight: string;
	}): void;
	removeHighlightIfExist({
		idMap,
		idHighlight,
	}: {
		idMap: string;
		idHighlight: string;
	}): void;
	changeStyleOfHighlight({
		idMap,
		idHighlight,
		userStyle,
		styleSVG,
	}: {
		idMap: string;
		idHighlight: string;
		userStyle?: types.styleFromUser;
		styleSVG?: types.ISVGIcon;
	}): Promise<GoHighlight | string>;
	changeHighlightZIndex({
		idMap,
		idHighlight,
		zIndex,
	}: {
		idMap: string;
		idHighlight: string;
		zIndex: number;
	}): void;
	restoreHighlightStyles({
		idMap,
		idHighlight,
	}: {
		idMap: string;
		idHighlight: string;
	}): void;
	// WMS GetFeatureInfo methods -----------------------------------------------
	addWMSGetFeatureInfo({
		idMap,
		idWMSGetFeatureInfo,
		layers,
		coordinates,
	}: {
		idMap: string;
		idWMSGetFeatureInfo: string;
		layers: string[];
		coordinates: olCoordinate.Coordinate;
	}): Promise<GoWMSGetFeatureInfo>;
	hasWMSGetFeatureInfo({
		idMap,
		idWMSGetFeatureInfo,
	}: {
		idMap: string;
		idWMSGetFeatureInfo: string;
	}): boolean;
	getWMSGetFeatureInfo({
		idMap,
		idWMSGetFeatureInfo,
	}: {
		idMap: string;
		idWMSGetFeatureInfo: string;
	}): GoWMSGetFeatureInfo;
	removeWMSGetFeatureInfo({
		idMap,
		idWMSGetFeatureInfo,
	}: {
		idMap: string;
		idWMSGetFeatureInfo: string;
	}): void;
	// Hover Feature methods ---------------------------------------------------
	hasHoverFeature({
		idMap,
		idHover,
	}: {
		idMap: string;
		idHover: string;
	}): boolean;
	getLayerHover({
		idMap,
		idHover,
	}: {
		idMap: string;
		idHover: string;
	}): GoHover | undefined;
	mouseHover({
		idMap,
		idHover,
		layer,
		style,
		callback,
		styleSVG,
		zindex,
	}: {
		idMap: string;
		idHover: string;
		layer?: string;
		style?: types.styleFromUser;
		callback?: (feature: Feature<Geometry>) => void;
		styleSVG?: types.ISVGIcon;
		zindex?: number;
	}): GoHover | void;
	removeHover({ idMap, idHover }: { idMap: string; idHover: string }): void;
	hasGetFeature(idMap: string): boolean;
	addTooltipHoverFeature({
		idMap,
		idHover,
		featuresGeom,
		style,
	}: {
		idMap: string;
		idHover: string;
		featuresGeom: Feature<Geometry>;
		style?: types.styleFromUser;
	}): void;
	hasToolTipHover({
		idMap,
		idHover,
	}: {
		idMap: string;
		idHover: string;
	}): boolean;
	getTooltipHoverFeature({
		idMap,
		idHover,
	}: {
		idMap: string;
		idHover: string;
	}): GoHoverFeature;
	removeTooltipHoverFeature({
		idMap,
		idHover,
	}: {
		idMap: string;
		idHover: string;
	}): void;
	changeHoverZIndex({
		idMap,
		idHover,
		zIndex,
	}: {
		idMap: string;
		idHover: string;
		zIndex: number;
	}): void;
	// Close Tool methods ------------------------------------------------------
	refreshGeoTools({ idMap }: { idMap: string }): CancelablePromise<void>;
	addCloseTool({
		idMap,
		idCloseTool,
		userStyleValve,
		userStylePipes,
	}: {
		idMap: string;
		idCloseTool: string;
		userStyleValve?: types.styleFromUser;
		userStylePipes?: types.styleFromUser;
	}): Promise<unknown>;
	clearCloseTool({
		idMap,
		idCloseTool,
	}: {
		idMap: string;
		idCloseTool: string;
	}): void;
	addExtraDataCloseToolInteractions({ data }: { data: types.CloseTool });
	addExtraDataUpDownStreamInteractions({
		data,
	}: {
		data: types.UpDown;
	}): void;
	hasCloseTool({
		idMap,
		idCloseTool,
	}: {
		idMap: string;
		idCloseTool: string;
	}): boolean;
	removeCloseTool({
		idMap,
		idCloseTool,
	}: {
		idMap: string;
		idCloseTool: string;
	}): void;
	getCloseTool({
		idMap,
		idCloseTool,
	}: {
		idMap: string;
		idCloseTool: string;
	}): GoCloseTools;
	//UpDownStream ------------------------------------------------------
	addUpDownStream({
		idMap,
		idUpDown,
		type,
		userStyleWell,
		userStyleLastWell,
		userStylePipes,
		municipalityId,
	}: {
		idMap: string;
		idUpDown: string;
		type: UpDownType;
		userStyleWell?: types.styleFromUser;
		userStyleLastWell?: types.styleFromUser;
		userStylePipes?: types.styleFromUser;
		municipalityId?: string | number;
	}): Promise<unknown>;
	hasUpDownStream({
		idMap,
		idUpDown,
	}: {
		idMap: string;
		idUpDown: string;
	}): boolean;
	removeUpDownStream({
		idMap,
		idUpDown,
	}: {
		idMap: string;
		idUpDown: string;
	}): void;
	clearUpDownStream({
		idMap,
		idUpDown,
	}: {
		idMap: string;
		idUpDown: string;
	}): void;
	// Intersect Tools methods -------------------------------------------------------
	addIntersect({
		idMap,
		idIntersect,
		intersectDataIn,
		intersectDataClip,
		style,
		municipalityField,
		municipalityId,
	}: {
		idMap: string;
		idIntersect: string;
		intersectDataIn: types.dataTools;
		intersectDataClip: types.dataTools;
		style: types.styleFromUser;
		municipalityField?: string;
		municipalityId?: string | number;
	}): CancelablePromise<unknown>;
	clearIntersect({
		idMap,
		idIntersect,
	}: {
		idMap: string;
		idIntersect: string;
	}): void;
	hasIntersect({
		idMap,
		idIntersect,
	}: {
		idMap: string;
		idIntersect: string;
	}): boolean;
	removeIntersect({
		idMap,
		idIntersect,
	}: {
		idMap: string;
		idIntersect: string;
	}): void;
	// Near Data Tools methods -------------------------------------------------------
	addNearData({
		idMap,
		idNearData,
		pointCoordinate,
		lookingForData,
		stylePoint,
		style,
		municipalityField,
		municipalityId,
	}: {
		idMap: string;
		idNearData: string;
		pointCoordinate: [number, number];
		lookingForData: types.dataTools;
		stylePoint?: types.ISVGIcon;
		style: types.styleFromUser;
		municipalityField?: string;
		municipalityId?: string | number;
	});
	hasNearData({
		idMap,
		idNearData,
	}: {
		idMap: string;
		idNearData: string;
	}): boolean;
	removeNearData({
		idMap,
		idNearData,
	}: {
		idMap: string;
		idNearData: string;
	}): void;
	//Near Data Attributes Tools methods -------------------------------------------------------
	addNearDataAttributes({
		idMap,
		idInteraction,
		layer,
		coordinates,
		attribute,
		condition,
		value,
		stylePoint,
		style,
		municipalityField,
		municipalityId,
	}: {
		idMap: string;
		idInteraction: string;
		layer: types.dataTools;
		coordinates: [number, number];
		attribute: string;
		condition: GoFilterCondition;
		value: any; // eslint-disable-line
		stylePoint?: types.ISVGIcon;
		style?: types.styleFromUser;
		municipalityField?: string;
		municipalityId?: string | number;
	});
	hasNearDataAttributes({
		idMap,
		idInteraction,
	}: {
		idMap: string;
		idInteraction: string;
	}): boolean;
	removeNearDataAttributes({
		idMap,
		idInteraction,
	}: {
		idMap: string;
		idInteraction: string;
	}): void;
	//Buffer Tools methods ---------------------------------------------
	addBuffer({
		idMap,
		idBuffer,
		bufferData,
		style,
	}: {
		idMap: string;
		idBuffer: string;
		bufferData: types.bufferParams;
		style?: types.styleFromUser;
	}): Promise<unknown>;
	hasBuffer({
		idMap,
		idBuffer,
	}: {
		idMap: string;
		idBuffer: string;
	}): boolean;
	removeBuffer({
		idMap,
		idBuffer,
	}: {
		idMap: string;
		idBuffer: string;
	}): void;
	//SectorColor Tools methods ---------------------------------------------
	addSectorColor({
		idMap,
		sectorColorId,
		sectorColorData,
		sectorColorParams,
		municipalityField,
		municipalityId,
	}: {
		idMap: string;
		sectorColorId: string;
		sectorColorData: types.dataTools;
		sectorColorParams: types.sectorColorParams;
		municipalityField?: string;
		municipalityId?: string | number;
	}): CancelablePromise<unknown>;
	hasSectorColor({
		idMap,
		idSectorColor,
	}: {
		idMap: string;
		idSectorColor: string;
	}): boolean;
	removeSectorColor({
		idMap,
		idSectorColor,
	}: {
		idMap: string;
		idSectorColor: string;
	}): void;
	//PolygonSector Tools methods ---------------------------------------------
	addPolygonSector({
		params,
	}: {
		params: types.polygonSectorParams;
	}): CancelablePromise<unknown>;
	hasPolygonSector({
		idMap,
		idPolygonSector,
	}: {
		idMap: string;
		idPolygonSector: string;
	}): boolean;
	removePolygonSector({
		idMap,
		idPolygonSector,
	}: {
		idMap: string;
		idPolygonSector: string;
	}): void;
	//OpticalFiber Tools methods ---------------------------------------------
	addOpticalFiber({ params }: { params: types.opticalFiberParams });
	hasOpticalFiber({
		idMap,
		idOpticalFiber,
	}: {
		idMap: string;
		idOpticalFiber: string;
	}): boolean;
	removeOpticalFiber({
		idMap,
		idOpticalFiber,
	}: {
		idMap: string;
		idOpticalFiber: string;
	}): void;
	//Interpolation Tools methods ---------------------------------------------
	addInterpolation({
		params,
	}: {
		params: types.interpolationParams;
	}): CancelablePromise<unknown>;
	hasInterpolation({
		idMap,
		idInterpolation,
	}: {
		idMap: string;
		idInterpolation: string;
	}): boolean;
	removeInterpolation({
		idMap,
		idInterpolation,
	}: {
		idMap: string;
		idInterpolation: string;
	}): void;
	//Isolines Tools methods ---------------------------------------------
	addIsolines({
		params,
	}: {
		params: types.isolinesParams;
	}): CancelablePromise<unknown>;
	hasIsolines({
		idMap,
		idIsolines,
	}: {
		idMap: string;
		idIsolines: string;
	}): boolean;
	removeIsolines({
		idMap,
		idIsolines,
	}: {
		idMap: string;
		idIsolines: string;
	}): void;
	//Watershed Tools methods ---------------------------------------------
	addWatershed({ params }: { params: types.watershedParams });
	hasWatershed({
		idMap,
		idWatershed,
	}: {
		idMap: string;
		idWatershed: string;
	}): boolean;
	removeWatershed({
		idMap,
		idWatershed,
	}: {
		idMap: string;
		idWatershed: string;
	}): void;
	// Select By Polygon methods --------------------------------------------
	addDrawSelectByPolygon({
		idMap,
		idDrawSelectByPolygon,
		callback,
		layers,
		style,
		clickFinish,
	}: {
		idMap: string;
		idDrawSelectByPolygon: string;
		callback: (feature: Feature) => void;
		layers?: string[];
		style?: types.styleFromUser;
		clickFinish?: boolean;
	}): unknown;
	hasDrawSelectByPolygon({
		idMap,
		idDrawSelectByPolygon,
	}: {
		idMap: string;
		idDrawSelectByPolygon: string;
	}): boolean;
	setActiveDrawSelectByPolygon({
		idMap,
		idDrawSelectByPolygon,
		active,
	}: {
		idMap: string;
		idDrawSelectByPolygon: string;
		active: boolean;
	}): void;
	removeDrawSelectByPolygon({
		idMap,
		idDrawSelectByPolygon,
	}: {
		idMap: string;
		idDrawSelectByPolygon: string;
	}): void;
	addSelectByPolygon({
		idMap,
		idSelectByPolygon,
		geometryPolygon,
		callback,
		layers,
	}: {
		idMap: string;
		idSelectByPolygon: string;
		geometryPolygon: Feature<Polygon>;
		callback: (feature: Feature) => void;
		layers?: string[];
	}): void;
	hasSelectByPolygon({
		idMap,
		idSelectByPolygon,
	}: {
		idMap: string;
		idSelectByPolygon: string;
	}): boolean;
	removeSelectByPolygon({
		idMap,
		idSelectByPolygon,
	}: {
		idMap: string;
		idSelectByPolygon: string;
	}): void;
	// Select by polygon in Cluster
	addDrawSelectByPolygonInCluster({
		idMap,
		idDrawSelectByPolygonInCluster,
		callback,
		layers,
		style,
		clickFinish,
	}: {
		idMap: string;
		idDrawSelectByPolygonInCluster: string;
		callback: (feature: Feature) => void;
		layers?: string[];
		style?: types.styleFromUser;
		clickFinish?: boolean;
	}): unknown;
	hasDrawSelectByPolygonInCluster({
		idMap,
		idDrawSelectByPolygonInCluster,
	}: {
		idMap: string;
		idDrawSelectByPolygonInCluster: string;
	}): boolean;
	setActiveDrawSelectByPolygonInCluster({
		idMap,
		idDrawSelectByPolygonInCluster,
		active,
	}: {
		idMap: string;
		idDrawSelectByPolygonInCluster: string;
		active: boolean;
	}): void;
	removeDrawSelectByPolygonInCluster({
		idMap,
		idDrawSelectByPolygonInCluster,
	}: {
		idMap: string;
		idDrawSelectByPolygonInCluster: string;
	});
	addSelectByPolygonInCluster({
		idMap,
		idSelectByPolygonInCluster,
		geometryPolygon,
		callback,
		layers,
	}: {
		idMap: string;
		idSelectByPolygonInCluster: string;
		geometryPolygon: Feature<Polygon>;
		callback: (feature: Feature) => void;
		layers?: string[];
	}): void;
	hasSelectByPolygonInCluster({
		idMap,
		idSelectByPolygonInCluster,
	}: {
		idMap: string;
		idSelectByPolygonInCluster: string;
	}): boolean;
	removeSelectByPolygonInCluster({
		idMap,
		idSelectByPolygonInCluster,
	}: {
		idMap: string;
		idSelectByPolygonInCluster: string;
	}): void;
	// Rotation methods -------------------------------------------------------
	hasRotation({
		idMap,
		idRotation,
	}: {
		idMap: string;
		idRotation: string;
	}): boolean;
	removeRotation({
		idMap,
		idRotation,
	}: {
		idMap: string;
		idRotation: string;
	}): void;
	addRotation({
		idMap,
		idRotation,
	}: {
		idMap: string;
		idRotation: string;
	}): void;
	disableRotation({ idMap }: { idMap: string }): void;
	getRotation({
		idMap,
		idRotation,
	}: {
		idMap: string;
		idRotation: string;
	}): GoRotation;
	addMouseRotation({ olMap }: { olMap: OlMap }): MouseEventInteraction;
	addCustomRotateCompass({ idMap }: { idMap: string }): HTMLElement;
	// Print methods -------------------------------------------------------
	hasPrint({ idMap, idPrint }: { idMap: string; idPrint: string }): boolean;
	removePrint({ idMap, idPrint }: { idMap: string; idPrint: string }): void;
	addPrint({
		idMap,
		idPrint,
		mapTitle,
		name,
		author,
		date,
	}: {
		idMap: string;
		idPrint: string;
		mapTitle?: string;
		name?: string;
		author?: string;
		date?: string;
	}): void;
	getPrint({ idMap, idPrint }: { idMap: string; idPrint: string }): GoPrint;
	// Measure methods ---------------------------------------------------------
	addMeasure({
		idMap,
		idMeasure,
		geometryType,
		callbackStart,
		callbackEnd,
		style,
		defaultTooltip,
		unitSystemSelected,
	}: {
		idMap: string;
		idMeasure: string;
		geometryType: GOGeometryType;
		callbackStart: (...rest: unknown[]) => unknown;
		callbackEnd: (...rest: unknown[]) => unknown;
		style?: types.styleFromUser;
		defaultTooltip?: boolean;
		unitSystemSelected?: unitSystem;
	}): GoMeasure | undefined;
	hasMeasure({
		idMap,
		idMeasure,
	}: {
		idMap: string;
		idMeasure: string;
	}): boolean;
	setActiveMeasure({
		idMap,
		idMeasure,
		active,
	}: {
		idMap: string;
		idMeasure: string;
		active: boolean;
	}): void;
	clearMeasure({
		idMap,
		idMeasure,
	}: {
		idMap: string;
		idMeasure: string;
	}): void;
	removeMeasure({
		idMap,
		idMeasure,
	}: {
		idMap: string;
		idMeasure: string;
	}): void;
	// Flow Lines methods -------------------------------------------------------
	addFlowLines({
		idMap,
		idFlowLines,
		idLayer,
	}: {
		idMap: string;
		idFlowLines: string;
		idLayer: string;
	}): GoflowLinesMove;
	hasFlowLines({
		idMap,
		idFlowLines,
	}: {
		idMap: string;
		idFlowLines: string;
	}): boolean;
	removeFlowLines({
		idMap,
		idFlowLines,
	}: {
		idMap: string;
		idFlowLines: string;
	}): void;
	// Density Layer methods ---------------------------------------------------
	addDensityLayer({
		idMap,
		idDensityTool,
		type,
		layer,
		field,
		breaks,
		hexCase,
		resize,
		hexStyleColors,
	}: {
		idMap: string;
		idDensityTool: string;
		type: densityToolType;
		layer?: string;
		field?: string;
		breaks?: Array<number>;
		hexCase?: HexConditions;
		resize?: boolean;
		hexStyleColors?: types.hexStyleColors;
	}): GoDensityTool;
	removeDensityLayer({
		idMap,
		idDensityTool,
	}: {
		idMap: string;
		idDensityTool: string;
	}): void;
	hasDensityLayer({
		idMap,
		idDensityTool,
	}: {
		idMap: string;
		idDensityTool: string;
	}): boolean;
	getDensityLayer({
		idMap,
		idDensityTool,
	}: {
		idMap: string;
		idDensityTool: string;
	}): GoDensityTool | false;
	// Feature methods ---------------------------------------------------------
	hasEditFeature({
		idMap,
		idEditFeature,
	}: {
		idMap: string;
		idEditFeature: string;
	}): boolean;
	createFeature({ feature }: { feature: Feature }): void;
	removeEditFeature({
		idMap,
		idEditFeature,
	}: {
		idMap: string;
		idEditFeature: string;
	}): void;
	addEditFeature({
		idEditFeature,
		idMap,
		idLayer,
	}: {
		idEditFeature: string;
		idMap: string;
		idLayer: string;
	}): void;
	getEditFeatures({
		idMap,
		idEditFeature,
	}: {
		idMap: string;
		idEditFeature: string;
	}): GoEditFeatures;
	saveEditFeatures({
		idEditFeature,
		idMap,
		idLayer,
		user,
		pass,
	}: {
		idEditFeature: string;
		idMap: string;
		idLayer: string;
		user: string;
		pass: string;
	}): void;
	deleteFeatures({
		idMap,
		idLayer,
		feature,
		user,
		pass,
	}: {
		idMap: string;
		idLayer: string;
		feature: Feature[];
		user: string;
		pass: string;
	}): void;
	// Custom Zoom methods ------------------------------------------------------
	addCustomZoom({ idMap }: { idMap: string }): HTMLElement | undefined;
	removeCustomZoom: ({ idMap }: { idMap: string }) => void;
	removeAllCustomZoom: ({ idMap }: { idMap: string }) => void;
	// Custom Zoom CtrlScroll  ------------------------------------------------------
	addCtrlScrollZoom({ idMap }: { idMap: string }): void;
	hasCtrlScrollZoom({ idMap }: { idMap: string }): boolean;
	removeCtrlScrollZoom({ idMap }: { idMap: string }): void;
	// Zoom interaction  ------------------------------------------------------
	disableDoubleClickZoom({ idMap }: { idMap: string }): void;
	enableDoubleClickZoom({ idMap }: { idMap: string }): void;
	hasDisabledDoubleClickZoom({ idMap }: { idMap: string }): DoubleClickZoom;
	//IFC ---------------------------------------------
	createIFC({ props }: { props: IFCConfig }): Promise<GoIFCComponents>;
	hasIFCModel({
		idMap,
		idIFCModel,
	}: {
		idMap: string;
		idIFCModel: string;
	}): boolean;
	removeIFCModel({
		idMap,
		idIFCModel,
	}: {
		idMap: string;
		idIFCModel: string;
	}): void;
	// DMD ---------------------------------------------
	createDMD({ dmdId }: { dmdId: string }): GoDMD;
	getDMD({ dmdId }: { dmdId: string }): GoDMD;
	getDMDs(): Array<{ name: string; value: GoDMD }>;
	hasDMD({ dmdId }: { dmdId: string }): boolean;
	removeDMD({ dmdId }: { dmdId: string }): void;
	// Multi Extend ---------------------------------------------
	getMultiExtent({
		idMap,
		idLayers,
	}: {
		idMap: string;
		idLayers?: Array<string>;
	}): null | [number, number, number, number];
	// Event listeners
	on(type: string, callback: (...rest: unknown[]) => unknown): void;
	off(type: string, callback: (...rest: unknown[]) => unknown): void;
	once(type: string, callback: (...rest: unknown[]) => unknown): void;
	hasEvent(type: string): boolean;
	getEventHandlers(type: string): Array<(...rest: unknown[]) => unknown>;
	emit(type: string, ...rest: unknown[]): void;
	// FullScreen methods -------------------------------------------------------
	hasFullScreen({ idMap }: { idMap: string }): boolean;
	removeFullScreen({
		idMap,
		idFullScreen,
	}: {
		idMap: string;
		idFullScreen: string;
	}): void;
	addFullScreen({
		idMap,
		customClass,
	}: {
		idMap: string;
		customClass?: string;
	}): GoFullScreen;
	getFullScreen({ idMap }: { idMap: string }): GoFullScreen;
	onFullScreenEnter({ idMap, callBack }: { idMap: string; callBack }): void;
	onFullScreenExit({ idMap, callBack }: { idMap: string; callBack }): void;
	// Cadastre
	createCadastre({
		cadastreId,
		mapId,
	}: {
		cadastreId: string;
		mapId: string;
	}): GoCadastre;
	getCadastre({ cadastreId }: { cadastreId: string }): GoCadastre;
	getCadastres(): {
		name: string;
		value: GoDMD;
	}[];
	hasCadastre({ cadastreId }: { cadastreId: string }): boolean;
	removeCadastre({ cadastreId }: { cadastreId: string }): void;
	//thematicMap methods -----------------------------------------------
	addThematicMap({
		goMap,
		mapId,
		thematicMapId,
		layerGroupIA,
	}: {
		goMap: GoMap;
		mapId: string;
		thematicMapId: string;
		layerGroupIA: LayerGroupIA;
	}): void;
	hasThematicMap({
		idMap,
		thematicMapId,
	}: {
		idMap: string;
		thematicMapId: string;
	}): boolean;
	getThematicMap({
		idMap,
		thematicMapId,
	}: {
		idMap: string;
		thematicMapId: string;
	}): LayerGroupIA;
	removeThematicMap({
		idMap,
		thematicMapId,
	}: {
		idMap: string;
		thematicMapId: string;
	}): void;
}
// Geocoding methods --------------------------------------------------------
/**
 * This function allow to register the geocoding service.
 * @param {Object} params
 * @param {types.HEREParams} params.params - The environment, appID, clientID and APIKey.
 * @returns {CancelablePromise<void>} Return an empty promise.
 * @method
 */
GoInteractions.prototype.registerGeocodingHERE =
	functions.geocoding.registerGeocodingHERE;

/**
 * This function allow to get the direction from the coordinates.
 * @param {Object} params
 * @param {number} params.lat - The latitude of the point.
 * @param {number} params.lng - The longitude of the point.
 * @returns {CancelablePromise<any>} Return promise with the direction.
 * @method
 */
GoInteractions.prototype.getDirectionFromCoordinates =
	functions.geocoding.getDirectionFromCoordinates;

/**
 * This function allow to get the direction from the coordinates.
 * @param {Object} params
 * @param {number} params.lat - The latitude of the point.
 * @param {number} params.lng - The longitude of the point.
 * @returns {CancelablePromise<any>} Return promise with the direction.
 * @method
 */
GoInteractions.prototype.getDirectionFromCoordinatesProperties =
	functions.geocoding.getDirectionFromCoordinatesProperties;

/**
 * This function allow to get the direction from the coordinates.
 * @param {Object} params
 * @param {number} params.lat - The latitude of the point.
 * @param {number} params.lng - The longitude of the point.
 * @returns {CancelablePromise<any>} Return promise with the direction.
 * @method
 */
GoInteractions.prototype.getDirectionFromCoordinatesHERE =
	functions.geocoding.getDirectionFromCoordinatesHERE;

/**
 * This function allow to get the coordinates from the address.
 * @param {Object} params
 * @param {string} params.direction - The direction of the point.
 * @returns {any[]} Return promise with the coordinates.
 * @method
 */
GoInteractions.prototype.getCoordinatesFromDirection =
	functions.geocoding.getCoordinatesFromDirection;

/**
 * This function allow to get the coordinates from the address.
 * @param {Object} params
 * @param {string} params.direction - The direction of the point.
 * @returns {CancelablePromise<any>} Return promise with the coordinates.
 * @method
 */
GoInteractions.prototype.getCoordinatesFromDirectionProperties =
	functions.geocoding.getCoordinatesFromDirectionProperties;

/**
 * This function allow to get the coordinates from the address.
 * @param {Object} params
 * @param {string} params.direction - The direction of the point.
 * @returns {any[]} Return promise with the coordinates.
 * @method
 */
GoInteractions.prototype.getCoordinatesFromDirectionHERE =
	functions.geocoding.getCoordinatesFromDirectionHERE;

/**
 * This function suggest address while typing in the box. if an error exist, return {lat:0, lng:0}
 * @param {Object} params
 * @param {string} params.textToSearch - The string with the direction to get the coordinates.
 * @return  {CancelablePromise<any>}
 * @method
 */
GoInteractions.prototype.suggestAddress = functions.geocoding.suggestAddress;

/**
 * This function auto suggest address while typing in the box. if an error exist, return {lat:0, lng:0}
 * @param {Object} params
 * @param {string} params.textToSearch - The string with the direction to get the coordinates.
 * @return  {CancelablePromise<any>}
 * @method
 */
GoInteractions.prototype.autoSuggestAddress =
	functions.geocoding.autoSuggestAddress;
// Nexus info methods ------------------------------------------------------
/**
 * This function allow to get all the sensors from nexus.
 * @param {Object} params
 * @param {string} params.location - The location of the point.
 * @param {string} params.token - The token of the user.
 * @param {string} params.bindingUrl - The binding url.
 * @returns {CancelablePromise<any>} Return promise with the sensors.
 * @method
 */
GoInteractions.prototype.getAllsensorsFromNexus =
	functions.nexusInfo.getAllsensorsFromNexus;

/**
 * This function allow to get the historical data from nexus sensors.
 * @param {Object} params
 * @param {string} params.sensorUID - The sensor UID.
 * @param {string} params.token - The token of the user.
 * @param {string} params.bindingUrl - The binding url.
 * @returns {CancelablePromise<any>} Return promise with the historical data.
 * @method
 */
GoInteractions.prototype.getHistoricInfoFromNexus =
	functions.nexusInfo.getHistoricInfoFromNexus;
// Overlay methods ---------------------------------------------------------
/**
 * This function allow to add a overlay to the map.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idOverlay - The ID of the overlay.
 * @param {import('types').overlayData} params.overlayData - The data of the overlay.
 * @returns {GoOverlay | undefined} Return the overlay or undefined.
 * @method
 */
GoInteractions.prototype.addOverlay = functions.overlay.addOverlay;

/**
 * This function allow to determine if the overlay exist.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idOverlay - The ID of the overlay.
 * @returns {boolean} Return true if the overlay exist.
 * @method
 */
GoInteractions.prototype.hasOverlay = functions.overlay.hasOverlay;

/**
 * This function allow to remove the overlay.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idOverlay - The ID of the overlay.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeOverlay = functions.overlay.removeOverlay;

// GetFeaturesByLayer methods --------------------------------------------------
/**
 * Retrieves features from a layer using authenticated GeoServer WFS requests.
 * The layer must be defined in the map configuration.
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - The ID of the map
 * @param {string} [params.layerId] - The ID of the layer from config (either layerId or layerName is required)
 * @param {string} [params.layerName] - Direct GeoServer layer name in format "workspace:layerName" (either layerId or layerName is required)
 * @param {string} [params.cqlFilter] - Optional CQL filter expression
 * @param {number} [params.maxFeatures] - Maximum number of features to return (if not provided, returns all features)
 * @param {string[]} [params.propertyName] - Specific properties to return (if not provided, returns all properties)
 * @returns {Promise<types.GeoJSONFeatureCollection>} Promise that resolves to a GeoJSON feature collection
 * @method
 * @example
 * // Using layerId (from map config)
 * map.interactions.getFeaturesByLayer({
 *   idMap: "main",
 *   layerId: "valves",
 *   cqlFilter: "status='active'",
 *   maxFeatures: 100,
 *   propertyName: ["id", "name"]
 * });
 * 
 * // Using layerName (direct GeoServer)
 * map.interactions.getFeaturesByLayer({
 *   idMap: "main",
 *   layerName: "workspace:layerName",
 *   cqlFilter: "status='active'"
 * });
 */
GoInteractions.prototype.getFeaturesByLayer =
	functions.getFeaturesByLayer.getFeaturesByLayer;

// GetProperties methods -------------------------------------------------------
/**
 * This function allow to add hightlight to the map.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idInteraction - The ID of the highlight.
 * @returns {CancelablePromise<GoHighlight>} Return the CancelablePromise<GoHighlight>.
 * @method
 */
GoInteractions.prototype.getAllFeaturesProperties =
	functions.getAllFeaturesProperties.getAllFeaturesProperties;
// Highlight methods -------------------------------------------------------
/**
 * This function allow to add hightlight to the map.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @param {Array<Feature>} params.featuresGeom - The features of the highlight.
 * @param {import('types').styleFromUser} params.style - The style of the highlight.
 * @param {Function} params.textFunction - The function to get the text of the highlight.
 * @param {number} params.zIndex - zIndex of the highlight layer
 * @returns {CancelablePromise<GoHighlight>} Return the CancelablePromise<GoHighlight>.
 * @method
 */
GoInteractions.prototype.addHighlight = functions.highlight.addHighlight;

/**
 * This function allow to determine if the highlight exist.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @returns {boolean} Return true if the highlight exist.
 * @method
 */
GoInteractions.prototype.hasHighlight = functions.highlight.hasHighlight;

/**
 * This function allow to get the highlight.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @returns {GoHighlight} Return the highlight.
 * @method
 */
GoInteractions.prototype.getHighlight = functions.highlight.getHighlight;

/**
 * This function allow to remove the highlight.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeHighlight = functions.highlight.removeHighlight;

/**
 * This function allow to remove the highlight but only if the highlight exists.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeHighlightIfExist =
	functions.highlight.removeHighlightIfExist;

/**
 * This function allow to change the zIndex of the highlight.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @param {number} params.zIndex - The zIndex of the highlight.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.changeHighlightZIndex =
	functions.highlight.changeHighlightZIndex;

/**
 * This function allow to change the highlight style if the highlight exists.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @param {import('types').styleFromUser} params.userStyle - The highlight style in json variable.
 * @param {import('types').ISVGIcon} params.styleSVG - The highlight style as SVG icon.
 * @returns {void}
 * @method
 */
GoInteractions.prototype.changeStyleOfHighlight =
	functions.highlight.changeStyleOfHighlight;

/**
 * This function allow to restore the highlight style.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHighlight - The ID of the highlight.
 * @returns {void}
 * @method
 */
GoInteractions.prototype.restoreHighlightStyles =
	functions.highlight.restoreHighlightStyles;
// WMS GetFeatureInfo methods -----------------------------------------------
/**
 * This function allow to add WMSGetFeatureInfo to the map.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idWMSGetFeatureInfo - The ID of the WMSGetFeatureInfo.
 * @param {Array<Feature>} params.featuresGeom - The features of the highlight.
 * @returns {CancelablePromise<GoWMSGetFeatureInfo>} Return a promise of GoWMSGetFeatureInfo
 * @method
 */
GoInteractions.prototype.addWMSGetFeatureInfo =
	functions.wmsFeatureInfo.addWMSGetFeatureInfo;

/**
 * This function allow to determine if the WMSGetFeatureInfo exist.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idWMSGetFeatureInfo - The ID of the WMSGetFeatureInfo.
 * @returns {boolean} Return true if the WMSGetFeatureInfo exist.
 * @method
 */
GoInteractions.prototype.hasWMSGetFeatureInfo =
	functions.wmsFeatureInfo.hasWMSGetFeatureInfo;

/**
 * This function allow to get the WMSGetFeatureInfo.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idWMSGetFeatureInfo - The ID of the WMSGetFeatureInfo.
 * @returns {GoWMSGetFeatureInfo} Return the WMSGetFeatureInfo.
 * @method
 */
GoInteractions.prototype.getWMSGetFeatureInfo =
	functions.wmsFeatureInfo.getWMSGetFeatureInfo;

/**
 * This function allow to remove the WMSGetFeatureInfo.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idWMSGetFeatureInfo - The ID of the WMSGetFeatureInfo.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeWMSGetFeatureInfo =
	functions.wmsFeatureInfo.removeWMSGetFeatureInfo;
// Hover Feature methods ---------------------------------------------------
/**
 * This functio determine if the map has hover feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @returns {boolean} Return true if the hover exist.
 * @method
 */
GoInteractions.prototype.hasHoverFeature =
	functions.hoverFeature.hasHoverFeature;

/**
 * This function allow to get the hover feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @returns {GoHover} Return the hover feature.
 * @method
 */
GoInteractions.prototype.getLayerHover = functions.hoverFeature.getLayerHover;

/**
 * Implements mouse hover interaction on map features.
 *
 * @param {Object} params - Configuration object for hover interaction.
 * @param {string} params.idHover - The ID for the hover layer.
 * @param {OlMap} params.olMap - The OpenLayers map instance.
 * @param {GOVectorLayer} [params.layer] - Optional. Layer to apply the hover interaction.
 * @param {styleFromUser} [params.style] - Optional. Custom style to apply on hover.
 * @param {Function} [params.callback] - Optional. Callback triggered when a feature is hovered. Receives feature: Feature<Geometry> as parameter.
 * @param {types.ISVGIcon} [params.styleSVG] - Optional. SVG-based icon style.
 * @param {string} [params.svgIcon] - Optional. Path or ID of the SVG icon as string.
 * @param {number} [params.zIndex] - Optional. z-index of the hover layer.
 * @returns {GoHover|void} The hover interaction instance or nothing.
 * @method
 */
GoInteractions.prototype.mouseHover = functions.hoverFeature.mouseHover;

/**
 * This function allow to remove the hover feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeHover = functions.hoverFeature.removeHover;

/**
 * This function allow to determine if the feature has the id of the map.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @returns {boolean} Return true if the feature exist.
 * @method
 */
GoInteractions.prototype.hasGetFeature = functions.hoverFeature.hasGetFeature;

/**
 * This function allow to add the tooltip feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID Of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @param {Feature<Geometry>} params.featuresGeom - The feature of the geometry.
 * @param {import('types').styleFromUser} params.style - The styles of the user.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addTooltipHoverFeature =
	functions.hoverFeature.addTooltipHoverFeature;

/**
 * If the map has the key, and the value of the key is a map that has the key, then return true.
 * @param {Object} params
 * @param {string} params.idMap - The id of the map that the tooltip is on.
 * @param {string} params.idHover
 * @returns {boolean} A boolean value.
 * @method
 */
GoInteractions.prototype.hasToolTipHover =
	functions.hoverFeature.hasToolTipHover;

/**
 * This function allow to get the tooltip feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @returns {GoHoverFeature} Return the tooltip feature.
 * @method
 */
GoInteractions.prototype.getTooltipHoverFeature =
	functions.hoverFeature.getTooltipHoverFeature;

/**
 * This function allow to remove the tooltip feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeTooltipHoverFeature =
	functions.hoverFeature.removeTooltipHoverFeature;

/**
 * This function allow to remove the tooltip feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idHover - The ID of the hover.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.changeHoverZIndex =
	functions.hoverFeature.changeHoverZIndex;

// Close Tool methods ------------------------------------------------------

/**
 * This function allow to add the closetool feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idCloseTool - The ID of the closetool.
 * @param {number} params.id - The ID of the feature.
 * @param {string} params.tablePipe - The table pipe.
 * @param {string} params.tableValve - The table valve.
 * @param {import('types').styleFromUser} params.userStyleValve - The valve style of the user.
 * @param {import('types').styleFromUser} params.userStylePipes - The pipes style of the user.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addCloseTool = functions.closeTool.addCloseTool;

/**
 * This function allow to clear the closetool feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idCloseTool - The ID of the closetool.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.clearCloseTool = functions.closeTool.clearCloseTool;

/**
 * This function takes a CloseTool object and adds it to the closeToolData object.
 * @param {Object} params
 * @param {types.CloseTool} params.data
 * @method
 */
GoInteractions.prototype.addExtraDataCloseToolInteractions =
	functions.closeTool.addExtraDataCloseToolInteractions;

/**
 * Allow to add upDownToolData to the interaction.
 * @param {Object} params
 * @param {types.UpDown} params.data
 * @method
 */
GoInteractions.prototype.addExtraDataUpDownStreamInteractions =
	functions.closeTool.addExtraDataUpDownStreamInteractions;

/**
 * This function allow to determine if the closeTool feature exist.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idCloseTool - The ID of the closetool.
 * @returns {boolean} Return true if the closeTool feature exist, false otherwise.
 * @method
 */
GoInteractions.prototype.hasCloseTool = functions.closeTool.hasCloseTool;

/**
 * This function allow to get the closetool feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idCloseTool - The ID of the closetool.
 * @returns {GoCloseTools} Return the closetool feature.
 * @method
 */
GoInteractions.prototype.getCloseTool = functions.closeTool.getCloseTool;

/**
 * This function allow to remove the closetool feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idCloseTool - The ID of the closetool.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeCloseTool = functions.closeTool.removeCloseTool;

// UpDownStream

/**
 * This function allow to add the closetool feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idCloseTool - The ID of the closetool.
 * @param {number} params.id - The ID of the feature.
 * @param {string} params.tablePipe - The table pipe.
 * @param {string} params.tableValve - The table valve.
 * @param {import('types').styleFromUser} params.userStyleValve - The valve style of the user.
 * @param {import('types').styleFromUser} params.userStylePipes - The pipes style of the user.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addUpDownStream =
	functions.updownstream.addUpDownStream;

/**
 * This function allow to determine if the UpDown feature exist.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idUpDown - The ID of the UpDown.
 * @returns {boolean} Return true if the UpDown feature exist, false otherwise.
 * @method
 */
GoInteractions.prototype.hasUpDownStream =
	functions.updownstream.hasUpDownStream;

/**
 * This function allow to remove the UpDown feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idUpDown - The ID of the UpDown.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeUpDownStream =
	functions.updownstream.removeUpDownStream;

/**
 * This function allow to clear the UpDown feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idUpDown - The ID of the UpDown.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.clearUpDownStream =
	functions.updownstream.clearUpDownStream;

// Intersect Tools methods -------------------------------------------------------

/**
 * This function allow to add the intersect feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idIntersect - The ID of the intersect.
 * @param {string} params.intersectDataIn - The layer for intersect.
 * @param {string} params.intersectDataClip - The layer to intersect.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addIntersect = functions.intersect.addIntersect;

/**
 * Determine if the intersect exists
 * @param {Object} params
 * @param {string} params.idMap - the id of the map
 * @param {string} params.idIntersect - The id of the intersect
 * @returns {boolean} A boolean value.
 * @method
 */
GoInteractions.prototype.hasIntersect = functions.intersect.hasIntersect;

/**
 * This function allow to clear the intersect feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idIntersect - The ID of the intersect.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.clearIntersect = functions.intersect.clearIntersect;

/**
 * This function allow to remove the intersect feature.
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idIntersect - The ID of the intersect.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeIntersect = functions.intersect.removeIntersect;

GoInteractions.prototype.refreshGeoTools = functions.geoTools.refreshGeoTools;

// Select By Polygon methods --------------------------------------------
/**
 * Interaction that allows a polygon to be drawn and everything that has intersected on it is obtained
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idDrawSelectByPolygon
 * @param {Function(feature)} params.callback
 * @param {Array<string>} params.layers
 * @param {import('types').userStyle} params.style
 * @param {boolean} params.clickFinish
 * @returns {GoDrawSelectByPolygon}
 * @method
 */
GoInteractions.prototype.addDrawSelectByPolygon =
	functions.selectByPoligon.addDrawSelectByPolygon;

/**
 * Method that returns if a select interaction exists
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idDrawSelectByPolygon - The ID of the select interaction.
 * @returns {boolean}
 * @method
 */
GoInteractions.prototype.hasDrawSelectByPolygon =
	functions.selectByPoligon.hasDrawSelectByPolygon;

/**
 * Method to active or not active Interaction Select By Polygon
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygon
 * @param {boolean} params.active
 * @method
 */
GoInteractions.prototype.setActiveDrawSelectByPolygon =
	functions.selectByPoligon.setActiveDrawSelectByPolygon;

/**
 * Remove Draw Select By Polygon Interaction
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idDrawSelectByPolygon - The ID of the select interaction.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeDrawSelectByPolygon =
	functions.selectByPoligon.removeDrawSelectByPolygon;

/**
 * Interaction that allows a polygon to be drawn and everything that has intersected on it is obtained
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygon
 * @param {Function(feature)} params.callback
 * @param {Array<string>} params.layers
 * @returns {GoDrawSelectByPolygon}
 * @method
 */
GoInteractions.prototype.addSelectByPolygon =
	functions.selectByPoligon.addSelectByPolygon;

/**
 * Method that returns if a select interaction exists
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygon
 * @returns {boolean}
 * @method
 */
GoInteractions.prototype.hasSelectByPolygon =
	functions.selectByPoligon.hasSelectByPolygon;

/**
 * Remove Select By Polygon Interaction
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygon
 * @method
 */
GoInteractions.prototype.removeSelectByPolygon =
	functions.selectByPoligon.removeSelectByPolygon;

// Select By Polygon in Cluster methods --------------------------------------------
/**
 * Interaction that allows a polygon to be drawn and everything that has intersected on it is obtained
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idDrawSelectByPolygonInCluster
 * @param {Function(feature)} params.callback
 * @param {Array<string>} params.layers
 * @param {import('types').userStyle} params.style
 * @param {boolean} params.clickFinish
 * @returns {GoDrawSelectByPolygon}
 * @method
 */
GoInteractions.prototype.addDrawSelectByPolygonInCluster =
	functions.selectByPoligon.addDrawSelectByPolygonInCluster;

/**
 * Method that returns if a select interaction exists
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idDrawSelectByPolygonInCluster - The ID of the select interaction.
 * @returns {boolean}
 * @method
 */
GoInteractions.prototype.hasDrawSelectByPolygonInCluster =
	functions.selectByPoligon.hasDrawSelectByPolygonInCluster;

/**
 * Method to active or not active Interaction Select By Polygon
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idDrawSelectByPolygonInCluster
 * @param {boolean} params.active
 * @method
 */
GoInteractions.prototype.setActiveDrawSelectByPolygonInCluster =
	functions.selectByPoligon.setActiveDrawSelectByPolygonInCluster;

/**
 * Remove Draw Select By Polygon Interaction
 * @param {Object} params
 * @param {string} params.idMap - The ID of the map.
 * @param {string} params.idDrawSelectByPolygonInCluster - The ID of the select interaction.
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeDrawSelectByPolygonInCluster =
	functions.selectByPoligon.removeDrawSelectByPolygonInCluster;

/**
 * Interaction that allows a polygon to be drawn and everything that has intersected on it is obtained
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygonInCluster
 * @param {Function(feature)} params.callback
 * @param {Array<string>} params.layers
 * @returns {GoDrawSelectByPolygon}
 * @method
 */
GoInteractions.prototype.addSelectByPolygonInCluster =
	functions.selectByPoligon.addSelectByPolygonInCluster;

/**
 * Method that returns if a select interaction exists
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygonInCluster
 * @returns {boolean}
 * @method
 */
GoInteractions.prototype.hasSelectByPolygonInCluster =
	functions.selectByPoligon.hasSelectByPolygonInCluster;

/**
 * Remove Select By Polygon Interaction
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idSelectByPolygonInCluster
 * @method
 */
GoInteractions.prototype.removeSelectByPolygonInCluster =
	functions.selectByPoligon.removeSelectByPolygonInCluster;

// Rotation methods -------------------------------------------------------
/**
 * This method checks if rotation interaction exists
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.idRotation - The id of the rotation interaction
 * @returns {boolean} - Return true if rotation interaction exists
 * @method
 */
GoInteractions.prototype.hasRotation = functions.rotation.hasRotation;

/**
 * Remove Rotation Interaction
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.idRotation - The id of the rotation interaction
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeRotation = functions.rotation.removeRotation;

/**
 * Interaction to rotate view
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idRotation - The id of the rotation interaction
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addRotation = functions.rotation.addRotation;

/**
 * Interaction to disable rotate view
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.disableRotation = functions.rotation.disableRotation;

/**
 * Get the rotation interaction
 * @param {Object} params
 * @param {string} params.idMap - Id of the map that contains the rotatable view
 * @param {string} params.idRotation - Id of the rotation interaction
 * @returns {GoRotation} - Return the rotation interaction
 * @method
 */
GoInteractions.prototype.getRotation = functions.rotation.getRotation;

/**
 * Change perspective with right mouse button
 * @param {Object} params
 * @param {OlMap} params.olMap - the map object
 * @param {number} [params.sensibilityMultiplier=2] - The higher the number, the more sensitive the mouse is.
 * @method
 */
GoInteractions.prototype.addMouseRotation = functions.rotation.addMouseRotation;

/**
 * This method checks if print interaction exists
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @returns {HTMLElement} - Return HTMLElement or null
 * @method
 */
GoInteractions.prototype.addCustomRotateCompass =
	functions.rotation.addCustomRotateCompass;

// Print methods -------------------------------------------------------

/**
 * This method checks if print interaction exists
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.idPrint - The id of the print interaction
 * @returns {boolean} - Return true if print interaction exists
 * @method
 */
GoInteractions.prototype.hasPrint = functions.print.hasPrint;

/**
 * Remove Print Interaction
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.idPrint - The id of the print interaction
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removePrint = functions.print.removePrint;

/**
 * Adds a print interaction to the map.
 *
 * @param {Object} params - Parameters for the print interaction
 * @param {string} params.idMap - The ID of the map
 * @param {string} params.idPrint - The ID for the print interaction
 * @param {string} [params.mapTitle] - Title of the map
 * @param {string} [params.name] - Name of the map
 * @param {string} [params.author] - Author of the map
 * @param {string} [params.date] - Date of the map
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addPrint = functions.print.addPrint;

/**
 * Get the print interaction
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.idPrint - Id of the print interaction
 * @returns {GoPrint} - Return the print interaction
 * @method
 */
GoInteractions.prototype.getPrint = functions.print.getPrint;

// Measure methods -------------------------------------------------------

/**
 * Interaction for measure with line or polygon area
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idMeasure
 * @param {GeometryType} params.geometryType
 * @param {Function(feature)} params.callbackStart
 * @param {Function(feature)} params.callbackEnd
 * @param {import('types').userStyle} params.style
 * @param {boolean} params.defaultTooltip
 * @param {unitSystem} params.unitSystemSelected
 * @returns {GoMeasure}
 * @method
 */
GoInteractions.prototype.addMeasure = functions.measure.addMeasure;

/**
 * Method that returns if a select interaction exists
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idMeasure
 * @returns {boolean}
 * @method
 */
GoInteractions.prototype.hasMeasure = functions.measure.hasMeasure;

/**
 * Method to active or not active Interaction Measure
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idMeasure
 * @param {boolean} params.active
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.setActiveMeasure = functions.measure.setActiveMeasure;

/**
 * Remove Measure Layer data
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idMeasure
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.clearMeasure = functions.measure.clearMeasure;

/**
 * Remove Measure Interaction
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idMeasure - The id of the measure
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeMeasure = functions.measure.removeMeasure;

// Flow Lines methods ---------------------------------------------------------

/**
 * Interaction to transform a simple style in flow line style. This interactions is only for LineString Geometry
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idFlowLines
 * @param {string} params.idLayer
 * @returns {GoflowLines}
 * @method
 */
GoInteractions.prototype.addFlowLines = functions.flowLines.addFlowLines;

/**
 * Method that returns if a select interaction exists
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idFlowLines
 * @returns {boolean}
 * @method
 */
GoInteractions.prototype.hasFlowLines = functions.flowLines.hasFlowLines;

/**
 * Remove Select By Polygon Interaction
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idFlowLines - The id of the flowLines
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeFlowLines = functions.flowLines.removeFlowLines;

// Density methods ---------------------------------------------------------

/**
 * Interaction to transform a Point layer in density layer
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idDensityTool - The id of the density tool
 * @param {string} params.layer - The layer to be transformed
 * @param {import('constants').densityToolType} params.type
 * @returns {GoDensityTool}
 * @method
 */
GoInteractions.prototype.addDensityLayer =
	functions.densityLayer.addDensityLayer;

/**
 * Interaction to transform a Point layer in density layer
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idDensityTool - The id of the density tool
 * @param {import('constants').densityToolType} params.type
 * @returns {GoDensityTool}
 * @method
 */
GoInteractions.prototype.hasDensityLayer =
	functions.densityLayer.hasDensityLayer;

/**
 * Interaction to transform a Point layer in density layer
 * @param {Object} params
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idDensityTool - The id of the density tool
 * @param {import('constants').densityToolType} params.type
 * @returns {GoDensityTool}
 * @method
 */
GoInteractions.prototype.getDensityLayer =
	functions.densityLayer.getDensityLayer;

/**
 * This function allow to remove the density of the layer.
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idDensityTool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeDensityLayer =
	functions.densityLayer.removeDensityLayer;

// Feature methods ---------------------------------------------------------
/**
 * This method checks if any edit feature interaction exists
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idEditFeature
 * @returns {boolean} - Return true if the edit feature exists
 * @method
 */
GoInteractions.prototype.hasEditFeature = functions.feature.hasEditFeature;

/**
 * This function takes a GeoJSON object and returns an array of features.
 * @param {Object} params
 * @param {*} params.feature
 * @returns A GeoJSON object.
 * @method
 */
GoInteractions.prototype.createFeature = functions.feature.createFeature;

/**
 * Remove Edit Feature Interaction
 * @param {Object} params
 * @param {string} params.idMap
 * @param {string} params.idEditFeature
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeEditFeature =
	functions.feature.removeEditFeature;

/**
 * Interaction to edit feature geometry
 * @param {Object} params
 * @param {string} params.idEditFeature - The id of the edit feature
 * @param {string} params.idMap - The id of the map
 * @param {string} params.idLayer - The layer to be edited
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addEditFeature = functions.feature.addEditFeature;

/**
 * Interaction for measure with line or polygon area
 * @param {Object} params
 * @param {string} params.idMap - Id of the map that contains the editable feature/s
 * @param {string} params.idEditFeature - Id of the edit interaction
 * @returns {GoEditFeatures} - Return the edit feature
 * @method
 */
GoInteractions.prototype.getEditFeatures = functions.feature.getEditFeatures;

/**
 * Interaction to edit feature geometry
 * @param {Object} params
 * @param {string} params.idEditFeature Id of the edit interaction
 * @param {string} params.idMap Id of the map that contains the editable feature/s
 * @param {string} params.idLayer Id of the layer that contains the editable feature/s
 * @param {string} params.user Geoserver username with permisssions to make the transaction
 * @param {string} params.pass Geoserver password with permisssions to make the transaction
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.saveEditFeatures = functions.feature.saveEditFeatures;

/**
 * This Method allows to delete feature/s
 * @param {Object} params
 * @param {string} params.idMap Id of the map that contains the feature/s that you want to delete
 * @param {string} params.idLayer Id of the vector layer that contains the feature/s that you want to delete
 * @param {Array<Feature>} params.feature Array of feature/s you want to delete
 * @param {string} params.user Geoserver username with permisssions to make the transaction
 * @param {string} params.pass Geoserver password with permisssions to make the transaction
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.deleteFeatures = functions.feature.deleteFeatures;

// Custom zoom methods -----------------------------------------------------

/**
 * This function allow to add a custom zoom to the map
 * @param {Object} params
 * @param {string} params.theme string to define if the theme is dark or light
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.addCustomZoom = functions.customZoom.addCustomZoom;

/**
 * This function allow to remove a custom zoom to the map
 * @param {Object} params
 * @param {string} params.theme string to define if the theme is dark or light
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.removeCustomZoom =
	functions.customZoom.removeCustomZoom;

/**
 * This function allow to remove all the custom zooms of the map
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.removeAllCustomZoom =
	functions.customZoom.removeAllCustomZoom;

// Custom Zoom CtrlScroll  ------------------------------------------------------

/**
 * This function allow to enable the custom zoom with ctrl + scroll
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.addCtrlScrollZoom =
	functions.customZoom.addCtrlScrollZoom;

/**
 * This function allow to determine if the custom zoom with ctrl + scroll is enabled on the map
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.hasCtrlScrollZoom =
	functions.customZoom.hasCtrlScrollZoom;

/**
 * This function allow to disable the custom zoom with ctrl + scroll
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.removeCtrlScrollZoom =
	functions.customZoom.removeCtrlScrollZoom;

// Zoom interaction  ------------------------------------------------------

/**
 * This function allow to disable the zoom interaction
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.disableDoubleClickZoom =
	functions.customZoom.disableDoubleClickZoom;

/**
 * This function allow to enable the zoom interaction
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.enableDoubleClickZoom =
	functions.customZoom.enableDoubleClickZoom;

/**
 * This function allow to determine if the zoom interaction is enabled
 * @param {Object} params
 * @param {string} params.idMap Id of the map
 * @method
 */
GoInteractions.prototype.hasDisabledDoubleClickZoom =
	functions.customZoom.hasDisabledDoubleClickZoom;

// IFC methods -------------------------------------------------------------
/**
 * Interaction to transform a simple style in flow line style. This interactions is only for LineString Geometry
 * @param {IFCConfig} props
 * @returns {Promise<GoIFCComponents>}
 * @method
 */
GoInteractions.prototype.createIFC = functions.ifc.createIFC;
/**
 * This method checks if any edit feature interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap
 * @param {string} params.idEditFeature
 * @returns {boolean} - Return true if the edit feature exists
 * @method
 */
GoInteractions.prototype.hasIFCModel = functions.ifc.hasIFCModel;

/**
 * Remove Edit Feature Interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap
 * @param {string} params.idEditFeature
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeIFCModel = functions.ifc.removeIFCModel;

/**
 * Create the DMD interaction
 * @param {Object} params - Parameters object
 * @param {string} params.dmdId - The id of the DMD interaction
 * @param {string} params.dmdToken - The token of the DMD service
 * @returns {GoDMD} Return the DMD interaction
 * @method
 */
GoInteractions.prototype.createDMD = functions.dmd.createDMD;

/**
 * Get the DMD interaction
 * @param {Object} params - Parameters object
 * @param {string} params.dmdId - The id of the DMD interaction
 * @returns {GoDMD} Return the DMD interaction
 * @method
 */
GoInteractions.prototype.getDMD = functions.dmd.getDMD;

/**
 * Allow to get an array of the DMD interactions
 * @returns {Array<object>} Return the DMD interaction
 * @method
 */
GoInteractions.prototype.getDMDs = functions.dmd.getDMDs;

/**
 * Allow to determine if the map has a DMD interaction by id
 * @param {Object} params - Parameters object
 * @param {string} params.dmdId - The id of the DMD interaction
 * @returns {GoDMD} Return the DMD interaction
 * @method
 */
GoInteractions.prototype.hasDMD = functions.dmd.hasDMD;

/**
 * Allow to remove the DMD interaction by id
 * @param {Object} params - Parameters object
 * @param {string} params.dmdId - The id of the DMD interaction
 * @returns {boolean} Return true if the DMD interaction was removed
 * @method
 */
GoInteractions.prototype.removeDMD = functions.dmd.removeDMD;

/**
 * Create the Cadastre interaction
 * @param {Object} params - Parameters object
 * @param {string} params.cadastreId - The id of the Cadastre interaction
 * @returns {GoCadastre} Return the Cadastre interaction
 * @method
 */
GoInteractions.prototype.createCadastre = functions.cadastre.createCadastre;

/**
 * Get the Cadastre interaction
 * @param {Object} params - Parameters object
 * @param {string} params.cadastreId - The id of the Cadastre interaction
 * @returns {GoCadastre} Return the Cadastre interaction
 * @method
 */
GoInteractions.prototype.getCadastre = functions.cadastre.getCadastre;

/**
 * Allow to get an array of the Cadastre interactions
 * @returns {Array<GoCadastre>} Return the Cadastre interaction
 * @method
 */
GoInteractions.prototype.getCadastres = functions.cadastre.getCadastres;

/**
 * Allow to determine if the map has a Cadastre interaction by id
 * @param {Object} params - Parameters object
 * @param {string} params.cadastreId - The id of the Cadastre interaction
 * @returns {GoCadastre} Return the Cadastre interaction
 * @method
 */
GoInteractions.prototype.hasCadastre = functions.cadastre.hasCadastre;

/**
 * Allow to remove the Cadastre interaction by id
 * @param {Object} params - Parameters object
 * @param {string} params.cadastreId - The id of the Cadastre interaction
 * @returns {boolean} Return true if the Cadastre interaction was removed
 * @method
 */
GoInteractions.prototype.removeCadastre = functions.cadastre.removeCadastre;

/**
 * Create thematicMap interaction
 * @param {Object} params - Parameters object
 * @param {GoMap} params.goMap - The mapId
 * @param {string} params.mapId - The mapId
 * @param {string} params.thematicMapId - The id of the thematic map
 * @param {LayerGroupIA} params.layerGroupIA - Array of LayerIA
 */
GoInteractions.prototype.addThematicMap = functions.thematicMap.addThematicMap;

/**
 * return if exist thematicMap interaction
 * @param {Object} params - Parameters object
 * @param {string} params.mapId - The mapId
 * @param {string} params.thematicMapId - The id of the thematic map
 * @returns {Boolean} Return true | false
 */
GoInteractions.prototype.hasThematicMap = functions.thematicMap.hasThematicMap;

/**
 * get LayerGroupIA (array LayerIA) of thematicMap
 * @param {Object} params - Parameters object
 * @param {string} params.mapId - The mapId
 * @param {string} params.thematicMapId - The id of the thematic map
 * @returns {LayerGroupIA} Array of LayerIA
 */
GoInteractions.prototype.getThematicMap = functions.thematicMap.getThematicMap;

/**
 * remove thematicMap interaction if exists
 * @param {Object} params - Parameters object
 * @param {string} params.mapId - The mapId
 * @param {string} params.thematicMapId - The id of the thematic map
 * @returns {void} Return nothing
 */
GoInteractions.prototype.removeThematicMap =
	functions.thematicMap.removeThematicMap;

/**
 * Add Near Data Interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idNearData The id from a tool
 * @param {Array<number>} params.pointCoordinate The coordinate data
 * @param {import('types').dataTools} params.lookingForData The base layer to looking for near data
 * @param {import('types').ISVGIcon} params.stylePoint The style we want to put on the Valves drawing layer
 * @param {import('types').styleFromUser} params.style The style we want to put on the Valves drawing layer
 * @returns {CancelablePromise<any>} Return promise with the closest element.
 * @method
 */
GoInteractions.prototype.addNearData = functions.nearData.addNearData;

/**
 * This method allows to know if the near data interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idNearData The id of the tool
 * @returns {Boolean} Return true | false
 * @method
 */
GoInteractions.prototype.hasNearData = functions.nearData.hasNearData;

/**
 * This method allows to remove the near data interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idNearData The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeNearData = functions.nearData.removeNearData;

/**
 * Add Near Data Attributes Interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idInteraction The id from a tool
 * @param {Array<number>} params.pointCoordinate The coordinate data
 * @param {import('types').dataTools} params.layer The base layer to looking for near data attributes
 * @param {import('types').ISVGIcon} params.stylePoint The style we want to put on the Valves drawing layer
 * @param {import('types').styleFromUser} params.style The style we want to put on the Valves drawing layer
 * @returns {CancelablePromise<any>} Return promise with the closest element.
 * @method
 */
GoInteractions.prototype.addNearDataAttributes =
	functions.nearDataAttributes.addNearDataAttributes;

/**
 * This method allows to know if the near data Attributes interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idInteraction The id of the tool
 * @returns {Boolean} Return true | false
 * @method
 */
GoInteractions.prototype.hasNearDataAttributes =
	functions.nearDataAttributes.hasNearDataAttributes;

/**
 * This method allows to remove the near data Attributes interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idInteraction The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeNearDataAttributes =
	functions.nearDataAttributes.removeNearDataAttributes;

/**
 * Add Buffer Interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idBuffer The id of the tool
 * @param {import('types').bufferParams} params.dataBuffer Array contain data and distance for buffering
 * @param {import('types').styleFromUser} params.style The style we want to put on the buffer layer
 * @returns {CancelablePromise<any>} Return promise with the buffer.
 * @method
 */
GoInteractions.prototype.addBuffer = functions.buffer.addBuffer;

/**
 * This method allows to know if the buffer interaction exists
 * @param {string} idMap The id of the map where we want to apply the tool
 * @param {string} idBuffer The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasBuffer = functions.buffer.hasBuffer;

/**
 * This method allows to remove the buffer interaction
 * @param {string} idMap The id of the map where we have the tool applied
 * @param {string} idBuffer The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeBuffer = functions.buffer.removeBuffer;

/**
 * Add SectorColor Interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.sectorColorId The id of the sector color interaction
 * @param {import('types').dataTools} params.sectorColorData Layer data
 * @param {import('types').sectorColorParams} params.sectorColorParams Array contain attribute and palette size
 * @returns {CancelablePromise<any>} Return promise with the sector color object.
 * @method
 */
GoInteractions.prototype.addSectorColor = functions.sectorColor.addSectorColor;

/**
 * This method allows to know if the sector color interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idSectorColor The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasSectorColor = functions.sectorColor.hasSectorColor;

/**
 * This method allows to remove the sector color interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idSectorColor The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeSectorColor =
	functions.sectorColor.removeSectorColor;

/**
 * Add polygonSector Interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.polygonSectorId The id of the polygon sector interaction
 * @param {import('types').dataTools} params.polygonSectorData Layer data
 * @param {import('types').polygonSectorParams} params.polygonSectorParams Array contain the size of the grid
 * @returns {Promise<any>} Return promise with the polygon sector object.
 * @method
 */
GoInteractions.prototype.addPolygonSector =
	functions.polygonSector.addPolygonSector;

/**
 * This method allows to know if the polygon sector interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idPolygonSector The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasPolygonSector =
	functions.polygonSector.hasPolygonSector;

/**
 * This method allows to remove the polygon sector interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idPolygonSector The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removePolygonSector =
	functions.polygonSector.removePolygonSector;

/**
 * This method allows to create an optical fiber segment interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idOpticalFiber The id of the tool
 * @param {types.opticalFiberParams} params.params The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addOpticalFiber =
	functions.opticalFiber.addOpticalFiber;

/**
 * This method allows to know if the optical fiber segment interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idOpticalFiber The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasOpticalFiber =
	functions.opticalFiber.hasOpticalFiber;

/**
 * This method allows to remove the optical fiber segment interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idOpticalFiber The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeOpticalFiber =
	functions.opticalFiber.removeOpticalFiber;

/**
 * This method allows to create an interpolation interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idInterpolation The id of the tool
 * @param {types.interpolationParams} params.params The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addInterpolation =
	functions.interpolation.addInterpolation;

/**
 * This method allows to know if the interpolation interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idInterpolation The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasInterpolation =
	functions.interpolation.hasInterpolation;

/**
 * This method allows to remove the interpolation interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idInterpolation The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeInterpolation =
	functions.interpolation.removeInterpolation;

/**
 * This method allows to create an isolines interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idIsolines The id of the tool
 * @param {types.isolinesParams} params.params The params of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addIsolines = functions.isolines.addIsolines;

/**
 * This method allows to know if the isolines interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idIsolines The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasIsolines = functions.isolines.hasIsolines;

/**
 * This method allows to remove the isolines interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idIsolines The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeIsolines = functions.isolines.removeIsolines;

/**
 * Add watershed Interaction
 * @param {Object} params - Parameters object
 * @param {import('types').watershedParams} params.params Layer data
 * @returns {CancelablePromise<any>} Return promise with the watershed object.
 * @method
 */
GoInteractions.prototype.addWatershed = functions.watershed.addWatershed;

/**
 * This method allows to know if the watershed interaction exists
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we want to apply the tool
 * @param {string} params.idWatershed The id of the tool
 * @returns {Boolean} Return true|false
 * @method
 */
GoInteractions.prototype.hasWatershed = functions.watershed.hasWatershed;

/**
 * This method allows to remove the watershed interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map where we have the tool applied
 * @param {string} params.idWatershed The id of the tool
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.removeWatershed = functions.watershed.removeWatershed;

/**
 * This method allow to get the extend of multiple layers
 * @param {Object} params - Parameters object
 * @param {string} params.idMap The id of the map
 * @param {string[]} params.idLayers Array of id layers
 * @returns {void} Return null or the extend of the layers
 * @method
 */
GoInteractions.prototype.getMultiExtent = functions.selection.getMultiExtent;

/**
 * This method allow to add a new event listener to the interactions.
 * @param {string} event The event name
 * @param {function} callback The callback function
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.on = functions.events.on;

/**
 * This method allow to remove an event listener to the interactions.
 * @param {string} event The event name
 * @param {function} callback The callback function
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.off = functions.events.off;

/**
 * This method allow to add a event listeners to the interactions that will be executed only once.
 * @param {string} event The event name
 * @param {function} callback The callback function
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.once = functions.events.once;

/**
 * This method allow to check if the interactions has an event listener.
 * @param {string} event The event name
 * @returns {boolean} Return true if the event exists
 * @method
 */
GoInteractions.prototype.hasEvent = functions.events.hasEvent;

/**
 * This method allow to get all the event listeners of the interactions.
 * @returns {object} Return an object with all the event listeners
 * @method
 */
GoInteractions.prototype.getEventHandlers = functions.events.getEventHandlers;

/**
 * This method allow to emit an event to the interactions.
 * @param {string} event The event name
 * @param {object} data The data to send to the event
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.emit = functions.events.emit;

/**
 * Get the Full Screen interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - Id of the map
 * @returns {GoFullScreen} - Return the Full Screen interaction
 * @method
 */
GoInteractions.prototype.hasFullScreen = functions.fullScreen.hasFullScreen;

/**
 * Remove the Full Screen interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - Id of the map
 * @returns {boolean} - Return true if the Full Screen interaction was removed
 * @method
 **/
GoInteractions.prototype.removeFullScreen =
	functions.fullScreen.removeFullScreen;

/**
 * Add new Full Screen interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - Id of the map
 * @returns {void} - Return nothing
 * @method
 */
GoInteractions.prototype.addFullScreen = functions.fullScreen.addFullScreen;

/**
 * Get the Full Screen interaction
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - Id of the map
 * @returns {GoFullScreen} - Return the Full Screen interaction
 * @method
 * */
GoInteractions.prototype.getFullScreen = functions.fullScreen.getFullScreen;

/**
 * This method allow to add a new event listener to the Full Screen interaction. To listenen when the screen is in full screen
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - Id of the map
 * @param {function} params.callback The callback function
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.onFullScreenEnter =
	functions.fullScreen.onFullScreenEnter;

/**
 * This method allow to add a new event listener to the Full Screen interaction. To listenen when the screen is exit from full screen
 * @param {Object} params - Parameters object
 * @param {string} params.idMap - Id of the map
 * @param {function} params.callback The callback function
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.onFullScreenExit =
	functions.fullScreen.onFullScreenExit;

/**
 * This method allow to add a new event listener to the Full Screen interaction. To listenen when the screen is exit from full screen
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.routingID - Id of the route
 * @param {string} params.transportMode - Transport mode
 * @param {Array<types.geodesicCoordinates>} params.points - Start and end point
 * @param {string} params.lang - Language code
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addRoutingHERE = functions.routingHERE.addRoutingHERE;

/**
 * This method allow to add a new event listener to the Full Screen interaction. To listenen when the screen is exit from full screen
 * @param {Object} params
 * @param {string} params.idMap - Id of the map
 * @param {string} params.routingID - Id of the route
 * @param {Array<types.geodesicCoordinates>} params.points - Start and end point
 * @param {Array<types.geodesicCoordinates>} params.stops - Stops along the route
 * @param {string} params.transportMode - Transport mode
 * @param {string} params.apiKey - API key
 * @returns {void} Return nothing
 * @method
 */
GoInteractions.prototype.addMultiRoutingHERE =
	functions.routingHERE.addMultiRoutingHERE;

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Toc\GoOwnToc.ts
import { GoMap } from "@map/GoMap";
// import "./owntoc.css";
import * as owntocJs from "@utils/classes/static/owntoc.js";
import * as constants from "@utils/constants";
import { GoOwnTocEvents, themes } from "@utils/constants";
import * as types from "@utils/types/types";
import i18nService from "../i18nService";

import { GoCustomLegend } from "./CustomLegend";
import { MapEventManager } from "../Utils";

const svgOpenedEye =
	'<svg class="icon-eye" viewBox="0 0 24 24" aria-hidden="true" focusable="false">' +
	'<path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z" stroke="currentColor" fill="none"/>' +
	'<circle cx="12" cy="12" r="3" stroke="currentColor" fill="none"/>' +
	"</svg>";
const svgClosedEye =
	'<svg class="icon-eye-off" viewBox="0 0 24 24" aria-hidden="true" focusable="false">' +
	'<path d="M2 4.27L3.28 3 21 20.72 19.73 22l-2.2-2.2A10.94 10.94 0 0 1 12 21C7 21 2.73 17.39 1 13.5a11.6 11.6 0 0 1 3.11-4.26L2 4.27zM12 6c3.31 0 6.31 1.66 8.19 4.5a10.9 10.9 0 0 1-2.34 3.13L9.41 6.19A9.6 9.6 0 0 1 12 6z" stroke="currentColor" fill="none"/>' +
	"</svg>";

/**
 * @classdesc
 * This class allows render table of contents and legends dinamically
 * First of all, you need to include a new optionToc element in your config.json:
 * ```ts
 *      "optionToc":{
            "id": "toc-container",
            "panelId": "toc-panel",
            "theme": "DARK",
            "active": true,
            "maxHeight": 380,
            "legend": true,
			"searchLayersFilter" : true,
			"allFoldersExpanded": true,
			"tocAlwaysOpen" : true,
			"legendOpenByDefault" : true,
            "foldersOptions": [{
                "folder": "Capas Base/",
				"folderExpandedByDefault": true,
				"hasSwitcher": true,
				"onlyOneVisibleLayer": true,
				"folderAlwaysExpanded": true
            }]
        }
 * ```
 * id: DIV id of main container to render your TOC
 * panelId: DIV id of panel to render your TOC
 * active: state of TOC
 * maxHeight: maximum height of TOC in pixels
 * legend: boolean to render the legend dinamically
 * foldersOptions: object to define specific behaviour of folders. It allows to exclude switcher of selection to any folder and defines if one or multiple layers can be selected for that folder.
 * 
 * Only two levels of folder (folder/subfolder) can be automatically generated.
 * 
 * In addition, if you declare legend: true in optionToc, you should define legend: true in the object layer of config.json.
 * @constructor
 * @param {string} id - This is the id of the own toc.
 * @param {import('types').GoMap} goMap - This is the map object.
 * @api
 */

/**
 * @class GoOwnToc
 * @classdesc Manages the Table of Contents (TOC) for a specific map, including handling layer visibility and custom legends.
 */
export default class GoOwnToc {
	private id: string;
	private goMap: GoMap;
	public stageToc: types.ownToc;
	private config: types.GOMapOptions;
	public customLegend: GoCustomLegend;

	/**
	 * Initializes the GoOwnToc instance with a TOC ID and a map instance.
	 *
	 * @constructor
	 * @param {string} id - The unique identifier for the TOC.
	 * @param {GoMap} goMap - The map instance associated with this TOC.
	 */
	constructor(id: string, goMap: GoMap) {
		this.id = id;
		this.goMap = goMap;
	}

	public unmount(): void {
		this.stageToc = null as unknown as types.ownToc;
		this.config = null as unknown as types.GOMapOptions;
		this.id = null as unknown as string;
		this.goMap = null as unknown as GoMap;
		this.customLegend = null as unknown as GoCustomLegend;

		// Remove all the created elements from the toc
		document.getElementById("ownTocContainer")?.remove();
	}

	/**
	 * Removes empty folders from the TOC and manages legend visibility based on active layers.
	 * This function is called after layer operations to clean up the TOC structure.
	 *
	 * IMPORTANT: Base layers should NEVER appear in legends, so they are excluded from legend calculations.
	 *
	 * @param {types.ownToc} stageToc - The TOC structure containing layers and legends
	 * @returns {boolean} Returns true when all non-base layers are inactive
	 */
	public removeFolderFromToc(stageToc: types.ownToc): boolean {
		let active_false;
		const actives: types.tocLayer[] = [];
		let allLayersInactive;

		stageToc.layers.forEach((folder, index) => {
			// Collect all non-base layers to check their active status
			folder.layer.forEach((el) => {
				if (!el.base_layer) {
					actives.push(el);
					active_false = actives.filter((el) => !el.active).length;
				}
			});

			// If all non-base layers in this folder are inactive, hide the folder
			if (active_false === actives.length) {
				folder.visible = false;
				allLayersInactive = true;
				folder.active = false;
				this.stageToc.layers.splice(index);

				// Deactivate the corresponding legend group
				if (stageToc.groupLegend[index]) {
					stageToc.groupLegend[index].active = false;
				}
			} else {
				// Some layers are still active, keep folder visible
				allLayersInactive = false;
				folder.visible = true;
				folder.active = true;

				// CRITICAL: Only activate legend if there are active NON-BASE layers with legends
				// Base layers should NEVER appear in legends regardless of their state
				if (stageToc.groupLegend[index]) {
					const hasActiveLegendLayers = folder.layer.some((layer) => {
						// Find if this layer has an active legend entry
						const legendGroup = stageToc.groupLegend[index];
						if (!legendGroup) return false;

						const layerLegend = legendGroup.legend.find(
							(legendItem) =>
								legendItem.id ===
								`el_leg_ownTocContainer-${layer.id}`
						);

						// Only count layers that:
						// 1. Have a legend entry AND it's active
						// 2. Layer itself is active and visible
						// 3. Layer is NOT a base layer (base layers never show in legends)
						return (
							layerLegend &&
							layerLegend.active &&
							layer.active &&
							layer.visible &&
							!layer.base_layer
						);
					});

					// Only activate legend group if there are qualifying non-base layers
					stageToc.groupLegend[index].active = hasActiveLegendLayers;
				}
			}

			// Clean up empty folders by removing them entirely
			if (!folder.layer.length) {
				this.removeLayer(folder.id);
			}
		});
		return allLayersInactive;
	}
	/**
	 * Initializes the TOC structure based on the provided configuration.
	 * This method creates the complete TOC and legend structure from the config.json file.
	 *
	 * IMPORTANT: Base layers should NOT be included in legends, so they are filtered out
	 * during legend creation.
	 *
	 * @param {any} config - The map configuration object containing layer and TOC settings
	 * @method
	 */
	public createStageToc(config: any): void {
		this.config = config;
		const optTocInfo = config.optionToc as types.optionsToc;
		const optToc: types.optionsToc = {
			id: optTocInfo.id,
			panelId: optTocInfo.panelId
				? optTocInfo.panelId
				: "ownTocContainer",
			mapId: config.id,
			theme: optTocInfo.theme
				? optTocInfo.theme
				: (themes.DARK as themes),
			active: optTocInfo.active,
			maxHeight: optTocInfo.maxHeight,
			legend: optTocInfo.legend,
			allFoldersExpanded: optTocInfo.allFoldersExpanded ?? false,
			tocAlwaysOpen: optTocInfo.tocAlwaysOpen ?? false,
			legendOpenByDefault: optTocInfo.legendOpenByDefault ?? false,
			searchLayersFilter: Object.prototype.hasOwnProperty.call(
				optTocInfo,
				"searchLayersFilter"
			)
				? optTocInfo.searchLayersFilter
				: false,
			foldersOptions: optTocInfo.foldersOptions,
		};

		const layers: Array<types.tocGroupLayer> = [];
		const groupLegend: Array<types.tocGroupLegend> = [];

		this.stageToc = {
			id: optToc.id,
			panelId: optToc.panelId,
			mapId: optToc.mapId,
			theme: optToc.theme,
			active: optToc.active,
			maxHeight: optToc.maxHeight,
			layers: layers,
			groupLegend: groupLegend,
			searchLayersFilter: optToc.searchLayersFilter,
			allFoldersExpanded: optToc.allFoldersExpanded,
			tocAlwaysOpen: optToc.tocAlwaysOpen,
			legendOpenByDefault: optToc.legendOpenByDefault,
			foldersOptions: optToc.foldersOptions,
		};
		MapEventManager.events.fire(GoOwnTocEvents.TOC_LOADING, this);
		const stagingMap = this.goMap.getStaging(this.config.id);

		if (!stagingMap) {
			console.error(
				`GoOwnToc: No staging found for map with id ${this.config.id}`
			);
			return;
		}

		// eslint-disable-next-line
		config.layer.forEach((l: any) => {
			const folderId =
				optToc.panelId +
				"-" +
				this.getFolderId(stagingMap.getConfigTranslation(l.urlFolder));
			const folderName = this.getFolderName(
				stagingMap.getConfigTranslation(l.urlFolder)
			);
			const subfolderName = this.getSubFolderName(
				stagingMap.getConfigTranslation(l.urlFolder)
			);
			const subfolderId =
				optToc.panelId + "-" + this.getSubFolderId(subfolderName);

			// Check if this folder ID already exists to determine if a new object element needs to be created
			let existsFolderId = false;
			let existsFolderLegendId = false;

			if (l.toc == undefined || l.toc) {
				// Layer visibility: if the layer is active or linked to an active one, set it as active
				let layerVisibility: boolean;
				if (l.defaultVisibility == undefined) {
					layerVisibility = true;
				} else {
					layerVisibility = l.defaultVisibility;
				}

				if (!layerVisibility && l.toc_link?.length) {
					// Layer is not active but is linked to others, check their visibility
					l.toc_link.forEach((layer_linkada) => {
						// Check if linked layers have defaultVisibility = true
						// eslint-disable-next-line
						config.layer.forEach((lay: any) => {
							if (
								lay.id === layer_linkada &&
								lay.defaultVisibility
							) {
								layerVisibility = true;
							}
						});
					});
				}
				// Create the tocLayer object
				const tocLayer: types.tocLayer = {
					id: l.id,
					subfolder: subfolderName,
					subfolder_id: subfolderId,
					name: l.alias,
					layerType: l.layerType,
					filterTocSubmenu: l.filterTocSubmenu
						? l.filterTocSubmenu
						: false,
					active: l.toc === false ? false : true,
					visible: layerVisibility,
					base_layer: l.isBaseLayer ? true : false,
					toc_link: l.toc_link ? l.toc_link : null,
				};

				// Check existing folders to see if this one already exists
				this.stageToc.layers.forEach((folder: types.tocGroupLayer) => {
					if (folder.id == folderId) {
						existsFolderId = true;

						// If layer visibility is false, update folder visibility
						if (!layerVisibility) {
							folder.visible = layerVisibility;
						}
						// Add the layer to the folder
						folder.layer.push(tocLayer);
					}
				});
				if (!existsFolderId) {
					// If folder does NOT exist, create a new one
					const arrayTocLayer: Array<types.tocLayer> = [];
					arrayTocLayer.push(tocLayer);

					// Check if the folder has switcher and if it activates only one layer at a time
					let hasSwitcher = true;
					let onlyOneVisibleLayer = false;
					let folderExpandedByDefault = false;
					let folderAlwaysExpanded = false;

					// eslint-disable-next-line
					optTocInfo.foldersOptions?.forEach((opt: any) => {
						if (
							this.getFolderName(
								stagingMap.getConfigTranslation(opt.folderName)
							) ==
							this.getFolderName(
								stagingMap.getConfigTranslation(l.urlFolder)
							)
						) {
							hasSwitcher = opt.hasSwitcher ?? false;
							onlyOneVisibleLayer =
								opt.onlyOneVisibleLayer ?? false;
							folderExpandedByDefault =
								opt.folderExpandedByDefault ?? false;
							folderAlwaysExpanded =
								opt.folderAlwaysExpanded ?? false;
						}
					});

					const tocGroupLayer: types.tocGroupLayer = {
						id: folderId,
						active: arrayTocLayer[0].active,
						visible: arrayTocLayer[0].visible,
						header: folderName,
						layer: arrayTocLayer,
						switcher: hasSwitcher,
						onlyOneVisibleLayer: onlyOneVisibleLayer,
						folderExpandedByDefault: folderExpandedByDefault,
						folderAlwaysExpanded: folderAlwaysExpanded,
					};

					this.stageToc.layers.push(tocGroupLayer);
				}
			}
			// If the config marks that the map has legend, load layers that have legend
			// IMPORTANT: Base layers are excluded from legends (!l.isBaseLayer)
			if (config.optionToc.legend && !l.isBaseLayer) {
				// The layer needs to be added to the legend
				const tocElement: types.tocElementLegend = {
					id: "el_leg_" + optToc.panelId + "-" + l.id,
					url: "",
					name: l.alias,
					active: l.legend ? true : false,
					defaultVisibility: l.defaultVisibility
						? l.defaultVisibility
						: false,
				};

				// Check if the legend folder already exists
				this.stageToc.groupLegend.forEach((folder, index) => {
					if (folder.id == "leg_" + folderId) {
						this.stageToc.groupLegend[index].legend.push(
							tocElement
						);
						// If the layer state is active in the legend, its folder will also be active
						if (tocElement.active) {
							this.stageToc.groupLegend[index].active = true;
						}
						existsFolderLegendId = true;
					}
				});

				if (!existsFolderLegendId) {
					const arr_toc_elem_legend: Array<types.tocElementLegend> =
						[];
					arr_toc_elem_legend.push(tocElement);
					// Create the folder
					const tocGroupLegend: types.tocGroupLegend = {
						id: "leg_" + folderId,
						name: folderName,
						active: tocElement.active,
						legend: arr_toc_elem_legend,
					};
					this.stageToc.groupLegend.push(tocGroupLegend);
				}
			}
		});
		this.createTocStructure();
	}

	/**
	 * This function creates HTML structure taking in account the stageToc generated in previous step
	 * @param {import('types').ownToc} stageToc - TOC configuration variable
	 * @method
	 */
	private createTocStructure(): void {
		// Reload folder visibility
		this.reloadFolderVisibility();

		// Sort the stageToc alphabetically by subfolder order
		this.orderStageTocBySubFolder();

		// Create main container
		const ownToc_container = document.createElement("div");
		ownToc_container.id = this.stageToc.panelId;
		ownToc_container.classList.add("ui-toc-panel");
		ownToc_container.classList.add("theme-" + this.stageToc.theme);

		//Add class active if it´s always open
		if (this.config.optionToc?.tocAlwaysOpen) {
			ownToc_container.classList.add("active");
		}

		// Clean the container and assign the ownToc_container
		const container = document.getElementById(this.stageToc.id);

		if (!container) {
			return;
		}

		container.innerHTML = "";
		container?.appendChild(ownToc_container);

		// Create header: only layers or layers + legend
		const hasLegend = this.stageToc.groupLegend.length ? true : false;

		// Set max height based on configuration
		this.setHeightOwnToc(ownToc_container, this.stageToc);

		// Create header container
		const tocTabsElement: HTMLElement = this.loadOwnTocHeader(hasLegend);
		const ownToc_header = document.createElement("div");
		ownToc_header.id = this.stageToc.panelId + "-ownTocHeader";
		ownToc_header.appendChild(tocTabsElement);
		ownToc_header.classList.add("ownTocHeader");
		ownToc_container?.appendChild(ownToc_header);

		// Create body container
		const ownToc_bodyText: string = this.loadOwnTocBody();
		const ownToc_body = document.createElement("div");
		ownToc_body.id = this.stageToc.panelId + "-ownTocBody";
		ownToc_body.classList.add("ui-toc-panel-body", "body-layers");

		//Creates a search input and appends it to the ownToc_container.
		if (this.stageToc.searchLayersFilter) {
			this.createSearchInput(ownToc_container);
		}

		ownToc_body.innerHTML += ownToc_bodyText;
		ownToc_container?.appendChild(ownToc_body);
		document.getElementById(
			this.stageToc.panelId + "-ownTocBodyFolderContainer"
		).style.display = "block";

		// Event handler for folder expansion to show/hide layers
		document
			.querySelectorAll(
				"#" + this.stageToc.panelId + " .ownTocBodyFolderElem"
			)
			.forEach((item) => {
				//si el atributo folder-always-open es "true",no añadimos evento de expandir/contrair la carpeta
				const folderAlwaysOpen =
					item.getAttribute("folder-always-open");
				if (folderAlwaysOpen !== "true") {
					item.addEventListener("click", () => {
						owntocJs.displayLayersList(item.id);
					});
				}
			});

		// Event handler for layer checkboxes
		document
			.querySelectorAll(
				"#" + this.stageToc.panelId + " .ownTocBodyChBoxLayer"
			)
			.forEach((item) => {
				item.addEventListener("click", () => {
					this.stageToc = owntocJs.enableLayer(
						item.id,
						this.goMap,
						this.stageToc
					);

					const isParentChecked = document.getElementById(
						item.id
					).checked;

					// Get the first parent .ownTocBodyLayersElem of the checkbox (item)
					const parentLayerElem = item.closest(
						".ownTocBodyLayersElem"
					);
					if (parentLayerElem) {
						// Look inside the parent for submenu existence
						const submenu = parentLayerElem.querySelector(
							".ownTocSubmenuBodyLayersCont"
						);
						if (submenu) {
							// Iterate through all submenu checkboxes and activate/deactivate according to parent state
							const checkboxes =
								submenu.querySelectorAll(".checkbox-submenu");
							checkboxes.forEach((checkbox) => {
								if (checkbox.checked !== isParentChecked) {
									checkbox.checked = isParentChecked;

									// Manually trigger the event to apply filters as in the original handler
									checkbox.dispatchEvent(
										new Event("click", {
											bubbles: true,
										})
									);
								}
							});
						}
					}
				});
			});

		// Event handler for switcher to activate/deactivate layers in a folder
		document
			.querySelectorAll(
				"#" + this.stageToc.panelId + " .ownTocBodyFolderSwitcher"
			)
			.forEach((item) => {
				item.addEventListener("mousedown", () => {
					const folderElem = item.closest(".ownTocBodyFolderElem");
					if (!folderElem) return;
					this.stageToc = owntocJs.switchLayersList(
						folderElem.id,
						this.goMap,
						this.stageToc
					);

					MapEventManager.events.fire(
						GoOwnTocEvents.LAYER_CHANGE_VISIBILITY,
						{
							id: folderElem.id
								.split(this.stageToc.panelId + "-")
								.pop(),
							type: "FOLDER",
							mapId: this.stageToc.mapId,
							state: this.getEventTOCState(this.stageToc),
						}
					);
				});
			});

		// Event handler for expanding TOC submenu of layers in the TOC
		ownToc_body
			.querySelectorAll(".toc-submenu-toggle")
			.forEach((toggle) => {
				toggle.addEventListener("click", (e) => {
					e.preventDefault(); // Prevent checkbox from changing state when clicking the submenu button
					e.stopPropagation(); // Same but for the label

					const subfolderId = (toggle as HTMLElement).dataset
						.subfolderId;
					const submenu = ownToc_container.querySelector(
						`#toc-layer-submenu-${subfolderId}`
					);

					if (submenu) {
						owntocJs.displaySubmenuList(submenu, toggle);
					}
				});
			});

		//Evento sobre checkbox submenu
		document.querySelectorAll(".ownTocSubmenuChBox").forEach((itemDiv) => {
			const checkbox = itemDiv.querySelector(".checkbox-submenu");
			if (checkbox) {
				checkbox.addEventListener("click", (e) => {
					e.stopPropagation();

					const parent = itemDiv.closest(".ownTocBodyLayersCont");
					const layerId = itemDiv.getAttribute("layer-id");

					// Obtenemos capa del mapa
					const layerFromMap = this.goMap
						.getStaging(this.config.id)
						.getLayer(layerId);

					// Filtramos solo los checkboxes que pertenecen a esta capa
					const allCheckboxes = parent?.querySelectorAll(
						`.ownTocSubmenuChBox[layer-id="${layerId}"] .checkbox-submenu`
					);

					const objFilter = [];
					allCheckboxes?.forEach((cb) => {
						if (cb.checked) {
							const cbItemDiv = cb.closest(".ownTocSubmenuChBox");
							const objStyle = JSON.parse(
								cbItemDiv.getAttribute("data-style")
							);

							if (objStyle) {
								const filter = {
									field: objStyle.field,
									condition: objStyle.type,
									value: objStyle.value,
								};
								objFilter.push(filter);
							}
						}
					});

					//borramos los filtros
					layerFromMap.removeFilterNoCQL();

					const checkBoxMapLayer = document.querySelector(
						`#ownTocContainer_checklayer_${layerFromMap.layerId}`
					);

					if (objFilter.length > 0) {
						//habilitamos capa del mapa y activamos checkbox
						layerFromMap.setVisibility(true);

						MapEventManager.events.fire(
							GoOwnTocEvents.LAYER_CHANGE_VISIBILITY,
							{
								layerId: layerFromMap.layerId,
								visible: true,
								mapId: this.stageToc.mapId,
								state: this.getEventTOCState(this.stageToc),
							}
						);

						if (checkBoxMapLayer) {
							checkBoxMapLayer.checked = true;
						}

						// Create the filter with OR logic
						const filterWithOR = {
							...objFilter[0],
							or: objFilter.slice(1),
						};
						layerFromMap.addFilterNoCQL(filterWithOR);
					} else {
						//si no hay filtro ponemos la capa invisible y desactivamos checkbox
						layerFromMap.setVisibility(false);

						MapEventManager.events.fire(
							GoOwnTocEvents.LAYER_CHANGE_VISIBILITY,
							{
								layerId: layerFromMap.layerId,
								visible: false,
								mapId: this.stageToc.mapId,
								state: this.getEventTOCState(this.stageToc),
							}
						);

						if (checkBoxMapLayer) checkBoxMapLayer.checked = false;

						//actualizamos el estado del stageTOC
						this.stageToc = owntocJs.enableLayer(
							`ownTocContainer_checklayer_${layerFromMap.layerId}`,
							this.goMap,
							this.stageToc
						);
					}
				});
			}
		});

		if (hasLegend) {
			// Generate legend containers
			const ownLegend_body = document.createElement("div");
			ownLegend_body.id = this.stageToc.panelId + "-ownLegendBody";
			ownLegend_body.classList.add("ui-toc-panel-body", "body-legend");
			ownLegend_body.innerHTML =
				'<div class="ownLegendBodyContainer" id="' +
				this.stageToc.panelId +
				'-ownLegendBodyContainer"></div>';
			ownLegend_body.style.display = "none";

			ownToc_container?.appendChild(ownLegend_body);

			if (this.config.optionToc?.legendOpenByDefault) {
				ownLegend_body.style.display = "block";
			}

			if (this.customLegend) {
				this.customLegend.replaceLegend();
			} else {
				// Load legend content
				owntocJs.loadLegendStructure(this.stageToc, this.goMap);
			}

			//if legend open by default
			if (this.config.optionToc?.legendOpenByDefault) {
				//hide layers content
				ownToc_body.style.display = "none";
				//hide search input for legend content
				const tocSearch = ownToc_container.querySelector(
					".toc-search-container"
				);
				if (tocSearch) {
					tocSearch.style.display = "none";
				}
			}
		}

		// Event handling for TOC tabs
		document
			.querySelectorAll("#" + this.stageToc.panelId + " .toc-tab")
			.forEach((item) => {
				item.addEventListener("click", () => {
					owntocJs.enableFlap(item.id, this.stageToc.panelId);
				});
			});
	}

	/**
	 * This function returns the folder id of any layer defined in config file
	 * @param {string} urlFolder - URL of folder defined in config file
	 * @method
	 */
	private getFolderId(urlFolder: string): string {
		const arrFolder: Array<string> = urlFolder.split("/");
		let textFolderId = "";
		textFolderId = this.getTextFolderId(arrFolder[0]);
		return textFolderId;
	}

	private getEventTOCState(stageToc: types.ownToc) {
		const state: { layer: string; visible: boolean }[] = [];

		let waterMarkEsriEnabled = false;
		const waterMarkEsri = document.querySelector(
			"#watermark-basemap"
		) as HTMLDivElement;
		if (waterMarkEsri) {
			waterMarkEsri.style.visibility = "hidden";
		}

		stageToc.layers.forEach((folder) => {
			folder.layer.forEach((layer) => {
				const layerState = {
					layer: layer.id,
					visible: layer.visible,
				};

				//If any ESRI basemap is enabled, show the watermark
				if (layer.layerType === "GO_ESRI_BASEMAP" && layer.visible) {
					waterMarkEsriEnabled = true;
				}

				state.push(layerState);
			});
		});

		if (waterMarkEsri && waterMarkEsriEnabled) {
			(
				document.querySelector("#watermark-basemap") as HTMLDivElement
			).style.visibility = "visible";
		}

		return state;
	}

	/**
	 * This function returns the folder name of any layer defined in config file
	 * @param {string} urlFolder - URL of folder defined in config file
	 * @method
	 */
	private getFolderName(urlFolder: string) {
		let arrFolder: Array<string> = urlFolder.split("/");
		arrFolder = this.cleanArrFolder(arrFolder);
		return arrFolder[0];
	}

	/**
	 * This function creates the id string
	 * @param {string} txtfolder - Name of folder defined in config file
	 * @method
	 */
	private getTextFolderId(txtfolder: string): string {
		const txtFolderId: string = txtfolder.toLowerCase().replace(/ /g, "-");
		return txtFolderId;
	}

	/**
	 * This function returns the subfolder name of any layer defined in config file
	 * @param {string} urlFolder - URL of folder defined in config file
	 * @method
	 */
	private getSubFolderName(urlFolder: string) {
		let subfolderName = "";
		let arrFolder: Array<string> = urlFolder.split("/");
		arrFolder = this.cleanArrFolder(arrFolder);
		if (arrFolder.length > 1) {
			subfolderName = arrFolder[arrFolder.length - 1];
		}
		return subfolderName;
	}

	/**
	 * This function creates the id string for any subfolder
	 * @param {string} subFolderName - Name of subfolder defined in config file
	 * @method
	 */
	private getSubFolderId(subFolderName: string | null) {
		let subfolderId = "";
		if (subFolderName) {
			subfolderId = "subf_" + this.getTextFolderId(subFolderName);
		}
		return subfolderId;
	}

	/**
	 * This function returns a cleaned array without empty elements
	 * @param {Array<string>} arrFolder - Array of URL splitted by '/'
	 * @method
	 */
	private cleanArrFolder(arrFolder: Array<string>): Array<string> {
		const cleanedArray: Array<string> = [];
		arrFolder.forEach((folder) => {
			if (folder) {
				cleanedArray.push(folder);
			}
		});
		return cleanedArray;
	}

	/**
	 * This function creates HTML structure of TOC Header
	 * @param {boolean} hasLegend - Flag to indicate if TOC also shows Legends
	 * @method
	 */
	private loadOwnTocHeader(hasLegend: boolean): HTMLElement {
		const tocTabs = document.createElement("div");
		tocTabs.className = "toc-tabs";

		const tabLayers = document.createElement("div");

		tabLayers.className =
			this.config.optionToc?.legendOpenByDefault &&
			this.config.optionToc.legend
				? "toc-tab"
				: "toc-tab tab-active";
		tabLayers.id = `${this.stageToc.panelId}-ownTocHeaderLayers`;
		tabLayers.textContent = i18nService.getTranslation("toc.header.layers");
		tocTabs.appendChild(tabLayers);

		if (hasLegend) {
			const tabLegends = document.createElement("div");
			tabLegends.className =
				this.config.optionToc?.legendOpenByDefault &&
				this.config.optionToc.legend
					? "toc-tab tab-active"
					: "toc-tab";
			tabLegends.id = `${this.stageToc.panelId}-ownTocHeaderLegends`;
			tabLegends.textContent =
				i18nService.getTranslation("toc.header.legend");
			tocTabs.appendChild(tabLegends);
		}

		return tocTabs;
	}

	/**
	 * This function creates HTML structure of TOC Body
	 * @method
	 */
	private loadOwnTocBody(): string {
		const container = document.createElement("div");
		container.className = "ownTocBodyContainer";
		container.id = `${this.stageToc.panelId}-ownTocBodyFolderContainer`;

		const svgPlusIcon =
			'<svg width="13" height="13" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><line x1="50" y1="20" x2="50" y2="80" stroke-width="15"/><line x1="20" y1="50" x2="80" y2="50" stroke-width="15"/></svg>';

		const svgMinusIcon =
			'<svg width="13" height="13" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><line x1="20" y1="50" x2="80" y2="50" stroke-width="15"/></svg>';

		let code = "";

		const stagingMap = this.goMap.getStaging(this.config.id);

		if (!stagingMap) {
			console.error(
				`GoOwnToc: No staging found for map with id ${this.config.id}`
			);
			return code;
		}
		this.stageToc.layers.forEach((folder) => {
			const folderActiveClass = folder.visible ? true : false;

			const folderName = stagingMap.getConfigTranslation(folder.header);
			const contFolder = document.createElement("div");
			contFolder.className = "ownTocBodyContFolder";

			const folderElem = document.createElement("div");
			folderElem.className = "ownTocBodyFolderElem";
			folderElem.id = folder.id;
			folderElem.setAttribute(
				"onlyOneVisibleLayer",
				folder.onlyOneVisibleLayer
			);
			const folderNameDiv = document.createElement("div");
			folderNameDiv.className = "ownTocBodyFolderName";
			folderNameDiv.innerText = folderName;
			folderElem.appendChild(folderNameDiv);

			if (folder.switcher) {
				const rightGroup = document.createElement("div");
				rightGroup.className = "right-group";

				const switcherDiv = document.createElement("div");
				switcherDiv.className = "ownTocBodyFolderSwitcher";

				const label = document.createElement("label");
				label.className = "switch";

				const input = document.createElement("input");
				input.type = "checkbox";
				input.id = `checkbox-${folder.id}`;
				if (folderActiveClass) {
					input.setAttribute("checked", "");
				}

				const spanSlider = document.createElement("span");
				spanSlider.className = "slider round";

				label.appendChild(input);
				label.appendChild(spanSlider);
				switcherDiv.appendChild(label);
				rightGroup.appendChild(switcherDiv);
				if (folder.folderAlwaysExpanded) {
					folderElem.setAttribute("folder-always-open", "true");
				} else {
					const toggleDiv = document.createElement("div");
					toggleDiv.className = "ui-toc-toggle";
					if (this.config.optionToc?.allFoldersExpanded) {
						toggleDiv.innerHTML = svgMinusIcon;
						toggleDiv.classList.add("toggle-opened");
					} else if (folder.folderExpandedByDefault) {
						toggleDiv.innerHTML = svgMinusIcon;
						toggleDiv.classList.add("toggle-opened");
					} else {
						toggleDiv.innerHTML = svgPlusIcon;
					}
					rightGroup.appendChild(toggleDiv);
				}
				folderElem.appendChild(rightGroup);
			} else {
				if (folder.folderAlwaysExpanded) {
					folderElem.setAttribute("folder-always-open", "true");
				} else {
					const toggleDiv = document.createElement("div");
					toggleDiv.className = "ui-toc-toggle";
					if (this.config.optionToc?.allFoldersExpanded) {
						toggleDiv.innerHTML = svgMinusIcon;
						toggleDiv.classList.add("toggle-opened");
					} else if (folder.folderExpandedByDefault) {
						toggleDiv.innerHTML = svgMinusIcon;
						toggleDiv.classList.add("toggle-opened");
					} else {
						toggleDiv.innerHTML = svgPlusIcon;
					}
					folderElem.appendChild(toggleDiv);
				}
			}

			contFolder.appendChild(folderElem);

			const layersCont = document.createElement("div");
			layersCont.className = "ownTocBodyLayersCont";

			//comprobar si existe allFoldersExpanded y poner display block
			if (
				this.config.optionToc?.allFoldersExpanded ||
				folder.folderAlwaysExpanded
			) {
				layersCont.style.display = "block";
			}

			let lastSubFolderId = "";
			const sizeLayers = folder.layer.length;

			folder.layer.forEach((layer, index) => {
				const checked = layer.visible ? "checked" : "";
				const nameDisplay = stagingMap.getConfigTranslation(layer.name);

				if (folder.folderExpandedByDefault) {
					layersCont.style.display = "block";
				}

				// No subfolder
				if (layer.subfolder_id === "" && layer.active) {
					code +=
						'<div class="ownTocBodyLayersElem" id="' +
						layer.id +
						'">' +
						'<label class="checkbox__toc-container">' +
						'<input class="ownTocBodyChBoxLayer" type="checkbox" ' +
						checked +
						' id="' +
						this.stageToc.panelId +
						"_checklayer_" +
						layer.id +
						'"/>' +
						'<span class="custom-eyes-icon">' +
						svgOpenedEye +
						svgClosedEye +
						"</span>" +
						'<span class="label-text">' +
						nameDisplay +
						"</span>" +
						"</label>" +
						"</div>";
				}

				// With subfolder (same as last)
				if (
					layer.subfolder_id &&
					layer.subfolder_id === lastSubFolderId &&
					layer.active
				) {
					code +=
						'<div class="ownTocBodyLayersElem" id="' +
						layer.id +
						'">' +
						'<label class="checkbox__toc-container' +
						(layer.filterTocSubmenu ? " with-submenu" : "") +
						'" for="' +
						this.stageToc.panelId +
						"_checklayer_" +
						layer.id +
						'">' +
						'<input class="ownTocBodyChBoxLayer" type="checkbox" ' +
						checked +
						' id="' +
						this.stageToc.panelId +
						"_checklayer_" +
						layer.id +
						'"/>' +
						'<span class="custom-eyes-icon">' +
						svgOpenedEye +
						svgClosedEye +
						"</span>" +
						'<span class="label-text">' +
						nameDisplay +
						"</span>" +
						(layer.filterTocSubmenu
							? `<div class="toc-submenu-toggle"
						id="submenu-toggle-${layer.subfolder_id}-${index}"
						data-subfolder-id="${layer.subfolder_id}-${index}">` +
							  svgPlusIcon +
							  "</div>"
							: "") +
						"</label>";
					if (layer.filterTocSubmenu) {
						code +=
							'<div class="ownTocSubmenuBodyLayersCont" id="toc-layer-submenu-' +
							`${layer.subfolder_id}-${index}` +
							'">';
						code += this.getSubmenuContent(layer).outerHTML;
						code += "</div>";
					}
					code += "</div>";
				}

				if (
					lastSubFolderId &&
					layer.subfolder_id !== lastSubFolderId &&
					layer.active
				) {
					code += "</div>";
				}

				if (
					layer.subfolder_id &&
					layer.subfolder_id !== lastSubFolderId &&
					layer.active
				) {
					code +=
						'<div class="ownTocBodySubFolderElem" id="' +
						layer.subfolder_id +
						'">';
					if (layer.subfolder) {
						code +=
							'<div class="ownTocBodySubFolderTitle">' +
							layer.subfolder +
							"</div>";
					}
					code +=
						'<div class="ownTocBodyLayersElem" id="' +
						layer.id +
						'">' +
						'<label class="checkbox__toc-container' +
						(layer.filterTocSubmenu ? " with-submenu" : "") +
						'" for="' +
						this.stageToc.panelId +
						"_checklayer_" +
						layer.id +
						'">' +
						'<input class="ownTocBodyChBoxLayer" type="checkbox" ' +
						checked +
						' id="' +
						this.stageToc.panelId +
						"_checklayer_" +
						layer.id +
						'"/>' +
						'<span class="custom-eyes-icon">' +
						svgOpenedEye +
						svgClosedEye +
						"</span>" +
						'<span class="label-text">' +
						nameDisplay +
						"</span>" +
						(layer.filterTocSubmenu
							? `<div class="toc-submenu-toggle"
						id="submenu-toggle-${layer.subfolder_id}-${index}"
						data-subfolder-id="${layer.subfolder_id}-${index}">` +
							  svgPlusIcon +
							  "</div>"
							: "") +
						"</label>";

					if (layer.filterTocSubmenu) {
						code +=
							'<div class="ownTocSubmenuBodyLayersCont" id="toc-layer-submenu-' +
							`${layer.subfolder_id}-${index}` +
							'">';
						code += this.getSubmenuContent(layer).outerHTML;
						code += "</div>";
					}

					code += "</div>";
				}

				if (
					layer.subfolder_id &&
					index === sizeLayers - 1 &&
					layer.active
				) {
					code += "</div>";
				}

				lastSubFolderId = layer.subfolder_id;
			});

			const tempWrapper = document.createElement("div");
			tempWrapper.innerHTML = code;
			while (tempWrapper.firstChild) {
				layersCont.appendChild(tempWrapper.firstChild);
			}

			contFolder.appendChild(layersCont);
			container.appendChild(contFolder);
			code = "";
		});

		return container.outerHTML;
	}

	private getSubmenuContent(layer): HTMLElement {
		const layerOpts = this.config.layer?.find(
			(elem) => elem.id === layer.id
		);
		const style = layerOpts?.styleSVGConditional || layerOpts?.styleGeojson;
		if (!style) return document.createElement("div");

		const stagingMap = this.goMap.getStaging(this.config.id);

		if (!stagingMap) {
			console.error(
				`GoOwnToc: No staging found for map with id ${this.config.id}`
			);
			return document.createElement("div");
		}

		const field = style?.field;
		const container = document.createElement("div");
		const processStyle = (styleInfo: any[]) => {
			styleInfo.forEach((elem, index) => {
				if (elem.condition) {
					//default no legendAlias
					const conditionName = this.getConditionName(
						elem.condition.type
					);
					let conditionText = `${field} ${conditionName} ${elem.condition.condition}`;

					//if exist legendAlias
					if (elem.legendAlias) {
						conditionText = stagingMap.getConfigTranslation(
							elem.legendAlias
						);
					}

					const elemWithField = {
						type: elem.condition.type,
						value: elem.condition.condition,
						field: field,
					};

					const chBoxDiv = document.createElement("div");
					chBoxDiv.className = "ownTocSubmenuChBox";
					chBoxDiv.setAttribute(
						"data-style",
						JSON.stringify(elemWithField)
					);
					chBoxDiv.setAttribute("layer-id", layer.id);

					const label = document.createElement("label");
					label.className = "checkbox__toc-container";

					const input = document.createElement("input");
					input.className = "checkbox-submenu";
					input.type = "checkbox";
					input.id = `${layer.id}-submenu-checkbox-${index}`;
					input.checked = true;
					input.setAttribute("checked", "checked");
					const eyesIcon = document.createElement("span");
					eyesIcon.className = "custom-eyes-icon";
					eyesIcon.innerHTML = svgOpenedEye + svgClosedEye;

					const labelText = document.createElement("span");
					labelText.className = "label-text";
					labelText.textContent = conditionText;

					label.appendChild(input);
					label.appendChild(eyesIcon);
					label.appendChild(labelText);
					chBoxDiv.appendChild(label);
					container.appendChild(chBoxDiv);
				}
			});
		};

		if (layerOpts.styleSVGConditional && style?.styleInfoSVG?.length > 0) {
			processStyle(style.styleInfoSVG);
		} else if (layerOpts.styleGeojson && style?.styleInfo?.length > 0) {
			processStyle(style.styleInfo);
		}

		return container;
	}

	/**
	 * Crea un campo de búsqueda en el contenedor TOC.
	 * Este campo permite filtrar dinámicamente las capas según el texto ingresado.
	 *
	 * La búsqueda:
	 * - Es insensible a mayúsculas/minúsculas y acentos.
	 * - Oculta/visualiza elementos `.ownTocBodyLayersElem` según coincidencia de texto.
	 * - También oculta títulos y subcarpetas si todos sus elementos hijos están ocultos.
	 * - Muestra un mensaje personalizado si no hay coincidencias visibles.
	 * - Muestra la cantidad de capas encontradas si hay traducciones configuradas.
	 * @private
	 * @param {HTMLDivElement} ownToc_container - El contenedor principal del TOC donde se inserta el buscador.
	 * @example
	 * this.createSearchInput(document.querySelector('#toc-container'));
	 */
	/**
	 * @private
	 * @param {HTMLDivElement} ownToc_container
	 */
	private createSearchInput(ownToc_container: HTMLDivElement) {
		const placeholder = i18nService.getTranslation(
			"toc.search.placeholder"
		);

		const tocSearch = document.createElement("div");
		tocSearch.className = "toc-search-container";

		const svgHTML = `
            <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" width="14" height="14" fill="currentColor" viewBox="0 0 16 16">
                <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85zm-5.242 1.156a5 5 0 1 1 0-10 5 5 0 0 1 0 10z"/>
            </svg>
            `;

		tocSearch.innerHTML = svgHTML;

		const input = document.createElement("input");
		input.type = "text";
		input.id = `${this.stageToc.panelId}-search-layers-input`;
		input.className = "search-layers-input";
		input.placeholder = placeholder;
		input.setAttribute("autocomplete", "off");

		tocSearch.appendChild(input);

		const resultsContainer = document.createElement("div");
		resultsContainer.innerHTML = `<span class="search-input-result-text"></span>`;
		tocSearch.appendChild(resultsContainer);

		ownToc_container.appendChild(tocSearch);

		ownToc_container
			.querySelector(`#${this.stageToc.panelId}-search-layers-input`)
			?.addEventListener("input", (event) =>
				this.handleSearchInput(event, ownToc_container)
			);
	}

	private handleSearchInput(event: Event, container: HTMLDivElement): void {
		const target = event.target as HTMLInputElement;
		const searchText = this.normalizeString(target.value);
		const resultText = container.querySelector(".search-input-result-text");

		if (resultText) {
			searchText
				? resultText.classList.add("visible")
				: resultText.classList.remove("visible");
		}

		const countVisibleLayers = this.filterLayersByText(
			container,
			searchText
		);
		this.updateSubFoldersVisibility(container);
		this.updateContainersEmptyMessage(container);
		this.updateResultCountText(resultText, countVisibleLayers);
	}

	private normalizeString(str: string): string {
		return str
			.toLowerCase()
			.normalize("NFD")
			.replace(/[\u0300-\u036f]/g, "");
	}

	private filterLayersByText(
		container: HTMLDivElement,
		searchText: string
	): number {
		let count = 0;
		container
			.querySelectorAll(".ownTocBodyLayersElem")
			.forEach((layer: HTMLElement) => {
				const textInputSearch = layer.querySelector(".label-text");
				const labelText = textInputSearch
					? this.normalizeString(textInputSearch.textContent || "")
					: "";

				if (labelText.includes(searchText)) {
					layer.style.display = "";
					count++;
				} else {
					layer.style.display = "none";
				}
			});
		return count;
	}

	private updateSubFoldersVisibility(container: HTMLDivElement): void {
		container
			.querySelectorAll(".ownTocBodySubFolderElem")
			.forEach((sub: HTMLElement) => {
				const layers = sub.querySelectorAll(".ownTocBodyLayersElem");
				const allChildHidden = Array.from(layers).every(
					(layer) => getComputedStyle(layer).display === "none"
				);
				const titleElem = sub.querySelector(
					".ownTocBodySubFolderTitle"
				) as HTMLElement;

				if (titleElem) {
					titleElem.style.display = allChildHidden ? "none" : "block";
					sub.style.display =
						getComputedStyle(titleElem).display === "none"
							? "none"
							: "block";
				}
			});
	}

	private updateContainersEmptyMessage(container: HTMLDivElement): void {
		container
			.querySelectorAll(".ownTocBodyLayersCont")
			.forEach((cont: HTMLElement) => {
				const somethingVisible = Array.from(
					cont.querySelectorAll(
						".ownTocBodyLayersElem, .ownTocBodySubFolderTitle"
					)
				).some((el) => getComputedStyle(el).display !== "none");

				let msg = cont.querySelector(".no-layers-msg") as HTMLElement;
				if (!somethingVisible) {
					if (!msg) {
						msg = document.createElement("div");
						msg.className = "no-layers-msg";
						msg.textContent = i18nService.getTranslation(
							"toc.search.noLayersMessage"
						);
						cont.appendChild(msg);
					}
					msg.style.display = "block";
				} else if (msg) {
					msg.style.display = "none";
				}
			});
	}

	private updateResultCountText(
		resultText: Element | null,
		count: number
	): void {
		if (!resultText) return;

		const translationKey =
			count === 1
				? "toc.search.singularResult"
				: "toc.search.pluralResult";
		resultText.textContent = `(${count} ${i18nService.getTranslation(
			translationKey
		)})`;
	}

	/**
	 * This function defines the maximum height of TOC container DIV regarding the config variable
	 * @param {HTMLDivElement} ownToc_container - Element HTML of TOC container
	 * @param {import('types').ownToc} stageToc - TOC configuration variable
	 * @method
	 */
	private setHeightOwnToc(
		ownToc_container: HTMLDivElement,
		stageToc: types.ownToc
	): void {
		ownToc_container.style.maxHeight = stageToc.maxHeight + "px";
	}

	/**
	 * This function orders the subfoders of the TOC by name
	 * @param {import('types').ownToc} stageToc - TOC configuration variable
	 * @method
	 */
	public orderStageTocBySubFolder(): void {
		this.stageToc.layers.forEach((folder) => {
			folder.layer.sort((a, b) => {
				if (a.subfolder !== b.subfolder) {
					return a.subfolder.localeCompare(b.subfolder);
				}
				return a.id.localeCompare(b.id);
			});
		});
	}

	/**
	 * Sets the visibility state of a specific layer in the TOC.
	 * This method updates the layer's visible property and optionally rebuilds the TOC structure.
	 *
	 * BEHAVIOR:
	 * - If the layer is already in the requested visibility state, no action is taken
	 * - If reload is true (default), the TOC structure will be rebuilt after the change
	 * - Emits a LAYER_CHANGE_VISIBILITY event after the operation
	 *
	 * @param {string} idLayer - ID of the layer to modify
	 * @param {boolean} visibility - New visibility state for the layer
	 * @param {boolean} reload - Whether to rebuild TOC structure after change (default: true)
	 * @method
	 */
	public setVisibility(
		idLayer: string,
		visibility: boolean,
		reload?: boolean
	): void {
		this.stageToc.layers.forEach((groupLayer) => {
			groupLayer.layer.forEach((lay) => {
				// If layer is already in the requested state, skip processing
				if (lay.id == idLayer && lay.visible == visibility) {
					return;
				} else if (lay.id == idLayer && lay.visible !== visibility) {
					// Update the layer's visibility state
					lay.visible = visibility;
				}
			});
		});

		// Rebuild TOC structure if reload is not explicitly set to false
		if (reload == null || reload) {
			this.createTocStructure();
		}

		// Emit visibility change event for external listeners
		MapEventManager.events.fire(GoOwnTocEvents.LAYER_CHANGE_VISIBILITY, {
			id: idLayer,
			type: "LAYER",
			mapId: this.stageToc.mapId,
			state: this.getEventTOCState(this.stageToc),
		});
	}

	/**
	 * Updates folder visibility based on the visibility state of their contained layers.
	 * A folder is visible only if ALL its layers are visible.
	 * This method is typically called after batch layer visibility changes.
	 *
	 * @method
	 */
	public reloadFolderVisibility() {
		this.stageToc.layers.forEach((groupLayer) => {
			let folderVisibility = true;
			groupLayer.layer.forEach((lay) => {
				if (!lay.visible) {
					folderVisibility = false;
				}
			});
			groupLayer.visible = folderVisibility;
		});
	}

	/**
	 * Sets the visibility state for multiple layers in a batch operation.
	 * This method is more efficient than calling setVisibility() multiple times
	 * as it only rebuilds the TOC structure once at the end.
	 *
	 * @param {Array<string>} idLayers - Array of layer IDs to modify
	 * @param {boolean} visibility - New visibility state for all specified layers
	 * @method
	 */
	public setVisibilityLayers(
		idLayers: Array<string>,
		visibility: boolean
	): void {
		if (idLayers !== null && idLayers.length) {
			idLayers.forEach((layer) => {
				this.setVisibility(layer, visibility, false);
			});
			this.createTocStructure();
		}
	}

	/**
	 * This function allows you get the language in TOC
	 * @method
	 */
	public getLanguage(): string {
		return this.stageToc.lang;
	}

	/**
	 * This function allows you specify the language in TOC
	 * @param {languages} idLanguage - id of the language for the TOC elements
	 * @method
	 */
	public setLanguage(idLanguage: string): void {
		this.stageToc.lang = idLanguage.toUpperCase();
		this.createStageToc(this.config);
		//si toc estaba abierto, quitar el fondo azul del boton del TOC
		const btnTOC = document.getElementById(`ui-btn-toc-${this.id}`);
		if (btnTOC && btnTOC.classList.contains("ui-btn-active")) {
			btnTOC.classList.remove("ui-btn-active");
		}
		this.createTocStructure();
	}

	/**
	 * This function allows you get the theme of TOC
	 * @method
	 */
	public getTheme(): constants.themes {
		return this.stageToc.theme;
	}

	/**
	 * This function allows you specify the theme in TOC
	 * @param {constants.themes} theme - id of the theme for the TOC
	 * @method
	 */
	public setTheme(theme: constants.themes): void {
		this.stageToc.theme = theme;
		//remove all theme-* layers
		const panel = document.getElementById(this.stageToc.panelId);
		this.removeClassByPrefix(panel, "theme-");
		//add class
		panel?.classList.add("theme-" + theme);
		// if (reloadToc) this.createTocStructure();
	}

	private removeClassByPrefix(node, prefix) {
		const regx = new RegExp("\\b" + prefix + "[^ ]*[ ]?\\b", "g");
		node.className = node.className.replace(regx, "");
		return node;
	}

	/**
	 * Removes a single layer from the TOC and its corresponding legend entry.
	 * This method handles both the layer visibility in the TOC structure and the legend cleanup.
	 *
	 * WORKFLOW:
	 * 1. Sets layer visibility to true (hide it visually)
	 * 2. Marks the layer as inactive in the TOC structure
	 * 3. Deactivates the layer's legend entry
	 * 4. Checks if the legend group should remain active (if other layers still have active legends)
	 * 5. Cleans up empty folders and rebuilds the TOC structure
	 *
	 * @param {string} idLayer - ID of the layer to remove from TOC and legend
	 * @method
	 */
	public removeLayer(idLayer: string): void {
		if (!idLayer) return;

		// Step 1: Find and deactivate the layer in TOC structure
		this.stageToc.layers.forEach((groupLayer) => {
			const layerSelected = groupLayer.layer.find(
				(layer) => layer.id === idLayer
			);
			if (layerSelected !== undefined) {
				// Hide the layer visually and mark as inactive
				this.setVisibility(idLayer, true, false);
				layerSelected.active = false;
			}
		});

		// Step 2: Handle legend cleanup
		this.stageToc.groupLegend.forEach((groupLegend) => {
			const layerLegend = groupLegend.legend.find(
				(legendItem) =>
					legendItem.id === `el_leg_ownTocContainer-${idLayer}`
			);
			if (layerLegend !== undefined) {
				// Deactivate this specific layer's legend entry
				layerLegend.active = false;

				// Check if there are other active legends in the same group
				// This prevents deactivating the entire legend group if other layers are still active
				const hasActiveLegends = groupLegend.legend.some(
					(legend) => legend.active && legend.id !== layerLegend.id
				);

				// Only deactivate the legend group if no other layers have active legends
				if (!hasActiveLegends) {
					const legendGroup = this.stageToc.groupLegend.find(
						(group) =>
							group.id ===
							`leg_ownTocContainer-${groupLegend.name.toLowerCase()}`
					);
					if (legendGroup !== undefined) {
						legendGroup.active = false;
					}
				}
			}
		});

		// Step 3: Clean up empty folders and rebuild TOC structure
		this.removeFolderFromToc(this.stageToc);
		this.createTocStructure();
	}

	/**
	 * Removes a specific layer from the legend only (not from the TOC structure).
	 * This method only affects the legend display, leaving the layer active in the TOC.
	 *
	 * @param {string} idLayer - ID of the layer to remove from legend display
	 * @method
	 */
	public removeLayerLegend(idLayer: string): void {
		const idPanel = this.stageToc.panelId;
		this.stageToc.groupLegend.forEach((groupLegend) => {
			const layerSelected = groupLegend.legend.find(
				(layer) => layer.id === "el_leg_" + idPanel + "-" + idLayer
			);
			if (layerSelected !== undefined && layerSelected.active == true) {
				layerSelected.active = false;
				this.createTocStructure();
			}
		});
	}

	/**
	 * Removes multiple layers from the TOC structure in a batch operation.
	 * This method is more efficient than calling removeLayer() multiple times.
	 * Note: This method does NOT automatically rebuild the TOC structure -
	 * you should call createTocStructure() after if needed.
	 *
	 * @param {Array<string>} idLayers - Array of layer IDs to remove from TOC
	 * @method
	 */
	public removeLayers(idLayers: Array<string>): void {
		if (idLayers !== null && idLayers.length) {
			idLayers.forEach((idLayer) => {
				this.stageToc.layers.forEach((groupLayer) => {
					const layerSelected = groupLayer.layer.find(
						(layer) => layer.id === idLayer
					);
					if (layerSelected !== undefined) {
						this.setVisibility(idLayer, true, false);
						layerSelected.active = false;
					}
				});
			});
		}
	}

	/**
	 * Removes multiple layers from the legend display in a batch operation.
	 * This affects only the legend visibility, not the TOC structure.
	 *
	 * @param {Array<string>} idLayers - Array of layer IDs to remove from legend
	 * @method
	 */
	public removeLayersLegend(idLayers: Array<string>): void {
		if (idLayers !== null && idLayers.length) {
			const idPanel = this.stageToc.panelId;
			idLayers.forEach((idLayer) => {
				this.stageToc.groupLegend.forEach((groupLegend) => {
					const layerSelected = groupLegend.legend.find(
						(layer) =>
							layer.id === "el_leg_" + idPanel + "-" + idLayer
					);
					if (
						layerSelected !== undefined &&
						layerSelected.active == true
					) {
						layerSelected.active = false;
					}
				});
			});
			this.createTocStructure();
		}
	}

	/**
	 * Adds a single layer back to the TOC and activates its legend entry.
	 * This method is typically used to restore a previously removed layer.
	 *
	 * WORKFLOW:
	 * 1. Activates the layer's legend entry if it exists
	 * 2. Finds the layer in the TOC structure and marks it as active
	 * 3. Sets the layer visibility and rebuilds the TOC structure
	 *
	 * @param {string} idLayer - ID of the layer to add back to the TOC
	 * @method
	 */
	public addLayer(idLayer: string): void {
		// Step 1: Activate the layer's legend entry
		this.stageToc.groupLegend.forEach((groupLegend) => {
			const layerLegend = groupLegend.legend.find(
				(layerLegend) =>
					layerLegend.id === `el_leg_ownTocContainer-${idLayer}`
			);
			if (layerLegend) layerLegend.active = true;
		});

		// Step 2: Find and activate the layer in TOC structure
		this.stageToc.layers.forEach((groupLayer) => {
			const layerSelected = groupLayer.layer.find(
				(layer) => layer.id === idLayer
			);
			if (layerSelected) {
				// Set visibility and mark as active
				this.setVisibility(idLayer, true, false);
				layerSelected.active = true;
			}
		});

		// Step 3: Rebuild the TOC structure to reflect changes
		this.createTocStructure();
	}

	/**
	 * Adds a specific layer to the legend display only.
	 * If the layer is found in the legend and is currently inactive, it will be activated
	 * and the entire layer will be added back to the TOC.
	 *
	 * @param {string} idLayer - ID of the layer to add to legend display
	 * @method
	 */
	public addLayerLegend(idLayer: string): void {
		const idPanel = this.stageToc.panelId;
		this.stageToc.groupLegend.forEach((groupLegend) => {
			const layerSelected = groupLegend.legend.find(
				(layer) => layer.id === "el_leg_" + idPanel + "-" + idLayer
			);
			if (layerSelected !== undefined && layerSelected.active == false) {
				layerSelected.active = true;
				this.addLayer(idLayer);
				this.createTocStructure();
			}
		});
	}

	/**
	 * Adds multiple layers back to the TOC structure in a batch operation.
	 * This method is more efficient than calling addLayer() multiple times.
	 * Note: This method does NOT automatically rebuild the TOC structure -
	 * you should call createTocStructure() after if needed.
	 *
	 * @param {Array<string>} idLayers - Array of layer IDs to add back to TOC
	 * @method
	 */
	public addLayers(idLayers: Array<string>): void {
		if (idLayers !== null && idLayers.length) {
			idLayers.forEach((idLayer) => {
				this.stageToc.layers.forEach((groupLayer) => {
					const layerSelected = groupLayer.layer.find(
						(layer) => layer.id === idLayer
					);
					if (layerSelected !== undefined) {
						this.setVisibility(idLayer, true, false);
						layerSelected.active = true;
					}
				});
			});
		}
	}

	/**
	 * Adds a new layer to the TOC and updates the configuration.
	 * @param {types.layerOptions} layerConfig - The configuration object for the new layer to be added.
	 * @returns {void}
	 */
	public addNewLayer(layerConfig: types.layerOptions): void {
		const newConfig: types.GOMapOptions = this.config;

		// Remove layer if exist
		newConfig.layer = newConfig.layer.filter(
			(layer) => layer.id !== layerConfig.id
		);

		newConfig.layer.push(layerConfig);
		this.config = newConfig;
		this.createStageToc(this.config);
	}

	/**
	 * This function allows you add a group of layers from the legend
	 * @param {Array<string>} idLayers - Array of layers to add
	 * @method
	 */
	public addLayersLegend(idLayers: Array<string>): void {
		if (idLayers !== null && idLayers.length) {
			const idPanel = this.stageToc.panelId;
			idLayers.forEach((idLayer) => {
				this.stageToc.groupLegend.forEach((groupLegend) => {
					const layerSelected = groupLegend.legend.find(
						(layer) =>
							layer.id === "el_leg_" + idPanel + "-" + idLayer
					);
					if (
						layerSelected !== undefined &&
						layerSelected.active == false
					) {
						layerSelected.active = true;
						this.addLayer(idLayer);
					}
				});
			});
			this.createTocStructure();
		}
	}

	/**
	 * This function reload the toc from original config
	 * @method
	 */
	public reload(config): void {
		this.createStageToc(config);
	}

	private getConditionName(conditionName: string): string {
		let code: string;
		switch (conditionName) {
			case "EQUAL TO":
				code = i18nService.getTranslation("conditions.EQUAL_TO");
				break;
			case "NOT EQUAL TO":
				code = i18nService.getTranslation("conditions.NOT_EQUAL_TO");
				break;
			case "LESS THAN":
				code = i18nService.getTranslation("conditions.LESS_THAN");
				break;
			case "LESS THAN OR EQUAL TO":
				code = i18nService.getTranslation(
					"conditions.LESS_THAN_OR_EQUAL_TO"
				);
				break;
			case "GREATER THAN":
				code = i18nService.getTranslation("conditions.GREATER_THAN");
				break;
			case "GREATER THAN OR EQUAL TO":
				code = i18nService.getTranslation(
					"conditions.GREATER_THAN_OR_EQUAL_TO"
				);
				break;
			case "BETWEEN":
				code = i18nService.getTranslation("conditions.BETWEEN");
				break;
			case "NOT BETWEEN":
				code = i18nService.getTranslation("conditions.NOT_BEETWEEN");
				break;
			case "IS NULL":
				code = i18nService.getTranslation("conditions.IS_NULL");
				break;
			case "IS NOT NULL":
				code = i18nService.getTranslation("conditions.IS_NOT_NULL");
				break;
			case "IS EMPTY":
				code = i18nService.getTranslation("conditions.IS_EMPTY");
				break;
			case "IS NOT EMPTY":
				code = i18nService.getTranslation("conditions.IS_NOT_EMPTY");
				break;
			case "IS IN":
				code = i18nService.getTranslation("conditions.IS_IN");
				break;
			case "IS NOT IN":
				code = i18nService.getTranslation("conditions.IS_NOT_IN");
				break;
			case "IS LIKE":
				code = i18nService.getTranslation("conditions.IS_LIKE");
				break;
			case "IS NOT LIKE":
				code = i18nService.getTranslation("conditions.IS_NOT_LIKE");
				break;
			default:
				code = conditionName;
				break;
		}
		return code;
	}

	/**
	 * Creates a custom legend for the map and associates it with the TOC.
	 * @param {string} mapID - The ID of the map for which the custom legend is created.
	 * @param {HTMLDivElement} customLegendElement - The HTML div element that represents the custom legend.
	 * @returns {GoCustomLegend} The created `GoCustomLegend` instance.
	 */
	public createCustomLegend(
		mapID: string,
		customLegendElement: HTMLDivElement
	) {
		this.customLegend = new GoCustomLegend(
			this.goMap,
			mapID,
			customLegendElement
		);
		return this.customLegend;
	}

	/**
	 * Creates a custom legend for the map and associates it with the TOC.
	 * @param {string} mapID - The ID of the map for which the custom legend is created.
	 * @param {HTMLDivElement} customLegendElement - The HTML div element that represents the custom legend.
	 * @returns {GoCustomLegend} The created `GoCustomLegend` instance.
	 */
	public addItemCustomLegend(
		mapID: string,
		customLegendElement: HTMLDivElement
	) {
		this.customLegend = new GoCustomLegend(
			this.goMap,
			mapID,
			customLegendElement,
			false
		);
		return this.customLegend;
	}
}

---

FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Interactions\Features\interact\GetFeaturesByLayer.ts
 FILE NOT FOUND  This file does not exist at the specified path. Both Features/index.ts and Interactions/functions/getFeaturesByLayer.ts reference this non-existent file.
---
