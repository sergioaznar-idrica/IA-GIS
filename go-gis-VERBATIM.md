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
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Map\GoMap.ts
import { EventEmitter } from "@billjs/event-emitter";
import "@goTypes/index";
import * as types from "@goTypes/index";
import { GoInteractions } from "@interactions/index";
import {
	GOClusterLayer,
	GOGeoJsonLayer,
	GOLayer,
	GOVectorLayer,
	GOVectorTiles,
	GOVectorTilesOwn,
	GOWFSLayer,
	GOWMSLayer,
	GOWMTSLayer,
	GoEsriBaseMap,
	GoHeatMapLayer,
	GoHexGrid,
	GoHexGridLayer,
	GoIOTLayer,
	GoMapBoxLayer,
	GoRemoteSensing,
	GoWaterLeakRemoteSensing,
	GoTruckLayer,
	GoVerticalRemoteSensing,
	GoWebGlLayer,
	GoXYZ,
} from "@layers/index";
import i18nService from "@localization/index";
import { GOStagingMap, GoMapLoader, GoView } from "@map/index";
import { GoStore, GoAxiosServices } from "@services/index";
import * as constants from "@utils/classes/GoError/constants";
import * as internalErrosMsg from "@utils/classes/GoError/errrosMsg/internalErros.msg";
import { MAPS_ERROR_MAP_DONT_EXIST } from "@utils/classes/GoError/errrosMsg/mapsErrors.msg";
import * as goErrorsHandlers from "@utils/classes/GoError/goErrorsHandlers";
import {
	GOLayerEvents,
	GOLayerType,
	GOMapEvents,
	themes,
	typeOn,
} from "@utils/constants";
import "@utils/constants/index";
import { MapEventManager, hasBackendFeatures } from "@utils/index";
import { Map as OlMap } from "ol";
import PerspectiveMap from "ol-ext/map/PerspectiveMap";
import * as olCoordinate from "ol/coordinate";
import { Extent } from "ol/extent";
import GeoJSON from "ol/format/GeoJSON";
import MapProperty from "ol/MapProperty";
import { addProjection, get as getProjection } from "ol/proj";
import Projection from "ol/proj/Projection";
import { register } from "ol/proj/proj4";
import proj4 from "proj4";
import { GoIFCComponents } from "../Interactions/IFC";

/**
 * @classdesc
 * The map is the core component of Go-GIS. For a map to render, a view,
 * one or more layers, and a target container for create one map we needed:
 * ```ts
 *     const options = ' {
			"id": "main",
			"div": "olMap",
			"optionView": {
				"id": "vista1",
				"center": [-0.37739,39.46975],
				"zoom": 15,
				"projection": "4326"
			}
		}
		'
 *     const GoMap = new GoMap();
 *     GoMap.addMap(options)
 * ```
 *
 * The above snippet creates a map using a {@link module:types~mapOptionsBasic} to
 * display a personal map
 * @param {GoInteractions} interactions
 * @api
 */
export class GoMap {
	private stagingMap: Map<string, GOStagingMap>;
	public interactions: GoInteractions;
	private eventEmitter = new EventEmitter();
	private objInteraction: types.interactions | null = null;
	private __theme: themes = themes.DARK;

	constructor() {
		this.stagingMap = new Map();
		this.interactions = new GoInteractions(this.stagingMap);
	}

	/**
	 * This function creates a Map with a json configuration
	 * @param {import('types').mapOptionsBasic} options - json configuration
	 * @method
	 */

	public addMap(
		options: types.mapOptionsBasic,
		interactions: types.interactions | null,
		mapOpts: types.GOMapOptions
	): void {
		let mapView: GoView | null = null;
		let map: OlMap | PerspectiveMap | null = null;
		mapView = this.createView(options.optionView);

		this.objInteraction = interactions;
		if (interactions?.perspective?.active) {
			map = this.createPerspectiveMap(options.div, mapView);
		} else {
			map = this.createMap(options.div, mapView);
		}

		this.stagingMap.set(
			options.id,
			new GOStagingMap(
				options.id,
				map,
				mapView,
				this.interactions,
				mapOpts
			)
		);
	}

	public unmountMap(mapId: string) {
		const goMap = this.stagingMap.get(mapId);
		if (!goMap) {
			console.error(MAPS_ERROR_MAP_DONT_EXIST + mapId);
			this.emit(GOMapEvents.MAP_ERROR, {
				errorMsg: MAPS_ERROR_MAP_DONT_EXIST + mapId,
			});
			return;
		}
		goMap.unmount();
		GoAxiosServices.cancelPendingRequests(
			`Request canceled due to map unmount: ${mapId}`
		);
		this.eventEmitter.offAll();
		MapEventManager.events.offAll();
		// Remove the staging map from the map
		this.stagingMap.delete(mapId);
	}

	/**
	 * This function add to the map all the interactions defined in json configuration
	 * @param {import('types').interactions} interactions - json configuration
	 * @method
	 */

	public addInteractions(id: string, interactions: types.interactions): void {
		if (interactions.rotation && interactions.rotation?.active) {
			this.interactions.addRotation({
				idMap: id,
				idRotation: interactions.rotation.idInteraction,
			});
		}
		if (interactions.rotation && !interactions.rotation?.active) {
			this.interactions.disableRotation({ idMap: id });
		}
		if (interactions.closeTool && interactions.closeTool.active) {
			this.interactions.addExtraDataCloseToolInteractions({
				data: interactions.closeTool,
			});
		} else if (
			interactions.updownStream &&
			interactions.updownStream.active
		) {
			this.interactions.addExtraDataUpDownStreamInteractions({
				data: interactions.updownStream,
			});
		}
	}

	/**
	 * @param {import('types').projOptions} options This function creates a projection with a json configuration
	 * @method
	 */

	public createProjection(options: types.projOptions): void {
		if (getProjection(options.code) === null) {
			// Register with proj4 if definition is provided
			if (options.def) {
				proj4.defs(options.code, options.def);
				register(proj4);
			}

			// eslint-disable-next-line @typescript-eslint/no-explicit-any
			const projection = new Projection(options as any);
			addProjection(projection);
		} else {
			// Error Handler
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_ARGUMENT_NOT_FOUND,
				internalErrosMsg.INTERNAL_ERROR_ARGUMENT_NOT_FOUND +
					` ${options.code}, unable to generate requested projection.`
			);
			this.emit(GOMapEvents.MAP_ERROR, {
				error:
					internalErrosMsg.INTERNAL_ERROR_ARGUMENT_NOT_FOUND +
					` ${options.code}, unable to generate requested projection.`,
			});
		}
	}

	/**
	 * This function return interactions for this map
	 * @method
	 */

	public getInteractions() {
		return this.interactions;
	}

	/**
	 * This function allow to create a map passing the id of the div
	 * @param {string} div - The id of the div
	 * @param {import('types').GoView} view - The view of the map
	 * @returns {import('types').OlMap} Return a new map
	 * @method
	 */

	private createMap(div: string, view: GoView): OlMap {
		const olMap = new OlMap({ target: div });
		// olMap.setView(view);
		olMap.set(MapProperty.VIEW, view);
		return olMap;
	}

	/**
	 * This function allow to create a perspective map passing the id of the div
	 * @param {string} div - The id of the div
	 * @param {import('types').GoView} view - The view of the map
	 * @returns {import('types').OlMap} Return a new map
	 * @method
	 */

	private createPerspectiveMap(div: string, view: GoView): OlMap {
		return new PerspectiveMap({ target: div, view });
	}

	/**
	 * This function allow to create a view passing the id of the map.
	 * @param {import('types').optionsView} options - The options of the view
	 * @param {string} idMap - The id of the map
	 * @returns {import('types').GoView} Return a new view
	 * @method
	 */

	private createView(options: types.optionsView): GoView {
		const actualProjection = options.projection
			? `EPSG:${options.projection}`
			: "3857";

		return new GoView({
			id: options.id,
			center: options.center,
			zoom: options.zoom,
			projection: actualProjection,
			extent: options.extent !== null ? options.extent : undefined,
			rotation: options.rotation !== null ? options.rotation : undefined,
			maxZoom:
				typeof options.maxZoom === "number"
					? options.maxZoom
					: undefined,
			minZoom:
				typeof options.minZoom === "number"
					? options.minZoom
					: undefined,
			multiWorld: options.multiWorld ? options.multiWorld : false,
		});
	}

	/**
	 * This function allow to get the projection of the map
	 * @param {string} idMap This function creates a Map with a json configuration
	 * @returns {string} EPSG Return a Code:EPSG projection map example: `EPSG:4326`
	 * @method
	 */

	public getProjection(idMap: string) {
		// eslint-disable-next-line
		const projectionObj: any = this.stagingMap
			?.get(idMap)
			?.getMap()
			?.getView()
			?.getProjection();
		const codeProjection = projectionObj?.getCode();
		return codeProjection;
	}

	/**
	 * This function obtain a View from Map
	 * @param {string} idMap Name of the map we want to obtain View
	 * @returns {GoView} Return a View from GoView
	 * @method
	 */

	public getView(idMap: string) {
		let view;
		if (!this.mapError({ idMap, fnName: "getView" })) {
			const mapa = this.stagingMap.get(idMap);
			view = mapa?.getMap().getView();
		}
		return view;
	}

	/**
	 *
	 * @typedef Coordinate
	 * @property {Array<number>} - Coordinates [x,y] or [lat,long]
	 */

	/**
	 * This function get center from a Map with a json configuration
	 * @param {string} idMap  Name of the map we want to obtain center
	 * @returns {Coordinate} Return a Coordinates Center
	 * @method
	 */

	public getCenter(idMap: string): olCoordinate.Coordinate | undefined {
		let center: olCoordinate.Coordinate | undefined;
		if (!this.mapError({ idMap, fnName: "getCenter" })) {
			const map = this.stagingMap.get(idMap);
			center = map!.getMap().getView().getCenter();
		}
		return center;
	}

	/**
	 * This function change center from  a Map
	 * @param {string} idMap  Name of the map we want to change center
	 * @param {Coordinate} center Coordinates Center
	 * @method
	 */

	public setCenter(idMap: string, center: [number, number]): void {
		if (!this.mapError({ idMap, fnName: "setCenter" })) {
			const map = this.stagingMap.get(idMap);
			map!.getMap().getView().setCenter(center);
		}
	}

	/**
	 * This function obtain the zoom level from Map in that moment
	 * @param {string} idMap  Name of the map we want to change center
	 * @method
	 */

	public getZoom(idMap: string): number | undefined {
		let zoom: number | undefined;
		if (!this.mapError({ idMap, fnName: "getZoom" })) {
			const map = this.stagingMap.get(idMap)?.getMap();
			zoom = map?.getView().getZoom();
		}
		return zoom;
	}
	/**
	 * This function set the zoom level from Map
	 * @param {string} idMap  Name of the map we want to change center
	 * @param {number} Zoom  Number value to set Zoom in the Map Number from 0 to 20
	 * @method
	 */

	public setZoom(idMap: string, zoom: number): void {
		if (!this.mapError({ idMap, fnName: "setZoom" })) {
			const map = this.stagingMap.get(idMap)?.getMap();
			map?.getView().setZoom(zoom);
		}
	}
	/**
	 * This function change location in the map from a especific Extent
	 * @param {string} idMap  Name of the map we want to change center
	 * @param {Extent} Extent Extent Value [xmin,ymin,xmax,ymax]
	 * @method
	 */

	public goToExtent(idMap: string, extent: Extent) {
		if (!this.mapError({ idMap, fnName: "goToExtent" })) {
			const map = this.stagingMap.get(idMap)?.getMap();
			map?.getView().fit(extent);
		}
	}
	/**
	 * This function add animation to translate from one position to another
	 * @param {string} idMap  Name of the map we want to change center
	 * @param {Array<number>} centerTo Center to fly
	 * @param {number} duration Duration in secons the dynamic fly
	 * @param {number} [zoom] Value to final zoom
	 * @method
	 */

	public flyToCenter(
		idMap: string,
		centerTo: [number, number],
		duration?: number,
		zoom?: number
	): void {
		if (!this.mapError({ idMap, fnName: "flyToCenter" })) {
			const map = this.stagingMap.get(idMap)?.getMap();
			this.flyTo(map!, centerTo, duration, zoom);
		}
	}
	/**
	 * Obtain OpenLayers Map
	 * @param {string} idMap  Name of the map we want obtain
	 * @returns {OlMap}
	 * @method
	 */

	public getMap(idMap: string): OlMap | undefined {
		let map: OlMap | undefined;
		if (!this.mapError({ idMap, fnName: "getMap" })) {
			map = this.stagingMap.get(idMap)?.getMap();
			return map;
		}
	}

	/**
	 * Obtain all data from all maps: Map,Layers, interacctions etc....
	 * @returns {Map<string, GOStagingMap>}
	 * @method
	 */

	public getAllStaging(): Map<string, GOStagingMap> {
		return this.stagingMap;
	}

	/**
	 * Obtain all data from one map: Map,Layers, interacctions etc....
	 * @param {string} idMap  Name of map to obtain all data
	 * @returns {GOStagingMap}
	 * @method
	 */

	public getStaging(idMap: string): GOStagingMap | undefined {
		return this.stagingMap.get(idMap);
	}

	/**
	 * Set language for all GOStagingMap instances.
	 * @param {types.SupportedLangs} lang - The language to set.
	 */
	public setLanguage(lang: types.SupportedLangs): void {
		if (!Object.values(types.SystemLanguages).includes(lang)) {
			console.error(
				`Unsupported language: '${lang}', operation ignored.`
			);
			return;
		}

		const LangLocalStorage =
			localStorage.getItem("gis-lang") || types.SystemLanguages.EN;
		if (LangLocalStorage !== lang) {
			localStorage.setItem("gis-lang", lang);
			i18nService.setLanguage(lang);
		}
		for (const staging of this.stagingMap.values()) {
			//Render DOM
			staging.getOwnToc()?.setLanguage(lang);
			staging.getMapLayerPreviewer()?.updatePreviewer();

			staging.getLayers().forEach((layer) => {
				if (
					layer instanceof GoRemoteSensing ||
					layer instanceof GoVerticalRemoteSensing ||
					layer instanceof GoWaterLeakRemoteSensing
				) {
					// UI may not be initialized yet (async init). Guard before calling getLang()/updateLang().
					const ui = (layer as any).getUI && (layer as any).getUI();
					if (ui && typeof ui.getLang === "function") {
						const langUIRemoteSensing = ui.getLang();
						if (
							langUIRemoteSensing &&
							typeof langUIRemoteSensing.updateLang === "function"
						) {
							langUIRemoteSensing.updateLang();
						}
					}
				}
			});

			const ifcMap = staging.getInteractions().getIFCRegistry();
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
	}

	/**
	 * Gets actual theme for all maps, if you never used setThemeAllMaps, it returns themes.DARK
	 * @returns {themes}
	 * @method
	 */
	public getTheme(): themes {
		//devuelve el último theme pasado al setThemeAllMaps, si nunca se uso,
		//para igualar todos los mapas se usara por defecto el DARK
		if (this.__theme) {
			return this.__theme;
		} else {
			return themes.DARK;
		}
	}

	/**
	 * Sets dark/light theme all maps.
	 * @param {themes} theme - The language to set.
	 */
	public setThemeAllMaps(theme: themes): void {
		this.__theme = theme;
		this.getAllStaging().forEach((mapInstance: GOStagingMap) => {
			mapInstance.setTheme(theme);
		});
	}

	/**
	 * Get Actual language
	 * @returns {types.SupportedLangs}
	 * @method
	 */
	public getLanguage(): types.SupportedLangs {
		return i18nService.getLanguage();
	}

	/**
	 * Refresh all content of map
	 * @param {string} idMap  Name of map to refresh
	 * @method
	 */

	public refreshMap(idMap: string): void {
		if (!this.mapError({ idMap, fnName: "refreshMap" })) {
			this.stagingMap.get(idMap)?.getMap().renderSync();
		}
	}

	/**
	 * Get All layers from Map
	 * @param {string} idMap  Name of map to obtain layers
	 * @returns {GOLayer}
	 * @method
	 */

	public getLayers(idMap: string) {
		if (!this.mapError({ idMap, fnName: "getLayers" })) {
			const layers = this.stagingMap.get(idMap)?.getLayers();
			return layers;
		}
	}

	/**
	 * Add one layer to Map
	 * *Example:
	 * ```ts
	 *     const options = ' {
			"id": "manantial",
			"alias": "Manantial",
			"layerType": "GO_VECTOR_LAYER",
			"isBaseLayer": false,
			"opacity": 1,
			"url": "ENV/geoserver/SACMEX",
			"layerName": "SACMEX:manantial",
			"urlFolder": "Geografía/",
			"zIndex":1,
			"defaultVisibility":false,
			"sldStyle": {
				"url": "ENV/geoserver/rest/styles/manantial.sld",
				"mapUnits": true
			}
		}
		'
	 *     const GoMap = new GoMap();
	 *     GoMap.addLayer(options)
	 * ```
	 * @param {string} idMap  Name of map to add layer
	 * @param {import('types').layerOptions} layerOptions  Options from Layer to configurate all
	 *
	 * @returns {GOLayer}
	 * @method
	 */

	public addLayer(idMap: string, layerOpts: types.layerOptions) {
		return this.setLayer(idMap, layerOpts);
	}

	public getState() {
		return GoStore.getState();
	}

	/**
	 * Remove one Layer from Map
	 * @param {string} idMap  Name of map to obtain layers
	 * @param {string} idLayer  Name of Layer
	 * @returns {GOStagingMap}
	 * @method
	 */

	public removeLayer(idMap: string, idLayer: string): void {
		const stagingMap = this.stagingMap.get(idMap);

		if (!stagingMap) {
			return;
		}

		const layer = stagingMap.getLayer(idLayer);
		if (layer) {
			// Remove layer
			stagingMap.getMap().removeLayer(layer.getLayer());
			stagingMap.getMap().renderSync();
			stagingMap.removeLayer(idLayer);

			// Check if layer has onRemove method
			if (layer.onRemove) {
				layer.onRemove();
			}

			// Remove layer from the ownTOC
			const ownToc = stagingMap.getOwnToc();
			if (ownToc) {
				ownToc.removeLayer(idLayer);
			}

			layer.emit(GOLayerEvents.LAYER_REMOVED, {
				mapId: idMap,
				layerId: idLayer,
				layer,
			});
		}
	}

	/**
	 * Refresh data from layer
	 * @param {string} idMap  Name of map to obtain layers
	 * @param {Array<string>} idLayer  Name of Layer
	 * @method
	 */

	public refreshLayer(idMap: string, idLayer?: Array<string>): void {
		if (!this.mapError({ idMap, fnName: "refreshLayer" })) {
			const map = this.stagingMap.get(idMap);
			if (idLayer) {
				idLayer.forEach((layer) => {
					if (layer) {
						map!.getLayer(layer).getLayer().getSource().refresh();
					} else {
						// Error Handler
						goErrorsHandlers.internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors
								.INTERNAL_ERROR_CANT_LOAD_LAYER_MISSING,
							internalErrosMsg.INTERNAL_ERROR_CANT_LOAD_LAYER_MISSING +
								idLayer
						);
						this.emit(GOMapEvents.MAP_ERROR, {
							errorMsg:
								internalErrosMsg.INTERNAL_ERROR_CANT_LOAD_LAYER_MISSING +
								idLayer,
						});
					}
				});
			} else {
				const layers = map!.getLayers();
				layers.forEach((layer) => {
					if (layer) {
						(layer as GOLayer).getLayer().getSource().refresh();
					} else {
						// Error Handler
						goErrorsHandlers.internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors
								.INTERNAL_ERROR_CANT_LOAD_LAYER_MISSING,
							internalErrosMsg.INTERNAL_ERROR_CANT_LOAD_LAYER_MISSING +
								idLayer
						);
						this.emit(GOMapEvents.MAP_ERROR, {
							errorMsg:
								internalErrosMsg.INTERNAL_ERROR_CANT_LOAD_LAYER_MISSING +
								idLayer,
						});
					}
				});
			}
		}
	}

	/**
	 * Inject data in a GeoJson layer
	 * @param {string} idMap  Name of map to obtain layers
	 * @param {string} idLayer  Name of Layer
	 * @param {boolean} ClearData  If Inject data Remove all Features from Layer
	 * @param {GeoJSON} Data  Data to inject in Layer
	 * @method
	 */

	public injectDataGeoJson(
		idMap: string,
		idLayer: string,
		clear: boolean,
		// eslint-disable-next-line
		data: any
	) {
		if (!this.mapError({ idMap, fnName: "injectDataGeoJson" })) {
			const map = this.stagingMap.get(idMap);
			if (this.existLayer(idMap, idLayer)) {
				const layer = map!.getLayer(idLayer);
				if (clear === true) {
					try {
						layer.getLayer().getSource().clear();
						layer
							.getLayer()
							.getSource()
							.addFeatures(
								new GeoJSON()
									.readFeatures(data.default)
									.filter((feat) => feat.getGeometry())
							);
					} catch {
						// Error Handler
						goErrorsHandlers.internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors.INTERNAL_ERROR_DATA,
							internalErrosMsg.INTERNAL_ERROR_DATA
						);
						this.emit(GOMapEvents.MAP_ERROR, {
							errorMsg: internalErrosMsg.INTERNAL_ERROR_DATA,
						});
					}
				} else {
					try {
						layer
							.getLayer()
							.getSource()
							.addFeatures(
								new GeoJSON()
									.readFeatures(data.default)
									.filter((feat) => feat.getGeometry())
							);
					} catch (error: any) {
						// Error Handler
						goErrorsHandlers.internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors.INTERNAL_ERROR_DATA,
							internalErrosMsg.INTERNAL_ERROR_DATA,
							error
						);
						this.emit(GOMapEvents.MAP_ERROR, {
							errorMsg: internalErrosMsg.INTERNAL_ERROR_DATA,
							error: error,
						});
					}
				}
			}
		}
	}

	/**
	 * This function allow to determine if the layer exists
	 * @param {string} idMap - The id of the map
	 * @param {string} idLayer - The id of the layer
	 * @returns {void} Return nothing
	 * @method
	 */

	public existLayer(idMap: string, idLayer: string) {
		if (!this.mapError({ idMap, fnName: "existLayer" })) {
			return this.stagingMap.get(idMap)!.getLayers().has(idLayer);
		}
	}

	/**
	 * Do any interaction when the map .....
	 * @param {string} idMap  Name of map of launch method
	 * @param {import('constants').typeOn} type  types to do in the method :
	 *change - Generic change event. Triggered when the revision counter is increased.
	 *change:layerGroup
	 *change:size
	 *change:target
	 *change:view
	 *click  - A click with no dragging. A double click will fire two of this.
	 *dblclick  - A true double click, with no dragging.
	 *error  - Generic error event. Triggered when an error occurs.
	 *moveend  - Triggered after the map is moved.
	 *movestart  - Triggered when the map starts moving.
	 *pointerdrag  - Triggered when a pointer is dragged.
	 *pointermove - Triggered when a pointer is moved. Note that on touch devices this is triggered when the map is panned, so is not the same as mousemove.
	 *postcompose  - Triggered after all layers are rendered. The event object will not have a context set.
	 *postrender  - Triggered after a map frame is rendered.
	 *precompose  - Triggered before layers are rendered. The event object will not have a context set.
	 *propertychange  - Triggered when a property is changed.
	 *rendercomplete- Triggered when rendering is complete, i.e. all sources and tiles have finished loading for the current viewport, and all tiles are faded in. The event object will not have a context set.
	 *singleclick
	 * @param {Function} callback  Function to do in this method
	 * @returns {boolean}
	 * @method
	 */
	// eslint-disable-next-line
	public onMap(idMap: string, type: typeOn, callback: any) {
		if (!this.mapError({ idMap, fnName: "onMap" })) {
			const map = this.stagingMap.get(idMap)?.getMap();
			// eslint-disable-next-line
			map!.on(type as any, callback);
		}
	}

	/**
	 * This function allow to determine if the map is a staging map
	 * @param {string} idMap - The id of the map
	 * @returns {void} Return nothing
	 * @method
	 */
	private hasStagingMap(idMap: string) {
		return this.stagingMap.has(idMap);
	}

	/**
	 * This Method allows to bind views taking one of them as a reference of bind
	 * @param {string} refView  Name of reference map
	 * @param {Array<string>} viewsToBind  Array of map names
	 * @method
	 */
	public bindViews(refView: string, viewsToBind?: Array<string>): void {
		const refGoView = this.stagingMap.get(refView)!.getMap().getView();
		if (viewsToBind === null) {
			this.stagingMap.forEach((mapa) => {
				mapa.getMap().setView(refGoView);
			});
		} else if (viewsToBind && viewsToBind.length) {
			viewsToBind.forEach((mapa) => {
				this.stagingMap.get(mapa)!.getMap().setView(refGoView);
			});
		}
	}

	/**
	 * This Method unbind all views of maps
	 * @param {any} option - The option to unbind
	 * @method
	 */
	// eslint-disable-next-line
	public unbindAllViews(option: any): void {
		this.stagingMap.forEach((mapa) => {
			const viewDummy = this.createView(
				option.GOMapOptions[0].optionView
			);
			this.getMap(mapa.getId())!.setView(viewDummy);
		});
	}

	/**
	 * This function allow to move the map to a new position
	 * @param {import('types').OlMap} map - The map to move
	 * @param {number[]} centerTo - The center to move
	 * @param {number} duration - The duration of the move
	 * @param {number} zoom - The zoom to move
	 * @returns {void} Return nothing
	 * @method
	 */
	private flyTo(
		map: OlMap,
		centerTo: [number, number],
		duration?: number,
		zoom?: number
	): void {
		const zoomTo = zoom || map.getView().getZoom();
		const durationTime = duration || 2000;
		let parts = 2;
		let called = false;
		// eslint-disable-next-line
		const done = (complete: any) => {};
		// eslint-disable-next-line
		function callback(complete: any) {
			--parts;
			if (called) {
				return;
			}
			if (!parts || !complete) {
				called = true;
				done(complete);
			}
		}
		map.getView().animate(
			{
				center: centerTo,
				duration: duration,
			},
			callback
		);
		map.getView().animate(
			{
				zoom: zoomTo! - 1,
				duration: durationTime / 2,
			},
			{
				zoom: zoomTo,
				duration: durationTime / 2,
			},
			callback
		);
	}

	/**
	 * This function allow to set a new layer.
	 * @param {string} idMap - The id of the map
	 * @param {import('types').layerOptions} layerOption - The layer options.
	 * @returns {import('types').GOLayer  | null } Return promise Promise<GOLayer | null>
	 * @method
	 */
	private setLayer(idMap: string, layerOption: types.layerOptions) {
		if (!this.mapError({ idMap, fnName: "setLayer" })) {
			const layerOpts: types.layerOptions | types.LayerOptsMapbox =
				layerOption;

			const stagingMap = this.stagingMap.get(idMap);

			if (!stagingMap) {
				throw new ReferenceError(
					`${MAPS_ERROR_MAP_DONT_EXIST} ${idMap}`
				);
			}

			switch (layerOpts.layerType) {
				case GOLayerType.GO_WMTS_LAYER: {
					return new GOWMTSLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_VECTOR_TILE: {
					return new GOVectorTiles(stagingMap, layerOpts);
				}
				case GOLayerType.GO_VECTOR_TILE_OWN: {
					return new GOVectorTilesOwn(stagingMap, layerOpts);
				}
				case GOLayerType.GO_WMS_LAYER: {
					return new GOWMSLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_VECTOR_LAYER: {
					return new GOVectorLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_TRUCK_LAYER: {
					return new GoTruckLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_IOT_LAYER: {
					return new GoIOTLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_HEXGRID_LAYER: {
					return new GoHexGridLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_HEXGRID: {
					return new GoHexGrid(stagingMap, layerOpts);
				}
				case GOLayerType.GO_HEATMAP_LAYER: {
					return new GoHeatMapLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_XYZ: {
					return new GoXYZ(stagingMap, layerOpts);
				}
				case GOLayerType.GO_GEOJSON_LAYER: {
					return new GOGeoJsonLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_WFS_LAYER: {
					return new GOWFSLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_MAPBOX: {
					return new GoMapBoxLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_ESRI_BASEMAP: {
					return new GoEsriBaseMap(stagingMap, layerOpts);
				}
				case GOLayerType.GO_CLUSTER_LAYER: {
					return new GOClusterLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_WEBGL_LAYER: {
					return new GoWebGlLayer(stagingMap, layerOpts);
				}
				case GOLayerType.GO_REMOTE_SENSING: {
					if (!hasBackendFeatures(`Error on setLayer method`)) {
						break;
					} else {
						if (
							layerOpts.remoteSensingType &&
							layerOpts.remoteSensingType == "vertical"
						) {
							return new GoVerticalRemoteSensing(
								stagingMap,
								layerOpts
							);
						} else if (
							layerOpts.remoteSensingType &&
							layerOpts.remoteSensingType == "waterLeak"
						)
							return new GoWaterLeakRemoteSensing(
								this,
								stagingMap,
								layerOpts
							);
						else {
							return new GoRemoteSensing(stagingMap, layerOpts);
						}
					}
				}
				default:
					// Error Handler
					goErrorsHandlers.internalErrorHandler.set(
						constants.ErrorSeverity.Warning,
						constants.internalErrors
							.INTERNAL_ERROR_IS_NOT_A_VALID_LAYER_TYPE,
						internalErrosMsg.INTERNAL_ERROR_IS_NOT_A_VALID_LAYER_TYPE
					);
					this.emit(GOMapEvents.MAP_ERROR, {
						errorMsg:
							internalErrosMsg.INTERNAL_ERROR_IS_NOT_A_VALID_LAYER_TYPE,
					});
					break;
			}
		}
	}

	public addNewLayer(idMap: string, layerConfig: types.layerOptions) {
		if (this.existLayer(idMap, layerConfig.id)) {
			this.handleDuplicateLayerError(idMap, layerConfig.id);
			return null;
		}

		this.extractLayerNameFromGeoparams(layerConfig);
		this.processLayerUrls(layerConfig);

		const layer = this.addLayer(idMap, layerConfig);

		if (layer) {
			this.layersLoader(layer as GOLayer);
			if (layer.goMap.getOwnToc()) {
				layer.goMap.getOwnToc()?.addNewLayer(layerConfig);
			}
			return layer;
		}
	}

	private handleDuplicateLayerError(idMap: string, layerId: string): void {
		console.warn(
			`Layer with id '${layerId}' already exists in map '${idMap}'`
		);

		this.emit(GOMapEvents.MAP_ERROR, {
			errorMsg: `Layer with id '${layerId}' already exists in map '${idMap}'`,
			mapId: idMap,
			layerId: layerId,
		});
	}

	private extractLayerNameFromGeoparams(
		layerConfig: types.layerOptions
	): void {
		if (layerConfig.layerName || !layerConfig.geoparams) {
			return;
		}

		const geoparams: Record<string, string> = layerConfig.geoparams
			.split("&")
			.reduce(
				(params, param) => {
					const [key, value] = param.split("=");
					if (key && value) {
						params[key.toUpperCase()] = value;
					}
					return params;
				},
				{} as Record<string, string>
			);

		if (geoparams["LAYERS"]) {
			layerConfig.layerName = geoparams["LAYERS"];
		}
	}

	private processLayerUrls(layerConfig: types.layerOptions): void {
		if (layerConfig.layerType === GOLayerType.GO_GEOJSON_LAYER) {
			return;
		}

		const checkedEnviroment: string = GoMapLoader.checkEnviroment(
			layerConfig.url
				? layerConfig.url
				: GoStore.getState().geoserverAuthData!.url
		);

		this.processLayerUrl(layerConfig, checkedEnviroment);
		this.processSldStyleUrl(layerConfig, checkedEnviroment);
	}

	private processLayerUrl(
		layerConfig: types.layerOptions,
		checkedEnviroment: string
	): void {
		if (!layerConfig.url || GoMapLoader.containsHttp(layerConfig.url)) {
			return;
		}

		const checkedUrlLayer: string = GoMapLoader.checkUrlLayer(
			layerConfig.url
		);
		layerConfig.url = `${checkedEnviroment}/${checkedUrlLayer}`;
	}

	private processSldStyleUrl(
		layerConfig: types.layerOptions,
		checkedEnviroment: string
	): void {
		if (
			!layerConfig.sldStyle ||
			!layerConfig.sldStyle.url ||
			GoMapLoader.containsHttp(layerConfig.sldStyle.url)
		) {
			return;
		}

		const checkedUrlLayer: string = GoMapLoader.checkUrlLayer(
			layerConfig.sldStyle.url
		);
		layerConfig.sldStyle.url = `${checkedEnviroment}/${checkedUrlLayer}`;
	}

	public layersLoader(layer: GOLayer) {
		try {
			const { id } = layer.getOptions();
			const idMap = layer.goMap.getId();

			this.emit(GOLayerEvents.LAYER_LOADING, {
				mapId: idMap,
				layerId: id,
			});

			this.emit(GOLayerEvents.LAYER_LOADED, {
				mapId: idMap,
				layerId: id,
			});

			// Añadir la capa al mapa
			layer.olMap.addLayer(layer.getLayer());
			layer.goMap.addLayer(id, layer);

			//Add default interaction hover if exist
			if (
				this.objInteraction?.hover &&
				this.objInteraction?.hover?.active
			) {
				const obj = this.objInteraction?.hover as any;
				obj.layers.forEach((elem: string) => {
					if (elem === id) {
						this.interactions.mouseHover({
							idMap,
							idHover: obj.prefix + "-" + elem,
							layer: elem,
						});
					}
				});
			}

			this.emit(GOLayerEvents.LAYER_ADDED, {
				mapId: idMap,
				layerId: id,
			});
		} catch (error: any) {
			// Error Handler
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER,
				error
			);

			this.emit(GOMapEvents.MAP_ERROR, {
				errorMsg: internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER,
				error,
			});

			return error;
		}
	}

	private mapError({ idMap, fnName }): boolean {
		const msg =
			`Map ID ${idMap}. Error on "${fnName}":` +
			MAPS_ERROR_MAP_DONT_EXIST;
		if (this.hasStagingMap(idMap)) {
			return false;
		}
		// Error Handler
		goErrorsHandlers.mapsErrorHandler.set(
			constants.ErrorSeverity.Warning,
			constants.mapsErrors.MAPS_ERROR_MAP_DONT_EXIST,
			msg
		);
		this.emit(GOMapEvents.MAP_ERROR, {
			mapId: idMap,
			errorMsg: msg,
		});

		return true;
	}

	public on(eventName: string, callback): void {
		this.eventEmitter.on(eventName, callback);
	}

	public once(eventName: string, callback): void {
		this.eventEmitter.once(eventName, callback);
	}

	public off(eventName?: string, callback?): void {
		this.eventEmitter.off(eventName, callback);
	}

	public hasEvent(eventName: string): boolean {
		return this.eventEmitter.has(eventName);
	}

	public getEventHandlers(eventName: string) {
		return this.eventEmitter.getHandlers(eventName);
	}

	public emit(eventName: string, data): void {
		MapEventManager.events.fire(eventName, data);
		this.eventEmitter.fire(eventName, data);
	}
}

---
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Map\GoMapLoader.ts
import { GO_CONFIG } from "@config/index";
import * as types from "@goTypes/index";
import GosetDataBack from "@services/GoDataBack/setDataBack";
import { GoAuth, GoStore } from "@services/index";
import GoOwnToc from "@toc/GoOwnToc";
import {
	GODateTodayLoader,
	GoLayerSourceType,
	GOLayerType,
	GOMapEvents,
	GOOverloadFilterOption,
	themes,
} from "@utils/constants";
import {
	ErrorSeverity,
	goErrorsHandlers,
	hasBackendFeatures,
	isLocalhost,
	MapEventManager,
	mapsErrors,
	mapsErrorsMsg,
} from "@utils/index";
import { GoMapBoxLayer } from "../Layers";
import { MapLayerPreviewer } from "../UIMap/MapLayerPreviewer";
import { UIDisplay } from "../UIMap/UIDisplay";
import { GoMap } from "./GoMap";

export interface MapConfigLoader {
	mapConfig: types.configOptions;
	enviroment: string;
	userToken?: string;
	useBackendFeatures?: boolean;
	isExample?: boolean;
}
/**
 * This is a class to generate maps dinamicaly
 * @class GoMapLoader
 * @param  {config} Config JSON CONFIG from all maps and layers
 * @param  {string} enviroment Enviroment from server we have Geoserver or enviorment of site
 */
export class GoMapLoader {
	public static linkArray: Array<string> = [];
	public static dataBack: Array<any> = []; // eslint-disable-line
	public static defaultMapConfig: MapConfigLoader = {
		mapConfig: {
			GOMapOptions: [],
		},
		enviroment: GO_CONFIG.REGION_ENVIROMENTS.EUROPE,
		userToken: undefined,
		useBackendFeatures: true,
		isExample: false,
	};

	/**
	 * This function allow to load the map.
	 * @param {any} init - JSON CONFIG from all maps and layers
	 * @param {string} enviroment - Enviroment from server we have Geoserver or enviorment of site
	 * @param {string} geoserver - The region enviroment where the map is loaded.
	 * @returns {import('types').GoMap} Return the map
	 * @method
	 */
	public static async loadGoMap(
		mapConfigLoader: MapConfigLoader
	): Promise<GoMap> {
		const {
			mapConfig,
			enviroment,
			userToken,
			useBackendFeatures,
			isExample,
		} = {
			...GoMapLoader.defaultMapConfig,
			...structuredClone(mapConfigLoader),
		};

		this.dataBack = [];

		const localConfig = this.configureLocalConfig(mapConfig);
		GoStore.dispatch({ useBackendFeatures });
		if (!hasBackendFeatures()) {
			const mapState = {
				mapConfig: localConfig,
				enviroment,
			};

			// Initialize the store
			GoStore.dispatch(mapState);
			return GoMapLoader.__loadGoMap(localConfig, enviroment);
		} else if (userToken) {
			const geoserverAuthData =
				await this.getGeoServerAuthData(enviroment, userToken);
			
			const mapState = {
				mapConfig: localConfig,
				enviroment,
				geoserverAuthData,
				token: userToken,
				isExample,
			};

			// Initialize the store
			GoStore.dispatch(mapState);

			return GoMapLoader.__loadGoMap(mapConfig, geoserverAuthData.url);
		}
	}

	/**
	 * Gets the GeoServer authentication data for the user.
	 * @param {string} enviroment - Entorno del servidor.
	 * @param {string} userToken - Token proporcionado por el usuario.
	 * @returns {Promise<any>} Objeto con la configuración de geoserver.
	 */
	private static async getGeoServerAuthData(
		enviroment: string,
		userToken: string
	): Promise<any> {
		GoStore.dispatch({ token: userToken });
		return await GoAuth.getGeoServerAuth(enviroment);
	}

	private static configureLocalConfig(mapConfig: types.configOptions) {
		//Check if the url is local or remote
		if (isLocalhost()) {
			mapConfig.GOMapOptions.forEach((mapC) => {
				// eslint-disable-next-line
				const darkThemeLayer = mapC.layer!.find((layer: any) => {
					return (
						layer.layerType === GOLayerType.GO_MAPBOX &&
						layer.isBaseLayer &&
						layer.style === "cljzp2t2r008b01qr8izjchch"
					);
				});

				// eslint-disable-next-line
				const lightThemeLayer = mapC.layer!.find((layer: any) => {
					return (
						layer.layerType === GOLayerType.GO_MAPBOX &&
						layer.isBaseLayer &&
						layer.style === "ckugis5on3dz917ql0ml326rk"
					);
				});

				if (darkThemeLayer) {
					(darkThemeLayer.url =
						"https://basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png"),
						(darkThemeLayer.layerType = GOLayerType.GO_XYZ);
				}

				if (lightThemeLayer) {
					(lightThemeLayer.url =
						"https://basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png"),
						(lightThemeLayer.layerType = GOLayerType.GO_XYZ);
				}
			});
		}

		// Check if there are some layer with the type GO_TRUCK_LAYER to move it to the end of the array
		const truckLayer = mapConfig.GOMapOptions.map((map) => {
			return map.layer?.find((layer) => {
				return layer.layerType === GOLayerType.GO_TRUCK_LAYER;
			});
		});

		if (truckLayer) {
			mapConfig.GOMapOptions.forEach((map) => {
				const truckLayerIndex = map.layer?.findIndex((layer) => {
					return layer.layerType === GOLayerType.GO_TRUCK_LAYER;
				});

				if (truckLayerIndex !== undefined && truckLayerIndex >= 0) {
					const truckLayer = map.layer?.splice(truckLayerIndex, 1);
					if (truckLayer) {
						map.layer?.push(truckLayer[0]);
					}
				}
			});
		}

		return mapConfig;
	}

	public static __loadGoMap(
		mapConfig: types.configOptions,
		geoserverURL: string
	): GoMap {
		const goMap = new GoMap();

		if (mapConfig && Array.isArray(mapConfig.GOMapOptions)) {
			mapConfig.GOMapOptions.forEach((option) => {
				MapEventManager.events.fire(GOMapEvents.MAP_LOADING, {
					mapId: option.id,
				});
			});
		}

		const checkedEnviroment: string = this.checkEnviroment(geoserverURL);

		for (const mapOpts of mapConfig.GOMapOptions) {
			const mapContainer = document.getElementById(mapOpts.div);

			if (mapOpts.projections && mapOpts.projections.length) {
				mapOpts.projections.forEach((projection: types.projOptions) => {
					goMap.createProjection(projection);
				});
			}

			if (!mapContainer) {
				goErrorsHandlers.mapsErrorHandler.set(
					ErrorSeverity.Warning,
					mapsErrors.MAPS_ERROR_DIV_NOT_FOUND,
					mapsErrorsMsg.MAPS_ERROR_DIV_NOT_FOUND + mapOpts.div
				);
				continue;
			}

			mapContainer.classList.add("go-map-container");

			goMap.addMap(
				{
					id: mapOpts.id,
					div: mapOpts.div,
					optionView: mapOpts.optionView,
				},
				mapOpts.interactions ? mapOpts.interactions : null,
				mapOpts
			);

			const currentTheme = goMap.getStaging(mapOpts.id)?.getTheme();
			if (currentTheme) {
				goMap.getStaging(mapOpts.id)?.setTheme(currentTheme);
			}

			const uiDisplay = new UIDisplay(goMap, mapOpts, mapContainer);
			goMap.getStaging(mapOpts.id)?.setUIDisplay(uiDisplay);

			const baseLayers: GoMapBoxLayer[] = [];

			for (const layerOpts of mapOpts.layer || []) {
				const weglCondition =
					layerOpts.layerType !== GOLayerType.GO_WEBGL_LAYER ||
					layerOpts.sourceType == GoLayerSourceType.GEOSERVER;

				if (
					!layerOpts.url &&
					GoStore.getState().useBackendFeatures &&
					GoStore.getState().geoserverAuthData?.url &&
					(layerOpts.layerType === GOLayerType.GO_VECTOR_LAYER ||
						(layerOpts.layerType === GOLayerType.GO_WEBGL_LAYER &&
							layerOpts.sourceType === "GEOSERVER") ||
						(layerOpts.layerType === GOLayerType.GO_CLUSTER_LAYER &&
							layerOpts.layerName) ||
						layerOpts.layerType === GOLayerType.GO_TRUCK_LAYER ||
						layerOpts.layerType === GOLayerType.GO_HEXGRID_LAYER ||
						layerOpts.layerType === GOLayerType.GO_WMS_LAYER ||
						layerOpts.layerType === GOLayerType.GO_IOT_LAYER)
				) {
					layerOpts.url = GoStore.getState().geoserverAuthData!.url;
				}

				if (layerOpts.url && !this.containsHttp(layerOpts.url)) {
					const checkedUrlLayer: string = this.checkUrlLayer(
						layerOpts.url
					);
					layerOpts.url = `${checkedEnviroment}/${checkedUrlLayer}`;

					if (
						layerOpts.layerType !== GOLayerType.GO_WFS_LAYER &&
						layerOpts.layerType !== GOLayerType.GO_GEOJSON_LAYER &&
						weglCondition &&
						layerOpts.layerType !== GOLayerType.GO_WMTS_LAYER &&
						layerOpts.layerType !== GOLayerType.GO_CLUSTER_LAYER
					) {
						const base = new URL(layerOpts.url);
						layerOpts.url = base.origin;
					}
				} else if (
					layerOpts.url &&
					!layerOpts.isBaseLayer &&
					layerOpts.layerType !== GOLayerType.GO_WFS_LAYER &&
					layerOpts.layerType !== GOLayerType.GO_GEOJSON_LAYER &&
					weglCondition &&
					layerOpts.layerType !== GOLayerType.GO_WMTS_LAYER
				) {
					const state = GoStore.getState();
					const tenantUrl = state.geoserverAuthData?.url
						? new URL(state.geoserverAuthData.url)
						: null;
					const tileUrl = new URL(layerOpts.url);
					const isSameTenant =
						tenantUrl && tenantUrl.origin === tileUrl.origin;

					if (
						isSameTenant &&
						layerOpts.layerType === GOLayerType.GO_WMS_LAYER
					) {
						const base = new URL(layerOpts.url);
						layerOpts.url = base.origin;
					} else if (
						layerOpts.layerType !== GOLayerType.GO_WMS_LAYER &&
						layerOpts.layerType !== GOLayerType.GO_CLUSTER_LAYER
					) {
						const base = new URL(layerOpts.url);
						layerOpts.url = base.origin;
					}
				}

				if (
					layerOpts.sldStyle &&
					layerOpts.sldStyle.url &&
					!this.containsHttp(layerOpts.sldStyle.url)
				) {
					const checkedUrlLayer: string = this.checkUrlLayer(
						layerOpts.sldStyle.url
					);
					let base: URL;
					if (
						checkedEnviroment &&
						GoStore.getState().useBackendFeatures
					) {
						base = new URL(checkedEnviroment);
					} else if (layerOpts.url) {
						base = new URL(layerOpts.url);
					} else {
						throw new ReferenceError(
							"URL not found in layer for SLD style"
						);
					}

					if (
						layerOpts.layerType !== GOLayerType.GO_WFS_LAYER &&
						layerOpts.layerType !== GOLayerType.GO_GEOJSON_LAYER &&
						weglCondition &&
						layerOpts.layerType !== GOLayerType.GO_WMTS_LAYER
					) {
						layerOpts.sldStyle.url = `${
							base.origin
						}/geoserver/${checkedUrlLayer
							.replace("/geoserver/", "")
							.replace("/geoserver", "")
							.replace("geoserver/", "")
							.replace("geoserver", "")}`;
					} else {
						if (GoStore.getState().useBackendFeatures) {
							layerOpts.sldStyle.url = `${checkedEnviroment}/geoserver/${checkedUrlLayer
								.replace("/geoserver/", "")
								.replace("/geoserver", "")
								.replace("geoserver/", "")
								.replace("geoserver", "")}`;
						} else {
							layerOpts.sldStyle.url = `${
								layerOpts.url
							}/geoserver/${checkedUrlLayer
								.replace("/geoserver/", "")
								.replace("/geoserver", "")
								.replace("geoserver/", "")
								.replace("geoserver", "")}`;
						}
					}
				}
				const layer = goMap.addLayer(
					mapOpts.id,
					this.extendFilter(layerOpts, mapOpts.filter)
				);

				if (layerOpts.isBaseLayer) {
					baseLayers.push(layer as unknown as GoMapBoxLayer);
				}

				goMap.layersLoader(layer);
			}

			if (mapOpts.layer) {
				if (mapOpts.ui?.showLayersPreviewer) {
				const visibleBaseLayer = baseLayers.find(
					(layer) => layer.getVisibility() === true
				);

				//Quitamos la legenda de las capas base del toc
				baseLayers.forEach((layer) => {
					layer.getLayerOpts().toc = false;
				});

					if (visibleBaseLayer) {
						const mapLayerPreviewer = new MapLayerPreviewer(
							baseLayers,
							mapOpts,
							mapContainer,
							goMap
						);
						goMap
							.getStaging(mapOpts.id)
							?.setMapLayerPreviewer(mapLayerPreviewer);
					}
				}

				if (mapOpts.optionToc && mapOpts.optionToc.active) {
					const existPanelId = mapOpts.optionToc.panelId
						? mapOpts.optionToc.panelId
						: "ownTocContainer";

					const numMaps = mapConfig.GOMapOptions.length;

					if (!mapOpts.optionToc.tocAlwaysOpen) {
						const buttonToc = document.createElement("button");
						if (numMaps === 1) {
							buttonToc.id = `ui-btn-toc`;
							buttonToc.className = `ui-btn-toc`;

							buttonToc.onclick = function () {
								document
									.getElementById("ui-btn-toc")
									?.classList.toggle("ui-btn-active");
								document
									.getElementById(`${existPanelId}`)
									?.classList.toggle("active");
							};
						} else {
							buttonToc.id = `ui-btn-toc-${mapOpts.id}`;
							buttonToc.className = `ui-btn-toc ui-btn-toc-${mapOpts.optionToc.id}`;

							buttonToc.onclick = function () {
								document
									.getElementById(`ui-btn-toc-${mapOpts.id}`)
									?.classList.toggle("ui-btn-active");
								document
									.getElementById(`${existPanelId}`)
									?.classList.toggle("active");
							};
						}
						mapContainer.appendChild(buttonToc);
					}

					const tocContainer = document.createElement("div");
					tocContainer.id = `${mapOpts.optionToc.id}`;
					tocContainer.className = `ui-toc-container`;

					mapContainer.appendChild(tocContainer);

					const ownToc = new GoOwnToc(mapOpts.id, goMap);

					ownToc.createStageToc(mapOpts);
					goMap.getStaging(mapOpts.id)?.setOwnToc(ownToc);

					const currentTheme = goMap
						.getStaging(mapOpts.id)
						?.getTheme();
					goMap
						.getStaging(mapOpts.id)
						?.getOwnToc()
						?.setTheme(
							currentTheme ? currentTheme : themes.DARK,
							false
						);
				}

				if (mapOpts.interactions)
					goMap
						.getStaging(mapOpts.id)
						?.setInteractions(mapOpts.interactions);

				if (
					document.querySelector(
						"#" + mapOpts.div + "> #go-map-loader"
					)
				) {
					document
						.querySelector("#" + mapOpts.div + "> #go-map-loader")
						?.remove();
				}

				//Add interactions defined in config
				this.sendDataBack(mapOpts, goMap);
			}
			MapEventManager.events.fire(GOMapEvents.MAP_LOADED, {
				mapId: mapOpts.id,
			});
		}

		return goMap;
	}

	private static sendDataBack(mapOpts, GoMaps: GoMap) {
		if (mapOpts.interactions) {
			GoMaps.addInteractions(mapOpts.id, mapOpts.interactions);
			if (mapOpts.interactions.closeTool) {
				mapOpts.interactions.closeTool.dataPipes.forEach((layer) => {
					const layerOpts = mapOpts.layer.find(
						(element) => element.id === layer
					);

					if (!layerOpts) {
						// Show error
						goErrorsHandlers.mapsErrorHandler.set(
							ErrorSeverity.Warning,
							mapsErrors.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS,
							mapsErrorsMsg.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS +
								layer
						);

						return;
					}

					const dataFormat = {
						layerName: layerOpts.layerName,
						url:
							layerOpts.url ||
							(GoStore.getState().enviroment || "") + "/geoserver/wfs",
						mun_filter:
							mapOpts.interactions.closeTool.filterLocationName,
						mun_id: 0,
					};
					// eslint-disable-next-line @typescript-eslint/no-explicit-any
					this.dataBack.push(dataFormat as any);
				});
				mapOpts.interactions.closeTool.dataValves.forEach((layer) => {
					const layerOpts = mapOpts.layer.find(
						(element) => element.id === layer
					);

					if (!layerOpts) {
						// Show error
						goErrorsHandlers.mapsErrorHandler.set(
							ErrorSeverity.Warning,
							mapsErrors.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS,
							mapsErrorsMsg.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS +
								layer
						);

						return;
					}
					const dataFormat = {
						layerName: layerOpts.layerName,
						url:
							layerOpts.url ||
							(GoStore.getState().enviroment || "") + "/geoserver/wfs",
						mun_filter:
							mapOpts.interactions.closeTool.filterLocationName,
						mun_id: 0,
					};
					this.dataBack.push(dataFormat);
				});
			} else if (mapOpts.interactions.updownStream) {
				mapOpts.interactions.updownStream.dataPipes.forEach((layer) => {
					const layerOpts = mapOpts.layer.find(
						(element) => element.id === layer
					);
					if (!layerOpts) {
						// Show error
						goErrorsHandlers.mapsErrorHandler.set(
							ErrorSeverity.Warning,
							mapsErrors.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS,
							mapsErrorsMsg.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS +
								layer
						);

						return;
					}
					const dataFormat = {
						layerName: layerOpts.layerName,
						url:
							layerOpts.url ||
							(GoStore.getState().enviroment || "") + "/geoserver/wfs",
						mun_filter:
							mapOpts.interactions.updownStream
								.filterLocationName,
						mun_id: 0,
					};
					this.dataBack.push(dataFormat);
				});
				mapOpts.interactions.updownStream.dataWells.forEach((layer) => {
					const layerOpts = mapOpts.layer.find(
						(element) => element.id === layer
					);
					if (!layerOpts) {
						// Show error
						goErrorsHandlers.mapsErrorHandler.set(
							ErrorSeverity.Warning,
							mapsErrors.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS,
							mapsErrorsMsg.MAPS_ERROR_FILTER_LAYER_DONT_EXISTS +
								layer
						);

						return;
					}
					const dataFormat = {
						layerName: layerOpts.layerName,
						url:
							layerOpts.url ||
							(GoStore.getState().enviroment || "") + "/geoserver/wfs",
						mun_filter:
							mapOpts.interactions.updownStream
								.filterLocationName,
						mun_id: 0,
					};
					this.dataBack.push(dataFormat);
				});
			}
			if (!hasBackendFeatures(`Error on sendDataBack method`)) return;

			const setDataBack = new GosetDataBack();
			setDataBack.setDataBack(this.dataBack);
		}
	}

	/**
	 * This functions is used for store the region enviroment.
	 * @param env - The enviroment to add to the store.
	 * @returns The dispatch function is being returned.
	 */
	public static AddRegionEnv(env) {
		return GoStore.dispatch({ geoserver: env });
	}

	/**
	 * This function allow to extend the filter of layer.
	 * @param {import('types').layerOptions} layerOpts - The layer options
	 * @param {string} mapFilter - The filter of the map
	 * @returns {types.layerOptions} Return the layer options
	 * @method
	 */
	private static extendFilter(
		layerOpts: types.layerOptions,
		mapFilter?: string
	): types.layerOptions {
		const layerExtend: types.layerOptions = layerOpts;
		const originalFilter: string = layerOpts.filter;

		if (mapFilter !== null && mapFilter !== undefined) {
			layerExtend.filter = this.replaceDateKeyword(mapFilter);
		}

		if (originalFilter && layerOpts.filterOverload) {
			if (layerOpts.filterOverload === GOOverloadFilterOption.OVERLOAD) {
				layerOpts.filter = this.replaceDateKeyword(originalFilter);
			}
			if (layerOpts.filterOverload === GOOverloadFilterOption.ANDFILTER) {
				layerOpts.filter = `${
					layerOpts.filter
				} AND (${this.replaceDateKeyword(originalFilter)})`;
			}
			if (layerOpts.filterOverload === GOOverloadFilterOption.ORFILTER) {
				layerOpts.filter = `${
					layerOpts.filter
				} OR (${this.replaceDateKeyword(originalFilter)})`;
			}
		}

		return layerExtend;
	}

	/**
	 * This function allow to check the enviroment.
	 * @param {string} enviroment - Enviroment from server we have Geoserver or enviorment of site
	 * @returns {string} Return the enviroment
	 * @method
	 */
	public static checkEnviroment(geoserver: string): string {
		let updatedEnviroment = "";
		if (geoserver.endsWith("/")) {
			updatedEnviroment = geoserver.substr(0, geoserver.length - 1);
		} else {
			updatedEnviroment = geoserver;
		}
		return updatedEnviroment;
	}

	/**
	 * This function allow to check the URL of the layer.
	 * @param {string} layer - The layer URL.
	 * @returns {string} Return the URL of the layer.
	 * @method
	 */
	public static checkUrlLayer(layer: string): string {
		let updatedEnviroment = "";
		if (layer.startsWith("/")) {
			updatedEnviroment = layer.substr(1, layer.length);
		} else {
			updatedEnviroment = layer;
		}
		return updatedEnviroment;
	}

	/**
	 * This function allow to check if the URL contains http.
	 * @param {string} url - The url to check.
	 * @returns {boolean} Return true if the url contains http.
	 * @method
	 */
	public static containsHttp(url: string): boolean {
		if (/(http(s?)):\/\//i.test(url)) {
			return true;
		}
		return false;
	}

	/**
	 * This function allow to replace the date keyword.
	 * @param {string} filter - The filter to replace.
	 * @returns {string} Return the filter with the date keyword replaced.
	 * @method
	 */
	private static replaceDateKeyword(filter: string): string {
		if (filter.indexOf(GODateTodayLoader)) {
			// Get Today Date
			const dateToday = new Date();

			const mm = dateToday.getMonth() + 1; // getMonth() is zero-based
			const dd = dateToday.getDate();
			const stringDate = [
				dateToday.getFullYear(),
				(mm > 9 ? "" : "0") + mm,
				(dd > 9 ? "" : "0") + dd,
			].join("-");

			return filter.split(GODateTodayLoader).join(stringDate);
		}
		return filter;
	}
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
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\GoLayer\index.ts
import { EventEmitter } from "@billjs/event-emitter";
import * as types from "@goTypes/index";
import {
	GoFilter,
	GoGlobalStyle,
	LayerInteractions,
	layerOptions,
} from "@goTypes/index";

import GoLayerInteractions from "@interactions/GoLayerInteractions";
import { GOStagingMap } from "@map/index";
import { GoGeoServices, GoStore } from "@services/index";
import * as constants from "@utils/classes/GoError/constants";
import { internalErrosMsg } from "@utils/classes/GoError/errrosMsg";
import { internalErrorHandler } from "@utils/classes/GoError/goErrorsHandlers";
import { GoFilterCondition, GOLayerEvents } from "@utils/constants";
import { getParamsFromUrl, getUrlWithoutParams } from "@utils/functions";
import {
	goErrorsHandlers,
	GOLayerType,
	GoStyle,
	MapEventManager,
} from "@utils/index";
import { CancelablePromise } from "cancelable-promise";
import { Map as OlMap } from "ol";
import Feature from "ol/Feature";
import { AxiosResponse } from "axios";
import GeoJSON from "ol/format/GeoJSON";
import Geometry from "ol/geom/Geometry";
import VectorLayer from "ol/layer/Vector";
import VectorSource from "ol/source/Vector";
import { TRUE } from "ol/functions";

/**
 * This class is used to create a layer.
 * @class GOLayer
 * @constructor
 * @param {string} layerId - The layer id.
 * @param {string} layerAlias - The layer alias.
 * @param {string} isBaseLayer - The layer is base layer.
 * @param {string} layerName - The layer name.
 * @param {string} layerType - The layer type.
 * @param {string} layerUrl - The layer url.
 * @param {string} topicId - The topic id.
 */
export abstract class GOLayer {
	protected baseLayer: boolean;
	protected eventEmitter = new EventEmitter();
	protected interactions!: GoLayerInteractions;
	public cqlFilterIsCanceled = false;
	private activeRequests = new Set<CancelablePromise<unknown>>();
	protected layer;
	protected layerAlias: string;
	protected layerId: string;
	protected layerName: string;
	public layerProjection: string | undefined;
	protected layerOpts: types.layerOptions;
	protected layerPromise!: Promise<unknown>;
	protected layerStyle = new GoStyle(this);
	protected layerType: string;
	protected layerUrl: string;
	public olMap: OlMap;
	protected zIndex = 0;
	//protected src = "EPSG:3857";
	public useBboxFilter = false;
	protected stagingMap: GOStagingMap;

	/**
	 * Constructs a GOLayer instance.
	 * @param {OlMap} olMap - The OpenLayers map instance.
	 * @param {types.layerOptions} layerOpts - Layer configuration options.
	 * @param {string} [topicId] - The topic identifier associated with the layer.
	 * @constructor
	 */
	constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {
		const { id, alias, isBaseLayer, layerName, layerType, url } = layerOpts;
		this.layerOpts = layerOpts;
		this.layerAlias = alias;
		this.stagingMap = stagingMap;
		this.olMap = this.stagingMap.getMap();
		this.layerType = layerType || "";
		this.layerId = id;
		this.baseLayer = isBaseLayer || false;
		this.layerName = layerName || "";
		this.layerUrl = url || "";
		if (layerOpts.interactions)
			this.addInteractions(layerOpts.interactions);
	}

	/**
	 * Unmount function
	 * @returns {void}
	 */
	public unmount(): void {
		this.layer = null;
		this.stagingMap = null as unknown as GOStagingMap;
		this.olMap = null as unknown as OlMap;
		this.layerOpts = null as unknown as types.layerOptions;
		this.layerStyle = null as unknown as GoStyle;
		this.layerPromise = null as unknown as Promise<unknown>;
		this.layerAlias = null as unknown as string;
		this.layerId = null as unknown as string;
		this.layerName = null as unknown as string;
		this.layerType = null as unknown as string;
		this.layerUrl = null as unknown as string;
		this.baseLayer = null as unknown as boolean;
		this.eventEmitter.offAll();
	}

	/**
	 * get layerUrl
	 */
	public getLayerUrl() {
		return this.layerUrl;
	}

	/**
	 * Validates the layer configuration and sets default values if necessary.
	 * @param {types.layerOptions} layerOpts - The layer options to validate.
	 * @returns {void}
	 */
	public validateLayerConfig(layerOpts: types.layerOptions): void {
		if (!layerOpts.layerName) {
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.layerName}: parameter layerName not found`
			);
			this.emit(GOLayerEvents.LAYER_ERROR, {
				error:
					internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.layerName}: parameter layerName not found`,
			});
		}
	}

	/**
	 * This function allow to get the map.
	 * @returns {void} Return GOStagingMap
	 * @method
	 */
	public get goMap(): GOStagingMap {
		return this.stagingMap;
	}

	// Abstract functions--------------------------------------------------------------------------------------------------------------------

	/**
	 * This function allow to get the layer.
	 * @returns {void} Return nothing
	 * @method
	 */
	public abstract getLayer();

	/**
	 * This function allow to determine if the layer is visible.
	 * @returns {boolean} Return true if the layer is visible.
	 * @method
	 */
	public abstract getVisibility(): boolean;

	/**
	 * This function determine if the layer can be filtered.
	 * @returns {boolean} Return true if the layer can be filtered.
	 * @method
	 */
	public abstract canFilter(): boolean;

	/**
	 * Checks if the layer is a base layer.
	 * @returns {boolean} True if the layer is a base layer, false otherwise.
	 */
	public isBaseLayer(): boolean {
		return this.baseLayer;
	}

	/**
	 * Changes the opacity of the layer.
	 * @param {number} opacity - The new opacity value (0-100).
	 * @returns {void}
	 */
	public changeLayerOpacity(opacity: number) {
		this.getLayer().setOpacity(opacity / 100);
	}

	/**
	 * Returns a promise that resolves when the layer is fully loaded.
	 * @returns {Promise<unknown>} A promise that resolves when the layer is loaded.
	 */
	public onLayerLoaded(): Promise<unknown> {
		return this.layerPromise;
	}

	/**
	 * Adds interactions to the layer.
	 * @param {LayerInteractions} interactions - The interactions to be added to the layer.
	 * @returns {GoLayerInteractions} The interactions object associated with the layer.
	 */
	public addInteractions(
		interactions: LayerInteractions
	): GoLayerInteractions {
		this.interactions = new GoLayerInteractions(
			this,
			this.olMap,
			interactions
		);
		return this.interactions;
	}

	// Events Handlers----------------------------------------------------------------------------------------------------------------------

	/**
	 * Registers an event listener for the specified event.
	 * @param {string} eventName - The name of the event to listen for.
	 * @param {Function} callback - The callback function to invoke when the event is fired.
	 * @returns {void}
	 */
	public on(eventName: string, callback): void {
		this.eventEmitter.on(eventName, callback);
	}

	/**
	 * Registers a one-time event listener for the specified event.
	 * @param {string} eventName - The name of the event to listen for once.
	 * @param {Function} callback - The callback function to invoke when the event is fired.
	 * @returns {void}
	 */
	public once(eventName: string, callback): void {
		this.eventEmitter.once(eventName, callback);
	}

	/**
	 * Removes an event listener for the specified event.
	 * @param {string} [eventName] - The name of the event to remove the listener from.
	 * @param {Function} [callback] - The callback function to remove.
	 * @returns {void}
	 */
	public off(eventName?: string, callback?): void {
		this.eventEmitter.off(eventName, callback);
	}

	/**
	 * Emits an event to all registered listeners for the specified event.
	 * @param {string} eventName - The name of the event to emit.
	 * @param {...any[]} args - Additional arguments to pass to the event listeners.
	 * @returns {void}
	 */
	public emit(eventName: string, ...args): void {
		MapEventManager.events.fire(eventName, ...args);
		this.eventEmitter.fire(eventName, ...args);
	}

	/**
	 * Checks if there are listeners registered for the specified event.
	 * @param {string} eventName - The name of the event to check for listeners.
	 * @returns {boolean} True if there are listeners for the event, false otherwise.
	 */
	public hasEvent(eventName: string): boolean {
		return this.eventEmitter.has(eventName);
	}

	/**
	 * Gets the event handlers for the specified event.
	 * @param {string} eventName - The name of the event to retrieve handlers for.
	 * @returns {Function[]} An array of event handler functions.
	 */
	public getEventHandlers(eventName: string) {
		return this.eventEmitter.getHandlers(eventName);
	}

	/**
	 * Monitors changes to a specific property of an object and fires an event when the property changes.
	 * @param {Object} obj - The object containing the property to monitor.
	 * @param {string} prop - The name of the property to monitor.
	 * @param {Function} callback - The callback function to invoke when the property changes.
	 * @returns {void}
	 */
	// Create a function that listen for changes in object properties and fire an event when a property is changed.
	public onPropChange(obj, prop: string, callback): void {
		let val = obj[prop];
		Object.defineProperty(obj, prop, {
			get() {
				return val;
			},
			set(newVal) {
				const oldVal = val;
				val = newVal;
				callback(oldVal, newVal);
			},
		});
	}

	public async applyConfigToLayer(layerOpts: layerOptions, bbox) {
		if (!this.layer) {
			console.error("Layer not defined");
			return;
		}

		this.layer.setZIndex(this.zIndex);
		(this.layer as VectorLayer<VectorSource>).set("id", layerOpts.id);
		(this.layer as VectorLayer<VectorSource>).set("filter", null);
		(this.layer as VectorLayer<VectorSource>).set(
			"bbox",
			this.useBboxFilter
		);
		(this.layer as VectorLayer<VectorSource>).set("type", this.type());

		if (layerOpts.filter) {
			if (!bbox) {
				this.addFilter(layerOpts.filter, this.useBboxFilter);
			} else {
				this.addFilter(layerOpts.filter, bbox);
			}
		}

		(this.layer as VectorLayer<VectorSource>).set(
			"geometryName",
			this.layerOpts.geometryField || "geom"
		);

		if (layerOpts.minResolution) {
			this.setMinResolution(layerOpts.minResolution);
		}

		if (layerOpts.maxResolution) {
			this.setMaxResolution(layerOpts.maxResolution);
		}

		this.setLayerStyle();
	}

	public type(): string {
		return this.layerType;
	}

	/**
	 * Checks if this layer currently has a filter applied.
	 * @returns {boolean} True if a filter is applied, false otherwise.
	 */
	public hasFilter(): boolean {
		if (!this.getLayer()) return false;
		const currentFilter = this.getLayer().get("filter");
		return currentFilter !== null && currentFilter !== undefined;
	}

	public addFilter(filter: string, bbox = false) {
		return new CancelablePromise((resolve, reject, onCancel) => {
			this.cancelActiveRequests();
			if (!bbox) {
				this.getLayer().set("bbox", false);
			}
			this.getLayer().set("filter", this.formatFilter(filter, bbox));

			const isCluster = this.getLayer().getSource()?.getSource;
			if (isCluster) {
				this.getLayer()
					.getSource()
					.getSource()
					.refresh({ force: true });
			} else {
				this.getLayer().getSource().refresh({ force: true });
			}

			this.cqlFilterIsCanceled = false;

			onCancel(() => {
				this.cqlFilterIsCanceled = true;
				reject();
			});
			resolve(null);
		});
	}
	/**
	 * This function allow to format the filter.
	 * @param {string} filter - The filter to format.
	 * @returns {string} Return the formatted filter.
	 * @method formatFilter
	 */
	public formatFilter(filter: string, bbox?: boolean): string {
		if (filter && !filter.trim().toUpperCase().startsWith("AND") && bbox) {
			return ` AND (${filter})`;
		}

		// Replace the first AND in adapterFilter
		return filter;
	}

	/**
	 * Removes the filter applied to the layer, resetting it to the default state.
	 * Forces the layer to refresh after removing the filter.
	 * @returns {void}
	 */
	public removeFilter(): void {
		this.cancelActiveRequests();
		this.getLayer().set("filter", null);
		this.cqlFilterIsCanceled = false;
		const isCluster = this.getLayer().getSource()?.getSource;
		if (isCluster) {
			this.getLayer().getSource().getSource().refresh({ force: true });
		} else {
			this.getLayer().getSource().refresh({ force: true });
		}
	}

	public trackRequest<T>(
		request: CancelablePromise<T>
	): CancelablePromise<T> {
		this.activeRequests.add(request as CancelablePromise<unknown>);
		request.finally(() => {
			this.activeRequests.delete(request as CancelablePromise<unknown>);
		});
		return request;
	}

	public cancelActiveRequests(): void {
		if (this.activeRequests.size === 0) {
			return;
		}
		this.activeRequests.forEach((request) => {
			if (typeof (request as CancelablePromise<unknown>).cancel === "function") {
				(request as CancelablePromise<unknown>).cancel();
			}
		});
		this.activeRequests.clear();
	}

	public consumeCancellationFlag(): boolean {
		if (!this.cqlFilterIsCanceled) {
			return false;
		}
		this.cqlFilterIsCanceled = false;
		return true;
	}

	/**
	 * Gets the OpenLayers map instance associated with the layer.
	 * @returns {OlMap} The OpenLayers map instance.
	 */
	public getOlMap(): OlMap {
		return this.olMap;
	}

	/**
	 * Gets the options used to configure the layer.
	 * @returns {types.layerOptions} The layer configuration options.
	 */
	public getLayerOpts(): layerOptions {
		return this.layerOpts;
	}

	/**
	 * Gets the alias of the layer.
	 * @returns {string} The alias of the layer.
	 */
	public getAlias(): string {
		return this.layerAlias;
	}

	/**
	 * Gets the unique identifier of the layer.
	 * @returns {string} The layer ID.
	 */
	public getId(): string {
		return this.layerId;
	}

	/**
	 * Gets the name of the layer.
	 * @returns {string} The name of the layer.
	 */
	public getLayerName(): string {
		return this.layerName;
	}

	/**
	 * Gets the URL associated with the layer.
	 * @returns {string} The URL of the layer.
	 */
	public getUrl(): string {
		return this.layerUrl;
	}

	/**
	 * Gets the type of the layer.
	 * @returns {string} The layer type.
	 */
	public getLayerType(): string {
		return this.layerType;
	}

	/**
	 * Gets the z-index of the layer.
	 * @returns {number} The z-index value.
	 */
	public getZIndex(): number {
		return this.getLayer().getZIndex();
	}

	/**
	 * Gets the maximum resolution at which the layer is visible.
	 * @returns {number} The maximum resolution.
	 */
	public getMaxResolution(): number {
		return this.getLayer().getMaxResolution();
	}

	/**
	 * Gets the minimum resolution at which the layer is visible.
	 * @returns {number} The minimum resolution.
	 */
	public getMinResolution(): number {
		return this.getLayer().getMinResolution();
	}

	/**
	 * Retrieves features from the layer by a specific attribute and condition.
	 * @param {string} field - The field name to filter by.
	 * @param {GoFilterCondition} condition - The condition to apply for filtering.
	 * @param {string} value - The value to match against the field.
	 * @returns {Promise<Array<Feature<Geometry>>>} A promise that resolves with the matching features.
	 */

	/*
		CASOS A REVISAR:
		1-. GO_VECTOR_LAYER: filtro cql
		2-. GO_CLUSTER_LAYER con layerName: filtro cql
		3-. GO_WEBGL_LAYER con sourceType="GEOSERVER": filtro cql
		4-. GO_GEOJSON_LAYER: filtro sobre toda la source
		5-. GO_CLUSTER_LAYER con empty=true: filtro sobre toda la source
  		6-. GO_WEBGL_LAYER con sourceType="GEOJSON": filtro sobre toda la source
	*/

	public async getFeaturesByAttribute(
		field: string,
		condition: GoFilterCondition,
		value: string
	) {
		let selectedFeatures: Array<Feature<Geometry>> = [];

		const layerOpts = this.getLayerOpts();
		let searchByCqlFilter = false;

		//Check if we should search by clq filter or by feature properties
		if (
			layerOpts.layerType === GOLayerType.GO_VECTOR_LAYER ||
			(layerOpts.layerType === GOLayerType.GO_CLUSTER_LAYER &&
				layerOpts.layerName) ||
			(layerOpts.layerType === GOLayerType.GO_WEBGL_LAYER &&
				layerOpts.sourceType === "GEOSERVER")
		) {
			searchByCqlFilter = true;
		}

		if (searchByCqlFilter) {
			const geoserverUrl = layerOpts.url
				? layerOpts.url +
				  "/geoserver/" +
				  layerOpts.layerName?.split(":")[0]
				: `${GoStore.getState().geoserverAuthData?.url}/geoserver/${
						layerOpts.layerName?.split(":")[0]
				  }`;

			const service = `/wfs?service=WFS&version=1.0.0&request=GetFeature&typeName=${layerOpts.layerName}`;
			const outputFormat = `&outputFormat=application/json&srsname=EPSG:${layerOpts.layerProjection}`;
			const cqlFilter = `&cql_filter=`;

			const filter = this.__applyCQLSelectionCondition({
				field,
				condition,
				value,
			});

			let cqlUrl = `${geoserverUrl}${service}${outputFormat}${cqlFilter}${encodeURIComponent(
				filter
			)}`;

			cqlUrl = cqlUrl.toString();

			const response = (await this.trackRequest(
				GoGeoServices.getGISApiRestFeatureCollection({
					url: getUrlWithoutParams(cqlUrl),
					data: getParamsFromUrl(cqlUrl),
					customGeoserverURL: cqlUrl,
					serviceType: "wfs",
				})
			)) as AxiosResponse;

			const geojsonFormat = new GeoJSON();

			selectedFeatures = response.data.features.map((feature) =>
				geojsonFormat.readFeature(feature)
			);
		} else {
			let features = this.getLayer().getSource().getFeatures();

			if (layerOpts.layerType === GOLayerType.GO_CLUSTER_LAYER) {
				// If the layer is a cluster layer, we need to get the source of the cluster
				features = this.getLayer()
					.getSource()
					.getSource()
					.getFeatures();
			}

			features.forEach((feature: Feature<Geometry>) => {
				const matchSelection = this.__applySelectionCondition(
					{
						field,
						condition,
						value,
					},
					feature.getProperties()[field]
				);

				if (matchSelection) {
					selectedFeatures.push(feature);
				}
			});
		}

		return selectedFeatures;
	}

	/**
	 * Gets the geographical extent of the layer.
	 * @returns {Array<number> | null} The extent as an array of numbers or null if the extent is not defined.
	 */
	public getExtent(): Array<number> | null {
		let extent;

		if (this.layerOpts.layerType === GOLayerType.GO_CLUSTER_LAYER) {
			extent = this.getLayer().getSource().getSource().getExtent();
		} else if (this.layerOpts.layerType === GOLayerType.GO_REMOTE_SENSING) {
			extent = this.getLayer().getSource().tileGrid.getExtent();
		} else {
			extent = this.getLayer().getSource().getExtent();
		}
		if (extent[0] !== Infinity) {
			return extent;
		} else {
			return null;
		}
	}

	/**
	 * Gets the options used to configure the layer.
	 * @returns {layerOptions} The layer configuration options.
	 */
	public getOptions(): layerOptions {
		return this.layerOpts;
	}

	/**
	 * Gets the style associated with the layer.
	 * @returns {GoStyle} The style of the layer.
	 */
	public getLayerStyle(): GoStyle {
		return this.layerStyle;
	}

	/**
	 * Get the projection code of the layer.
	 * @returns {string} The projection code of the layer.
	 * @method
	 */
	public getLayerProjection(): string {
		return this.layerProjection;
	}

	/**
	 * Sets the visibility of the layer.
	 * @param {boolean} visibility - True to make the layer visible, false to hide it.
	 * @returns {void}
	 */
	public setVisibility(visibility: boolean): void {
		this.getLayer().setVisible(visibility);
	}

	/**
	 * Sets the z-index of the layer.
	 * @param {number} zIndex - The z-index value to set.
	 * @returns {void}
	 */
	public setZIndex(zIndex: number): void {
		this.getLayer().setZIndex(zIndex);
	}

	/**
	 * Sets the maximum resolution at which the layer is visible.
	 * @param {number} maxResolution - The maximum resolution value.
	 * @returns {void}
	 */
	public setMaxResolution(maxResolution: number): void {
		this.getLayer().setMaxResolution(maxResolution);
	}

	/**
	 * Sets the minimum resolution at which the layer is visible.
	 * @param {number} minResolution - The minimum resolution value.
	 * @returns {void}
	 */
	public setMinResolution(minResolution: number): void {
		this.getLayer().setMinResolution(minResolution);
	}

	/**
	 * Sets the style of the layer and optionally adds annotations if applicable.
	 * @param {Array<GoGlobalStyle> | GoGlobalStyle} [style] - The style or array of styles to apply.
	 * @returns {void | Promise} - Returns nothing or a promise depending on the type of style applied.
	 */
	public setLayerStyle(style?: Array<GoGlobalStyle> | GoGlobalStyle) {
		try {
			const layerStyles = this.layerStyle.setStyle(style);
			// Check if layerStyles is a promise otherwise put directly the annotations
			if (
				layerStyles instanceof Promise ||
				layerStyles instanceof CancelablePromise
			) {
				layerStyles.then(() => {
					// Check if the layer has setAnnotations method
					if (
						this.getLayerType() === GOLayerType.GO_CLUSTER_LAYER ||
						this.getLayerType() === GOLayerType.GO_GEOJSON_LAYER ||
						this.getLayerType() === GOLayerType.GO_VECTOR_LAYER
					) {
						this.getLayerStyle().getAnnotations()!.addAnnotations();
					}
				});
			} else {
				// Check if the layer has setAnnotations method
				if (
					this.getLayerType() === GOLayerType.GO_CLUSTER_LAYER ||
					this.getLayerType() === GOLayerType.GO_GEOJSON_LAYER ||
					this.getLayerType() === GOLayerType.GO_VECTOR_LAYER
				) {
					this.getLayerStyle().getAnnotations()!.addAnnotations();
				}
				return layerStyles;
			}
		} catch (error) {
			console.error(error);
		}
	}

	private __applyCQLSelectionCondition(filter: GoFilter): string {
		let sendFilter = "";
		switch (filter.condition) {
			case GoFilterCondition.EQUAL_TO:
				sendFilter = `${filter.field}='${filter.value}'`;
				break;
			case GoFilterCondition.NOT_EQUAL_TO:
				sendFilter = `${filter.field}<>'${filter.value}'`;
				break;
			case GoFilterCondition.BETWEEN:
				sendFilter = `${filter.field} BETWEEN '${
					(filter.value as [number, number])[0]
				}' AND '${(filter.value as [number, number])[1]}'`;
				break;

			case GoFilterCondition.NOT_BEETWEEN:
				sendFilter = `${filter.field} NOT BETWEEN '${
					(filter.value as [number, number])[0]
				}' AND '${(filter.value as [number, number])[1]}'`;
				break;

			case GoFilterCondition.GREATER_THAN:
				sendFilter = `${filter.field}>'${filter.value}'`;
				break;

			case GoFilterCondition.GREATER_THAN_OR_EQUAL_TO:
				sendFilter = `${filter.field}>='${filter.value}'`;
				break;

			case GoFilterCondition.LESS_THAN:
				sendFilter = `${filter.field}<'${filter.value}'`;
				break;

			case GoFilterCondition.LESS_THAN_OR_EQUAL_TO:
				sendFilter = `${filter.field}<='${filter.value}'`;
				break;

			case GoFilterCondition.IS_NULL:
				sendFilter = `${filter.field} IS NULL`;
				break;

			case GoFilterCondition.IS_NOT_NULL:
				sendFilter = `${filter.field} IS NOT NULL`;
				break;

			case GoFilterCondition.IS_EMPTY:
				sendFilter = `${filter.field}=''`;
				break;

			case GoFilterCondition.IS_NOT_EMPTY:
				sendFilter = `${filter.field}<>''`;
				break;

			case GoFilterCondition.IS_IN:
				sendFilter = `${filter.field} IN ${filter.value}`;
				break;

			case GoFilterCondition.IS_NOT_IN:
				console.warn("__applyCQLFilterCondition: Not implemented");
				sendFilter = ``;
				break;

			case GoFilterCondition.IS_LIKE:
				sendFilter = `strToLowerCase(${
					filter.field
				}) like '${filter.value.toLowerCase()}'`;
				break;
			case GoFilterCondition.IS_NOT_LIKE:
				sendFilter = `strToLowerCase(${
					filter.field
				}) not like '${filter.value.toLowerCase()}'`;
				break;

			default:
				console.warn("__applyCQLFilterCondition: Not implemented");
				break;
		}
		return sendFilter;
	}

	private __applySelectionCondition(
		filter: GoFilter,
		value: string | number | Array<string | number>
	): boolean {
		let meetsCondition = false;
		switch (filter.condition) {
			case GoFilterCondition.EQUAL_TO:
				meetsCondition = value === filter.value;
				break;

			case GoFilterCondition.NOT_EQUAL_TO:
				meetsCondition = value !== filter.value;
				break;

			case GoFilterCondition.BETWEEN:
				meetsCondition =
					typeof value === "number" &&
					value >= (filter.value as [number, number])[0] &&
					typeof value === "number" &&
					value <= (filter.value as [number, number])[1];
				break;

			case GoFilterCondition.NOT_BEETWEEN:
				meetsCondition =
					(typeof value === "number" &&
						value < (filter.value as [number, number])[0]) ||
					(typeof value === "number" &&
						value > (filter.value as [number, number])[1]);
				break;

			case GoFilterCondition.GREATER_THAN:
				meetsCondition = value > filter.value;
				break;

			case GoFilterCondition.GREATER_THAN_OR_EQUAL_TO:
				meetsCondition = value >= filter.value;
				break;

			case GoFilterCondition.LESS_THAN:
				meetsCondition = value < filter.value;
				break;

			case GoFilterCondition.LESS_THAN_OR_EQUAL_TO:
				meetsCondition = value <= filter.value;
				break;

			case GoFilterCondition.IS_NULL:
				meetsCondition = value === null;
				break;

			case GoFilterCondition.IS_NOT_NULL:
				meetsCondition = value !== null;
				break;

			case GoFilterCondition.IS_EMPTY:
				meetsCondition = value === "";
				break;

			case GoFilterCondition.IS_NOT_EMPTY:
				meetsCondition = value !== "";
				break;

			case GoFilterCondition.IS_IN:
				meetsCondition = (filter.value as string).includes(
					value as string
				);
				break;

			case GoFilterCondition.IS_NOT_IN:
				meetsCondition = !(filter.value as string).includes(
					value as string
				);
				break;

			case GoFilterCondition.IS_LIKE:
				meetsCondition = (value as string).includes(
					filter.value as string
				);
				break;
			case GoFilterCondition.IS_NOT_LIKE:
				meetsCondition = !(value as string).includes(
					filter.value as string
				);
				break;

			default:
				console.warn("__applyFilterCondition: Not implemented");
				break;
		}
		return meetsCondition;
	}

	public getFeaturesAtPixel(
		pixel: [number, number]
	): Feature<Geometry>[] | Array<Feature<Geometry>>[] {
		let filteredFeatures: Array<Feature<Geometry>> = [];
		const ref = this.layerOpts;

		if (this.layerType === GOLayerType.GO_WEBGL_LAYER) {
			const coordinate = this.olMap.getCoordinateFromPixelInternal(pixel);

			if (!coordinate) {
				console.warn(
					"getFeaturesAtPixel: No coordinate found for pixel",
					pixel
				);
				return [];
			}

			const coordinatesFromFeature =
				this.olMap.renderer_.forEachFeatureAtCoordinate(
					coordinate,
					this.olMap.frameState_,
					0,
					false,
					function (feature) {
						return feature
							? feature.getGeometry().getCoordinates()
							: null;
					},
					null,
					TRUE,
					null
				);

			if (!coordinatesFromFeature) {
				return [];
			}

			const features = this.sourceLayer
				?.getSource()
				?.getFeaturesAtCoordinate(coordinatesFromFeature);

			// Check applied noCql filters
			filteredFeatures = features;

			if (this._webglStyleFilters.size) {
				filteredFeatures = [];

				features?.forEach((f) => {
					this._webglStyleFilters.forEach((filter) => {
						const featureMeetsConditions =
							this.getLayerStyle().isFeatureInFilter(
								filter.filter,
								f
							);

						if (featureMeetsConditions) {
							filteredFeatures?.push(f);
						}
					});
				});
			}
		} else if (this.layerType === GOLayerType.GO_CLUSTER_LAYER) {
			const clickedFeatures = this.olMap.getFeaturesAtPixel(pixel, {
				layerFilter: function (layer) {
					return layer.get("id") === ref.id;
				},
			});

			clickedFeatures.forEach((feature) => {
				feature.getProperties().features.forEach((f) => {
					filteredFeatures.push(f);
				});
			});
		} else {
			filteredFeatures = this.olMap.getFeaturesAtPixel(pixel, {
				layerFilter: function (layer) {
					return layer.get("id") === ref.id;
				},
			});
		}
		return filteredFeatures;
	}
}

---
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\GoVectorLayer\index.ts
import * as types from "@goTypes/index";
import { GOStagingMap } from "@map/index";
import { GoAxiosServices, GoGeoServices, GoStore } from "@services/index";
import { goErrorsHandlers } from "@utils/classes/GoError";
import * as constants from "@utils/classes/GoError/constants";
import * as errrosMsg from "@utils/classes/GoError/errrosMsg";
import {
	GOFeaturesEvents,
	GOGeometryType,
	GOLayerEvents,
} from "@utils/constants";
import {
	getObjectParamsFromUrl,
	getParamsFromUrl,
	getUrlWithoutParams,
	nextFrame,
} from "@utils/functions";
import { blobToDataURL } from "@utils/index";
import { memorize } from "@utils/functions/mathFunctions";
import { GoIcons, StyleUtils } from "@utils/index";
import { CancelablePromise } from "cancelable-promise";
import { Feature, Map as OlMap } from "ol";
import { AxiosResponse } from "axios";
import HexBin from "ol-ext/source/HexBin";
import { Extent } from "ol/extent";
import GeoJSON from "ol/format/GeoJSON";
import Geometry from "ol/geom/Geometry";
import Point from "ol/geom/Point";
import VectorLayer from "ol/layer/Vector";
import VectorImageLayer from "ol/layer/VectorImage";
import { bbox as bboxStrategy, all as allStrategy } from "ol/loadingstrategy";
import { transformExtent } from "ol/proj";
import { Vector } from "ol/source";
import VectorSource from "ol/source/Vector";
import Style, { StyleLike } from "ol/style/Style";
import "ol/style/Stroke";
import { GOStyledLayer } from "../GoStyledLayer";

/**
 * This class is used for create vector layers.
 * @class GOVectorLayer
 * @augments  GOStyledLayer
 * @constructor
 * @param {import('types').layerOptions} layerOpts - The layer options.
 * @param {string} topicId - The topic id.
 */
export class GOVectorLayer extends GOStyledLayer {
	protected geomName!: string;
	protected zIndex = 1;
	private map: OlMap;
	protected mapId: string | undefined;
	protected styleSVG: string | undefined;

	public layerProjection: string | undefined;
	public toProjection: string | undefined;
	public useBboxFilter: boolean;

	constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {
		super(stagingMap, layerOpts);
		this.map = this.olMap;
		this.mapId = this.stagingMap.getId();
		this.layerProjection = layerOpts.layerProjection
			? layerOpts.layerProjection.split(":").pop()?.toString()
			: "4326";
		this.toProjection = layerOpts.toProjection
			? layerOpts.toProjection.split(":").pop()?.toString()
			: undefined;
		this.useBboxFilter =
			layerOpts.useBboxFilter === undefined
				? true
				: layerOpts.useBboxFilter;
		this._initLayer();
	}

	/**
	 * Initializes the vector layer with options and configuration.
	 * @returns {CancelablePromise<GOVectorLayer>} A promise that resolves with the initialized vector layer.
	 */
	private _initLayer() {
		const { layerOpts } = this;

		if (this.consumeCancellationFlag()) {
			return;
		}

		this.emit(GOLayerEvents.LAYER_LOADING, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});

		if (layerOpts.styleSVG) this.setIcons();
		if (layerOpts.styleSVGConditional)
			this.setStyleValues(layerOpts.styleSVGConditional.styleInfoSVG);

		if (
			layerOpts.defaultAnnotations &&
			layerOpts.defaultAnnotations.length
		) {
			this.setDefaultAnnotations();
		}

		this.mapStyle.set("initStyle", new Map<string, Style | types.styleOptions | types.styleFromUser | types.styleSVG | types.styleConditionSVG | types.styleGeojson | types.ISVGIcon | StyleLike | null>());
		this.mapStyle.get("initStyle")!.set("", null);
		this.layerOpts = layerOpts;

		const opacity = layerOpts.opacity || 1;
		const visibility: boolean =
			layerOpts.defaultVisibility === null ||
			layerOpts.defaultVisibility === undefined
				? true
				: layerOpts.defaultVisibility;
		this.zIndex = layerOpts.zIndex || 1;

		this.validateLayerConfig(layerOpts);

		const vectorSource = new Vector({
			format: new GeoJSON(),
			loader: this.loader.bind(this),
			strategy: this.useBboxFilter ? bboxStrategy : allStrategy,
		});

		if (layerOpts.disableImageEffect) {
			this.layer = new VectorLayer({
				source: vectorSource,
				visible: visibility,
				opacity,
			});
		} else {
			this.layer = new VectorImageLayer({
				source: vectorSource,
				visible: visibility,
				opacity,
			});
		}

		this.applyConfigToLayer(layerOpts, undefined);

		this.layerPromise = Promise.resolve(this.layer);
	}

	/**
	 * Checks if the data has been loaded within a specified time interval.
	 * @param {number} [timeInterval=1000] - The time interval in milliseconds between checks.
	 * @param {number} [limit=0] - The maximum number of intervals before giving up.
	 * @returns {CancelablePromise<boolean>} A promise that resolves with `true` if data is loaded, otherwise `false`.
	 */
	public isLoadedData(timeInterval = 1000, limit = 0) {
		return new CancelablePromise((resolve, reject) => {
			try {
				// eslint-disable-next-line
				const intervals: Array<any> = [];
				let times = 0;
				const interval = setInterval(() => {
					times++;
					intervals.push(interval);
					const features = this.layer.getSource().getFeatures();
					if (limit === times) {
						this.clearAllIntervals(intervals);
						resolve(false);
					} else if (features.length) {
						this.clearAllIntervals(intervals);
						resolve(true);
					}
				}, timeInterval);
			} catch (error) {
				reject();
			}
		});
	}

	/**
	 * Refreshes the layer source.
	 * @returns {void}
	 */
	public refresh(): void {
		this.layer.getSource().refresh();
	}

	/**
	 * Changes the style of the layer based on the provided style information.
	 * @param {types.styleFromUser} style - The style information to apply to the layer.
	 * @returns {void}
	 */
	public changeStyle(style: types.styleFromUser): void {
		try {
			const geometry = this.getGeometryType(
				this.layer.getSource().getFeatures()[0].getGeometry().getType()
			);

			if (geometry === style.type) {
				this.layer.setStyle(
					memorize((style) => StyleUtils.newStyle(style))(style)
				);
				this.emit(GOLayerEvents.LAYER_STYLE_CHANGED, {
					mapId: this.mapId,
					layerId: this.layer.id,
					layer: this,
					style: this.layer.getStyle(),
				});
			} else {
				// Error Handler
				goErrorsHandlers.stylesErrorHandler.set(
					constants.ErrorSeverity.Warning,
					constants.stylesErrors.STYLES_ERROR_CANT_CREATE_STYLES,
					errrosMsg.stylesErrorsMsg.STYLES_ERROR_CANT_CREATE_STYLES +
						`Cap:${geometry} Geometry style: ${style.type}.`
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					error:
						errrosMsg.stylesErrorsMsg
							.STYLES_ERROR_CANT_CREATE_STYLES +
						`Cap:${geometry} Geometry style: ${style.type}.`,
				});
			}
		} catch (error: unknown) {
			const errorMessage = error instanceof Error ? error.message : String(error);
			// Error Handler
			goErrorsHandlers.stylesErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.stylesErrors.STYLES_ERROR_GEOMETRY_STYLE,
				errorMessage,
				error instanceof Error ? error : new Error(errorMessage)
			);
			this.emit(GOLayerEvents.LAYER_ERROR, {
				error: error instanceof Error ? error : new Error(errorMessage),
			});
		}
	}

	public unmount(): void {
		this.layer.getSource().clear();
		this.layer.setStyle(null);
		this.layer.setVisible(false);
		this.layer.setOpacity(0);
		this.layer.setZIndex(0);
		this.layer.set("filter", null);
		this.layer.set("bbox", null);
		this.layer = null;
	}

	/**
	 * Restores the layer to its default state by clearing filters and refreshing the layer.
	 * @returns {void}
	 */
	public restoreLayer(): void {
		const layer = this.getLayer();
		if (!layer) {
			return;
		}
		const source = layer.getSource();
		if (!source) {
			return;
		}
		//delete filter
		layer.set("filter", null);
		// //refresh layer
		source.refresh();
		this.restoreStyle();
	}

	/**
	 * Checks if the layer has a filter applied.
	 * @returns {boolean} `true` if a filter is applied, otherwise `false`.
	 */
	public hasFilter(): boolean {
		return this.getLayer().get("filter") !== null;
	}

	/**
	 * Checks if the layer supports filtering.
	 * @returns {boolean} `true` if filtering is supported, otherwise `false`.
	 */
	public canFilter(): boolean {
		return true;
	}

	/**
	 * Returns the type of the layer.
	 * @returns {string} The layer type.
	 */
	public type(): string {
		return "GOVectorLayer";
	}

	/**
	 * Exports the features of the layer in GeoJSON format.
	 * @returns {string | null} The GeoJSON string representation of the layer's features, or `null` if there are no features.
	 */
	public exportGeoJsonFormatFeatures() {
		// eslint-disable-next-line
		const geom: Array<any> = [];
		const writer = new GeoJSON();
		this.layer.getSource().forEachFeature(function (feature) {
			geom.push(new Feature(feature.getGeometry()));
		});
		if (geom.length) {
			const geoJsonStr = writer.writeFeatures(geom);
			return geoJsonStr;
		} else {
			return null;
		}
	}

	/**
	 * Checks if a given string represents a valid geometry type.
	 * @param {string} type - The geometry type to check.
	 * @returns {boolean} `true` if the type is a valid geometry type, otherwise `false`.
	 */
	public isGeometryType(type: string): boolean {
		return (
			type.toUpperCase() === "GEOMETRY" ||
			type.toUpperCase() === "MULTILINESTRING" ||
			type.toUpperCase() === "LINESTRING" ||
			type.toUpperCase() === "MULTIPOINT" ||
			type.toUpperCase() === "POINT" ||
			type.toUpperCase() === "POLYGON" ||
			type.toUpperCase() === "MULTIPOLYGON"
		);
	}

	private transformExtend = memorize(
		({
			extent,
			srcProjection,
			toProjection,
		}: {
			extent: Extent;
			srcProjection: string;
			toProjection: string;
		}): Extent => transformExtent(extent, srcProjection, toProjection)
	);

	/**
	 * Loads features from the server within the specified extent and updates the layer.
	 * @param {Extent} extent - The extent within which to load features.
	 * @returns {Promise<any>} A promise that resolves with the response data from the server.
	 * @throws {Error} Throws an error if the request fails or if the response indicates a problem.
	 */
	// eslint-disable-next-line
	public async loader(extent: Extent): Promise<any> {
		const { layerOpts } = this;

		if (this.consumeCancellationFlag()) {
			return;
		}

		if (!this.layer) {
			console.warn(
				`[${layerOpts.id}] Vector loader triggered before layer initialization. Skipping current load cycle.`
			);
			return;
		}

		const layerSource = this.layer.getSource();
		if (!layerSource) {
			console.warn(
				`[${layerOpts.id}] Vector loader cannot proceed because the layer source is not ready yet.`
			);
			return;
		}

		this.emit(GOFeaturesEvents.FEATURES_LOAD_STARTED, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});

		await nextFrame();
		let url: string;
		let url_Own: string;

		const mapProjection = this.map
			.getView()
			.getProjection()
			.getCode()
			.split(":")
			.pop()
			?.toString();
		if (mapProjection !== this.layerProjection) {
			extent = this.transformExtend({
				extent,
				srcProjection: `EPSG:${mapProjection}`,
				toProjection: `EPSG:${this.layerProjection}`,
			});
		}

		const geomName: string = await this.layer.get("geometryName");

		const bBox = `cql_filter=BBOX(${geomName},${extent.join(",")})&`;

		const service = `/wfs?service=WFS&version=1.0.0&request=GetFeature&typeName=${this.layerOpts.layerName}`;
		const outputFormat = `outputFormat=application/json&srsname=EPSG:${mapProjection}`;

		let propertyName = "";
		if (this.layerOpts.propertyName) {
			propertyName = `&propertyName=${geomName},${this.layerOpts.propertyName.toString()}`;
		}

		const filterNoBbox = "cql_filter=";
		const filter: string = this.layer.get("filter");

		// Check URL origin of data
		let geoserverUrlEnv = GoStore.getState().geoserverAuthData?.url;

		if (this.layerOpts.url) {
			geoserverUrlEnv = this.layerOpts.url;
		}
		const geoserverUrl = `${geoserverUrlEnv}/geoserver/${
			this.layerOpts.layerName?.split(":")[0]
		}`;

		if (this.layer.get("filter")) {
			if (this.layer.get("bbox")) {
				const bBoFilter = bBox.replace("&", "");
				url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&${bBoFilter}${encodeURIComponent(
					filter
				)}`;
				url = url + "&";
				url_Own = url.toString();
			} else {
				url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&${filterNoBbox}${encodeURIComponent(
					filter
				)}`;
				url = url + "&";
				url_Own = url.toString();
			}
		} else {
			if (this.useBboxFilter) {
				url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&${bBox}`;
				url_Own = url.toString();
			} else {
				url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&`;
				url_Own = url.toString();
			}
		}

		// Add geoparams if provided
		if (this.layerOpts.geoparams) {
			url += `${this.layerOpts.geoparams}&`;
			url_Own = url;
		}

		try {
			const responsePromise = this.trackRequest(
				GoGeoServices.getGISApiRestFeatureCollection({
					url: getUrlWithoutParams(url_Own),
					data: getParamsFromUrl(url_Own),
					customGeoserverURL: this.layerOpts.url ? url_Own : null,
					serviceType: "wfs",
				})
			);

			const response = await responsePromise as AxiosResponse;

			if (
				typeof response.data === "string" &&
				response.data.includes("?xml") &&
				response.data.includes("ServiceExceptionReport")
			) {
				if (!layerSource.get("firstTime")) {
					// Error Handler
					goErrorsHandlers.internalErrorHandler.set(
						constants.ErrorSeverity.Warning,
						constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
						errrosMsg.internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER
					);
					this.emit(GOLayerEvents.LAYER_ERROR, {
						error: errrosMsg.internalErrosMsg
							.INTERNAL_ERROR_CAN_LOAD_LAYER,
					});
					layerSource.set("firstTime", true);
				}
			} else {
				const typeLayer = this.type();

				if (response.data !== undefined) {
					if (typeLayer === "GOHexGridLayer") {
						//If hexgrid points, normal Hexgrid
						const layerType =
							response.data.features[0]?.geometry.type;
						(this.layer.getSource() as HexBin).getSource().clear();

						if (layerType == "MultiPoint" || layerType == "Point") {
							(this.layer.getSource() as HexBin)
								.getSource()
								.addFeatures(
									(this.layer.getSource() as HexBin)
										.getSource()
										.getFormat()
										.readFeatures(response.data)
										.filter((feat) =>
											feat.getGeometry()
										) as Feature<Geometry>[]
								);
						} else if (
							layerType == "MultiLineString" ||
							layerType == "LineString"
						) {
							//If hexgrid lines, add extra points
							const myFeatures = (
								this.layer.getSource() as HexBin
							)
								.getSource()
								.getFormat()
								.readFeatures(response.data)
								.filter((feat) => feat.getGeometry());

							const sizeHex = (this as unknown as { hexSize: number }).hexSize;

							const myNewFeatures: Feature<Geometry>[] = [];

							myFeatures.forEach((f) => {
								const length = f.getGeometry().getLength();
								if (length > sizeHex) {
									//Initial point
									const initialFeature: Feature<Geometry> =
										new Feature();
									const initialTramoCoordinates = f
										.getGeometry()
										.getFirstCoordinate();
									const initialGeometry = new Point(
										initialTramoCoordinates
									);

									initialFeature.setGeometry(initialGeometry);
									initialFeature.setProperties({
										sector: f.getProperties().sector,
										originLineString: true,
									});

									myNewFeatures.push(initialFeature);

									//Calculo cantidad de tramos a insertar en función de la longitud del elemento y del hexgrid
									const tramos = length / sizeHex;
									const tramoSize = 1 / tramos;
									//para cada tramo insertamos elemento
									for (let i = 0; i <= tramos; i++) {
										const feature: Feature<Geometry> =
											new Feature();
										const tramoCoordinates = f
											.getGeometry()
											.getCoordinateAt(i * tramoSize);
										const geometry = new Point(
											tramoCoordinates
										);

										feature.setGeometry(geometry);
										feature.setProperties({
											sector: f.getProperties().sector,
											originLineString: true,
										});

										myNewFeatures.push(feature);
									}

									//Last point
									const lastFeature: Feature<Geometry> =
										new Feature();
									const lastTramoCoordinates = f
										.getGeometry()
										.getLastCoordinate();
									const lastGeometry = new Point(
										lastTramoCoordinates
									);

									lastFeature.setGeometry(lastGeometry);
									lastFeature.setProperties({
										sector: f.getProperties().sector,
										originLineString: true,
									});

									myNewFeatures.push(lastFeature);
								} else {
									f.setProperties({
										originLineString: true,
									});
									myNewFeatures.push(f as Feature<Geometry>);
								}
							});
							(this.layer.getSource() as HexBin)
								.getSource()
								.addFeatures(myNewFeatures);
						}
					} else {
						this.layer.getSource().clear();
						this.layer.getSource().addFeatures(
							this.layer
								.getSource()
								.getFormat()
								.readFeatures(response.data)
								.filter((feat) =>
									feat.getGeometry()
								) as Feature<Geometry>[]
						);
					}
				}
				this.layer.getSource().set("firstTime", false);
			}

			this.selectChboxToc(this.layerOpts);

			this.emit(GOFeaturesEvents.FEATURES_LOAD_ENDED, {
				mapId: this.mapId,
				layerId: layerOpts.id,
			});

			return response;
		} catch (e) {
			const layerSource = this.layer?.getSource?.();
			if (layerSource && !layerSource.get("firstTime")) {
				const errorMessage = e instanceof Error ? e.message : String(e);
				// Error handler
				goErrorsHandlers.internalErrorHandler.set(
					constants.ErrorSeverity.Error,
					constants.internalErrors.INTERNAL_ERROR_FIELD,
					errrosMsg.internalErrosMsg.INTERNAL_ERROR_FIELD +
						`${this.layerOpts.id}: ${errorMessage}`
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					error:
						errrosMsg.internalErrosMsg.INTERNAL_ERROR_FIELD +
						`${this.layerOpts.id}: ${errorMessage}`,
				});
				layerSource.set("firstTime", true);

				this.emit(GOFeaturesEvents.FEATURES_LOAD_ERROR, {
					mapId: this.mapId,
					layerId: this.layerOpts.id,
				});
			}
		}
	}

	/**
	 * Selects the checkbox in the Table of Contents (TOC) for the layer if it is created and the layer should be visible.
	 * @param {object} options - The options for the layer.
	 * @param {boolean} [options.defaultVisibility] - The default visibility of the layer.
	 * @returns {void} This method does not return anything.
	 */
	public selectChboxToc(options): void {
		//function para seleccionar el checkbox del TOC en el caso que esté creado y la capa se tenga que visualizar
		if (options.defaultVisibility == undefined) {
			return;
		}
		if (!options.defaultVisibility) {
			return;
		}
		if (document.getElementById("checklayer_" + options.id) == undefined) {
			return;
		}
		const checkbox = document.getElementById(
			"checklayer_" + options.id
		) as HTMLInputElement;
		if (checkbox.checked) {
			return;
		}
		checkbox.click();
	}
	/**
	 * This function is used for render the layer in the map.
	 * @param {any} event - The event of the layer.
	 * @returns {void} Return nothing
	 * @method
	 */
	public layerRenderListener(event): void {
		const geometryType = this.layer
			.getSource()
			.getFeatures()[0]
			.getGeometry()
			.getType();
		if (
			this.getStyleFunction().styleType !==
			this.getGeometryType(geometryType)
		) {
			// Error Handler
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.internalErrors
					.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER,
				errrosMsg.internalErrosMsg
					.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER +
					this.getAlias() +
					`geometry. Loaded Default style`
			);
			this.emit(GOLayerEvents.LAYER_ERROR, {
				error:
					errrosMsg.internalErrosMsg
						.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER +
					this.getAlias() +
					`geometry. Loaded Default style`,
			});
		}
		event.target.un("prerender", this.layerRenderListener);
	}

	/**
	 *  This function allow to clear all the intervalls.
	 * @param {any[]} intervals - The intervals.
	 * @returns {void} Return nothing
	 * @method
	 */ // eslint-disable-next-line
	private clearAllIntervals(intervals: any[]) {
		// eslint-disable-next-line
		intervals.forEach((interval: any) => {
			clearInterval(interval);
		});
	}

	/**
	 * This method is a placeholder and currently not implemented.
	 * It triggers an error event indicating that the method is not implemented.
	 * @returns {void}
	 */
	public get(): void {
		// Error Handler
		goErrorsHandlers.internalErrorHandler.set(
			constants.ErrorSeverity.Info,
			constants.internalErrors.INTERNAL_ERROR_NOT_IMPLEMENTED,
			errrosMsg.internalErrosMsg.INTERNAL_ERROR_NOT_IMPLEMENTED
		);
		this.emit(GOLayerEvents.LAYER_ERROR, {
			error: errrosMsg.internalErrosMsg.INTERNAL_ERROR_NOT_IMPLEMENTED,
		});
	}

	/**
	 * Retrieves the OpenLayers vector layer associated with this instance.
	 * @returns {VectorLayer<VectorSource>} The vector layer.
	 */
	public getLayer(): VectorLayer<VectorSource> {
		return this.layer;
	}

	/**
	 * Retrieves the name of the layer.
	 * @returns {string} The name of the layer.
	 */
	public getLayerName(): string {
		return this.layerOpts.layerName || "";
	}

	/**
	 * Retrieves the geometry name of the layer.
	 * @returns {Promise<string>} A promise that resolves with the geometry name of the layer.
	 */
	public async getGeometryName(): Promise<string> {
		const response = await this.layer?.get("geometryName");
		return response;
	}

	/**
	 * Retrieves the projection code of the layer.
	 * @returns {string} The projection code of the layer.
	 */
	public getLayerProjection(): string {
		return this.layerProjection || "";
	}

	/**
	 * Retrieves the URL of the layer's service.
	 * @returns {string} The URL of the layer's service.
	 */
	public getUrl(): string {
		return this.layerOpts.url!;
	}

	/**
	 * Checks if the layer is visible.
	 * @returns {boolean} True if the layer is visible, otherwise false.
	 */
	public getVisibility(): boolean {
		return this.layer.getVisible();
	}

	/**
	 * Retrieves the filter applied to the layer.
	 * @returns {string} The filter string or an empty string if no filter is applied.
	 */
	public getFilter(): string {
		if (!this.getLayer().get("filter")) {
			return "";
		}
		return this.formatFilter(
			this.getLayer().get("filter"),
			this.getLayer().get("bbox")
		);
	}

	/**
	 * Retrieves the legend graphic for the layer.
	 * @param {string} [layerOptsLegend] - Optional legend options to include in the request.
	 * @returns {Promise<string | null>} A promise that resolves with the legend graphic as a data URL string.
	 */
	public async getLegend(layerOptsLegend?: string): Promise<string | null> {

		const geoserverUrl = this.layerOpts.url
			? new URL(this.layerOpts.url).origin
			: `${GoStore.getState().geoserverAuthData?.url}`;
		const { enviroment } = GoStore.getState();
		const sldStyleToApply = this.layerOpts.sldStyle?.url
			.split("/")
			.pop()
			?.split(".")[0];
		const legendUrl = `${geoserverUrl}/geoserver/wms?REQUEST=GetLegendGraphic&FORMAT=image/png&WIDTH=20&HEIGHT=20&LAYER=${this.layerOpts.layerName}&STYLE=${sldStyleToApply}&LEGEND_OPTIONS=${layerOptsLegend}`;

		try {
			const axiosConfig = {
				url: `${enviroment}/gis-apirest/api/geoserver/sld`,
				method: "POST",
				timeout: 20000,
				headers: {
					"Accept": "application/json, text/plain, */*",
					"Content-Type": "application/json",
				},
				data: getObjectParamsFromUrl(`${legendUrl}`),
				responseType: "arraybuffer",
			};

			// eslint-disable-next-line @typescript-eslint/no-explicit-any
			const response = await GoAxiosServices.sendRequest(
				axiosConfig as any,
				false
			);

			let blob: Blob | null = null;

			if (response.data instanceof ArrayBuffer) {
				blob = new Blob([response.data], { type: 'image/png' });
			} else if (typeof response.data === 'string') {
				const bytes = new Uint8Array(response.data.length);
				for (let i = 0; i < response.data.length; i++) {
					bytes[i] = response.data.charCodeAt(i) & 0xff;
				}
				blob = new Blob([bytes], { type: 'image/png' });
			} else if (response.data instanceof Blob) {
				blob = response.data;
			}

			if (blob) {
				return await blobToDataURL(blob);
			}

			return null;
		} catch (err) {
			console.error("Error cargando leyenda:", err);
			return null;
		}
	}

	/**
	 * Converts the layer's geometry type to a GOGeometryType.
	 * @param {string} layerGeomType - The geometry type of the layer as a string.
	 * @returns {GOGeometryType} The corresponding GOGeometryType.
	 * @throws {Error} Throws an error if the geometry type is not recognized.
	 */
	public getGeometryType(layerGeomType): GOGeometryType {
		let GOGeomType: GOGeometryType = GOGeometryType.POINT;
		switch (layerGeomType) {
			case "Point": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "MultiPoint": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "LineString": {
				GOGeomType = GOGeometryType.LINE_STRING;
				break;
			}
			case "MultiLineString": {
				GOGeomType = GOGeometryType.LINE_STRING;
				break;
			}
			case "Polygon": {
				GOGeomType = GOGeometryType.POLYGON;
				break;
			}
			case "MultiPolygon": {
				GOGeomType = GOGeometryType.POLYGON;
				break;
			}
			default: {
				// Error Handler
				goErrorsHandlers.internalErrorHandler.set(
					constants.ErrorSeverity.Warning,
					constants.internalErrors.INTERNAL_ERROR_GEOMETRY_NOT_FOUND,
					errrosMsg.internalErrosMsg.INTERNAL_ERROR_GEOMETRY_NOT_FOUND
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					error: errrosMsg.internalErrosMsg
						.INTERNAL_ERROR_GEOMETRY_NOT_FOUND,
				});
			}
		}
		return GOGeomType;
	}

	/**
	 * Retrieves the geometry type of the features in the layer.
	 * @returns {string} The geometry type of the features.
	 */
	public getFeatureType(): string {
		const layer = this.getLayer();
		if (!layer) {
			return "";
		}
		const source = layer.getSource();
		if (!source) {
			return "";
		}
		const features = source.getFeatures();
		if (features.length === 0 || !features[0]?.getGeometry()) {
			return "";
		}
		return features[0].getGeometry()!.getType();
	}

	/**
	 * Retrieves the description of the layer's feature type.
	 * @returns {CancelablePromise<string>} A promise that resolves with the geometry field name or rejects with an error.
	 */
	public getLayerDescription(): CancelablePromise<string> {
		const geometryField = this.layerOpts.geometryField || "geom";
		this.layer.set("geometryName", geometryField);
		if (this.layerOpts.geometryType) {
			this.layer.set("geometryType", this.layerOpts.geometryType);
		}
		return new CancelablePromise((resolve) => resolve(geometryField));
	}

	/**
	 * Sets the SVG icons for the layer.
	 * @returns {CancelablePromise<styleSVG>} A promise that resolves with the SVG style or rejects with an error.
	 */
	public setIcons() {
		if (!this.layerOpts.styleSVG) {
			throw new ReferenceError(
				"Style SVG not found in the layer options."
			);
		}
		const styleSVG = GoIcons.getSvgString(this.layerOpts.styleSVG);
		this.styleSVG = styleSVG;
		return styleSVG;
	}

	/**
	 * Sets default annotations for the layer based on the layer options.
	 * @returns {void}
	 */
	public setDefaultAnnotations(): void {
		const annotations = this.getLayerStyle().getAnnotations();
		if (annotations) {
			this.layerOpts?.defaultAnnotations?.forEach((annotation) => {
				annotations?.addNewAnnotation(
					annotation.idAnnotation,
					annotation
				);
			});
			annotations.setAnnotations();
		}
	}
}

---
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\GoClusterLayer\index.ts
import * as types from "@goTypes/index";
import { GOStagingMap } from "@map/index";
import { GoAxiosServices, GoGeoServices, GoStore } from "@services/index";
import * as convex from "@turf/convex";
import * as GoError from "@utils/classes/GoError";
import { goErrorsHandlers } from "@utils/classes/GoError";
import * as constants from "@utils/classes/GoError/constants";
import { internalErrosMsg } from "@utils/classes/GoError/errrosMsg";
import { STYLES_ERROR_CANT_LOAD } from "@utils/classes/GoError/errrosMsg/stylesErrors.msg";
import { gisColor } from "@utils/classes/static";
import {
	GOFeaturesEvents,
	GOGeometryType,
	GOLayerEvents,
} from "@utils/constants";
import {
	getParamsFromUrl,
	getUrlWithoutParams,
	nextFrame,
} from "@utils/functions";
import { memorize } from "@utils/functions/mathFunctions";
import { evaluateCondition, GoIcons, hasBackendFeatures } from "@utils/index";
import { AxiosResponse } from "axios";
import { CancelablePromise } from "cancelable-promise";
import { Feature, Map as OlMap } from "ol";
import { FeatureLike } from "ol/Feature";
import { Extent } from "ol/extent";
import GeoJSON from "ol/format/GeoJSON";
import Geometry from "ol/geom/Geometry";
import Polygon from "ol/geom/Polygon";
import VectorLayer from "ol/layer/Vector";
import VectorImageLayer from "ol/layer/VectorImage";
import { bbox as bboxStrategy, all as allStrategy } from "ol/loadingstrategy";
import { transformExtent } from "ol/proj";
import { Cluster } from "ol/source";
import VectorSource from "ol/source/Vector";
import { Fill, Icon, Stroke, Style } from "ol/style";
import CircleStyle from "ol/style/Circle";
import "ol/style/Stroke";
import { GOStyledLayer } from "../GoStyledLayer";

/**
 * This class is the cluster layer.
 * @class GOClusterLayer
 * @augments  GOStyledLayer
 * @property {number} clusterCounter
 * @property {Map<string, types.clusterExtAnnotation>} extAnnotation
 * @property {string} layerProjection
 * @property {string} toProjection
 * @constructor
 * @param {import('types').layerOptions} layerOpts - The layer options.
 * @param {string} topicId - The topic id.
 * @param {import('types').OlMap} map - The map.
 */
export class GOClusterLayer extends GOStyledLayer {
	/**
	 * Function callback (define more specific type and description based on usage).
	 * @private
	 */
	private callbackFunction;
	/**
	 * VectorLayer for displaying the convex hull.
	 * @type {VectorLayer<VectorSource>}
	 * @private
	 */
	private convexHullLayer: VectorLayer<VectorSource>;
	/**
	 * The label color for clustered features.
	 * @type {string}
	 */
	private labelColor = "#000000";
	/**
	 * The OpenLayers map associated with this layer.
	 * @type {OlMap}
	 */
	private map: OlMap;
	/**
	 * The maximum resolution for clustering features.
	 * @type {number}
	 */
	private maxResolutionCluster: number;
	/**
	 * The radius style for clustering.
	 * @type {types.clusterStyleCode|undefined}
	 */
	private radiousStyle: types.clusterStyleCode | undefined;
	/**
	 * A map that stores label styles for different cluster sizes.
	 * @type {Map<number, string>}
	 */
	private styleLabel: Map<number, string> = new Map();
	/**
	 * A map that stores custom styles for different cluster sizes.
	 * @type {Map<number, types.clusterStyle>}
	 */
	private styleMap: Map<number, types.clusterStyle> = new Map();
	/**
	 * The SVG representation of the cluster layer.
	 * @type {string}
	 */
	private svg = "";
	/**
	 * The SVG representation of clustered features.
	 * @type {string}
	 */
	private svgCluster = "";
	/**
	 * The SVG representation of individual clustered features.
	 * @type {string}
	 */
	private svgClusterInd = "";
	/**
	 * The zIndex for the cluster layer.
	 * @type {number}
	 */
	private zIndex: number;
	/**
	 * The counter for the number of clusters.
	 * @type {number}
	 */
	public clusterCounter: number;
	/**
	 * A map that stores external annotations for clusters.
	 * @type {Map<string, types.clusterExtAnnotation>}
	 */
	public extAnnotation: Map<string, types.clusterExtAnnotation> = new Map();

	protected mapId: string | undefined;
	public layerProjection: string | undefined;
	public toProjection: string | undefined;
	public useBboxFilter: boolean;

	/**
	 * Creates a new instance of the GOClusterLayer class.
	 * @param {OlMap} olMap - The OpenLayers map where the layer will be displayed.
	 * @param {types.layerOptions} layerOpts - Options for configuring the layer.
	 * @param {string} topicId - The ID of the topic associated with the layer.
	 */
	constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {
		super(stagingMap, layerOpts);
		this.map = this.olMap;
		this.mapId = stagingMap.getId();
		this.layerProjection = layerOpts.layerProjection
			? layerOpts.layerProjection.split(":").pop()?.toString()
			: "4326";
		this.toProjection = layerOpts.toProjection
			? layerOpts.toProjection.split(":").pop()?.toString()
			: undefined;
		this.useBboxFilter =
			layerOpts.useBboxFilter === undefined && layerOpts.layerName
				? true
				: false;

		this._initLayer();
	}

	// Pirvate methods ----------------------------------------------------------

	private _initLayer(): void {
		const { layerOpts, map } = this;
		this.emit(GOLayerEvents.LAYER_LOADING, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});
		this.setIcons();

		if (layerOpts.styleSVGConditional)
			this.setStyleValues(layerOpts.styleSVGConditional.styleInfoSVG);

		this.olMap = map;
		let { labelColor } = this;
		this.layerOpts = layerOpts;
		this.clusterCounter = 0;
		this.zIndex = layerOpts.zIndex || 1;
		this.maxResolutionCluster = layerOpts.maxResolutionCluster || 0;
		const visibility: boolean =
			layerOpts.defaultVisibility === null ||
			layerOpts.defaultVisibility === undefined
				? true
				: layerOpts.defaultVisibility;

		if (!layerOpts.distance) {
			// Error Handler
			GoError.goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${
						layerOpts.layerName
							? `${layerOpts.layerName}:`
							: layerOpts.id
					}
					 parameter distance not found`
			);

			this.emit(GOLayerEvents.LAYER_ERROR, {
				id: this.getOptions().id,
				error:
					internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${
						layerOpts.layerName
							? `${layerOpts.layerName}:`
							: layerOpts.id
					}
					 parameter distance not found`,
			});
		}

		if (layerOpts.styleCluster) {
			layerOpts.styleCluster.forEach((element) => {
				labelColor = element.labelColor
					? element.labelColor
					: this.labelColor;
				this.styleLabel.set(element.size, labelColor);
				this.styleMap.set(element.size, element.style);
			});
		} else {
			const style =
				'[{"size":1,"style":{"radius":8,"stroke":"#040404","fill":"rgba(254, 186, 186, 0.75)"}},{"size":3,"style":{"radius":11,"stroke":"#040404","fill":"rgba(233, 153, 153, 0.75)"}},{"size":5,"style":{"radius":15,"stroke":"#040404","fill":"rgba(231, 136, 136, 0.75)"}},{"size":8,"style":{"radius":23,"stroke":"#040404","fill":"rgba(225, 114, 114, 0.75)"}},{"size":10,"style":{"radius":28,"stroke":"#040404","fill":"rgba(227, 99, 99, 0.75)"}},{"size":15,"style":{"radius":35,"stroke":"#04040","fill":"rgba(222, 79, 79, 0.75)"}},{"size":20,"style":{"radius":45,"stroke":"#04040","fill":"rgba(219, 56, 56, 0.75)"}},{"size":40,"style":{"radius":60,"stroke":"#04040","fill":"rgba(203, 29, 29, 0.75)"}}]';
			const styleParse = JSON.parse(style);
			styleParse.forEach((element) => {
				this.styleMap.set(element.size, element.style);
			});
		}

		let vectorSource: Array<Feature>;
		let cluster: Cluster;

		let dist;
		if (this.olMap.getView().getResolution() < this.maxResolutionCluster) {
			dist = 0.001;
		} else {
			dist = layerOpts.distance;
		}

		if (!layerOpts.layerName && !layerOpts.url) {
			//AddFeatures later via method
			cluster = new Cluster({
				distance: dist,
				source: new VectorSource({
					format: new GeoJSON(),
					features: vectorSource,
				}),
			});
		} else if (layerOpts.url && !layerOpts.layerName) {
			//Add features from geojson url
			const vectorSourceInterno = new VectorSource();
			cluster = new Cluster({
				distance: dist,
				source: vectorSourceInterno,
			});

			this.getFeaturesfromUrl(layerOpts.url)
				.then((features) => {
					if (
						!features ||
						!features.type ||
						(features.type === "FeatureCollection" &&
							!Array.isArray(features.features))
					) {
						GoError.goErrorsHandlers.internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors
								.INTERNAL_ERROR_CAN_LOAD_LAYER,
							`[${layerOpts.id}]: The provided resource is not a valid GeoJSON.`
						);
					}

					let tempFeatures: any;
					if (this.layerProjection && this.toProjection) {
						tempFeatures = new GeoJSON()
							.readFeatures(features, {
								dataProjection: "EPSG:" + this.layerProjection,
								featureProjection: "EPSG:" + this.toProjection,
							})
							.filter((feat) => feat.getGeometry());
					} else {
						tempFeatures = new GeoJSON()
							.readFeatures(features)
							.filter((feat) => feat.getGeometry());
					}
					cluster!.getSource()!.clear();
					cluster!.getSource()!.addFeatures(tempFeatures);
				})
				.catch(() => {
					GoError.goErrorsHandlers.internalErrorHandler.set(
						constants.ErrorSeverity.Error,
						constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
						`[${layerOpts.id}]: The fetch request failed. Features data could not be loaded.`
					);
				});
		} else if (layerOpts.layerName) {
			//Add features from geoserver
			cluster = new Cluster({
				distance: dist,
				source: new VectorSource({
					format: new GeoJSON(),
					loader: this.loader.bind(this),
					strategy: this.useBboxFilter ? bboxStrategy : allStrategy,
				}),
			});
		}

		if (layerOpts.disableImageEffect) {
			this.layer = new VectorLayer({
				source: cluster,
				visible: visibility,
				style: this.clusterStyleFunction.bind(this),
			});
		} else {
			this.layer = new VectorImageLayer({
				source: cluster,
				visible: visibility,
				style: this.clusterStyleFunction.bind(this),
			});
		}

		const goClusterStyleData: {
			indiviualStyle: types.ISVGIcon | null;
			clusteredStyle: types.ISVGIcon | null;
			stackedStyle: types.ISVGIcon | null;
		} = {
			indiviualStyle: null,
			clusteredStyle: layerOpts.styleSVGCluster || null,
			stackedStyle: layerOpts.styleSVGClusterInd || null,
		};

		// Determine the style type for the invidual features in the cluster
		const fillClusterStyleData = () => {
			// Store in individualStyle the style for the individual features in the cluster
			const styleInfo = layerOpts.styleGeojson?.styleInfo[0];

			// svg
			if (layerOpts.styleSVG) {
				goClusterStyleData.indiviualStyle = layerOpts.styleSVG;
				return "svg";
			}

			// svgConditional
			if (layerOpts.styleSVGConditional) {
				goClusterStyleData.indiviualStyle =
					layerOpts.styleSVGConditional;
				return "svgConditional";
			}

			// json
			if (styleInfo && !styleInfo?.condition) {
				goClusterStyleData.indiviualStyle =
					layerOpts.styleGeojson as types.ISVGIcon;
				return "json";
			}

			// jsonConditional
			if (styleInfo?.condition) {
				goClusterStyleData.indiviualStyle =
					layerOpts.styleGeojson as types.ISVGIcon;
				return "jsonConditional";
			}

			// default
			console.warn(
				"No style has been defined for the individual features in the cluster"
			);
		};

		const styleType = fillClusterStyleData();

		// Save style in the stylelog for future use
		// FIXME: This is a temporal solution, when the migration is finished this will be removed
		this.layerStyle.addRetrocompatibilityInitalStyle(
			styleType,
			goClusterStyleData,
			this.clusterStyleFunction.bind(this)
		);
		// end save style
		this.layer.setZIndex(this.zIndex);
		this.layer.set("id", layerOpts.id);
		this.layer.set("bbox", this.useBboxFilter);
		this.layer.set("filter", null);

		this.layer.set("type", this.type());

		if (layerOpts.filter && layerOpts.layerName) {
			this.addFilter(layerOpts.filter, true);
			this.layer.set("bbox", true);
		} else if (layerOpts.filter && !layerOpts.layerName) {
			GoError.goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				`You can't use a filter with GeoJson data. The layerName parameter is required — use addFilterNoCQL method instead.`
			);
		}

		if (layerOpts.layerName) {
			this.layer.set("geometryName", this.layerOpts.geometryField || "geom");
		}

		if (layerOpts.minResolution) {
			this.setMinResolution(layerOpts.minResolution);
		}

		if (layerOpts.maxResolution) {
			this.setMaxResolution(layerOpts.maxResolution);
		}

		if (layerOpts.sldStyle) {
			this.getStyleFromSLD(layerOpts.sldStyle).then((style1) => {
				try {
					this.styleFunction = style1;
					this.layer.on("prerender", this.layerRenderListener);
					this.layer.setStyle(
						memorize((...rest) => style1.styleFunction(...rest))
					);

					this.emit(GOLayerEvents.LAYER_STYLE_CHANGED, {
						mapId: this.mapId,
						layerId: layerOpts.id,
						layer: this,
						style: this.layer.getStyle(),
					});
				} catch (error) {
					if (error instanceof Error)
						// Error Handler
						GoError.goErrorsHandlers.stylesErrorHandler.set(
							constants.ErrorSeverity.Warning,
							constants.stylesErrors.STYLES_ERROR_CANT_LOAD,
							STYLES_ERROR_CANT_LOAD,
							error
						);
				}
				this.layerPromise = Promise.resolve(this.layer);
				return this;
			});
		}

		if (this.layerOpts.envelope) {
			this.convexHullLayer = new VectorImageLayer({
				source: new VectorSource(),
				zIndex: 99,
				style: new Style({
					stroke: new Stroke({
						color: gisColor.ENVELOPE_COLOR_STROKE,
						width: 2,
						lineDash: [10, 10],
					}),
					fill: new Fill({
						color: gisColor.ENVELOPE_COLOR_FILL,
					}),
				}),
			});
			this.olMap.addLayer(this.convexHullLayer);
			this.envelope();
		}
		this.layerPromise = Promise.resolve(this.layer);
	}

	/**
	 * This function allow to set cluster feature
	 * @param {import('types').FeatureLike} feature - The feature to set
	 * @returns {string} Return the feature.
	 * @method
	 */
	private featureCluster(feature: FeatureLike): string {
		const a = this.callbackFunction(feature);
		return a;
	}

	/**
	 * Set the cluster callback function.
	 * @param {Function} callback - The callback function.
	 */
	public clusterCallback(callback): void {
		this.callbackFunction = callback;
	}

	/**
	 * Refresh the layer's source.
	 */
	public refresh(): void {
		this.layer.getSource().refresh({ force: true });
	}

	/**
	 * Calculate and display the envelope (convex hull) of the cluster layer.
	 */
	public envelope(): void {
		// eslint-disable-next-line
		const ref = this;
		this.olMap.on("pointermove", async (evt) => {
			await nextFrame();
			this.convexHullLayer.getSource().clear();
			const hit = this.olMap.hasFeatureAtPixel(evt.pixel, {
				layerFilter: function (layer) {
					return layer.get("id") === ref.layerOpts.id;
				},
			});
			if (hit) {
				const features = this.olMap.getFeaturesAtPixel(evt.pixel, {
					layerFilter: function (layer) {
						return layer.get("id") === ref.layerOpts.id;
					},
				});
				const coordinates = [];
				if (features[0].getProperties().features.length > 2) {
					features[0].getProperties().features.forEach((element) => {
						coordinates.push([
							element.getGeometry().getCoordinates()[0],
							element.getGeometry().getCoordinates()[1],
						]);
					});
					const pt = {
						type: "Feature",
						properties: {},
						geometry: {
							type: "MultiPolygon",
							coordinates: [[coordinates]],
						},
					};

					// eslint-disable-next-line
					const convexHull = convex.default(pt as any);
					if (convexHull) {
						const polygon = new Polygon(
							convexHull.geometry.coordinates
						);
						const featureHull = new Feature(polygon);
						this.convexHullLayer
							.getSource()
							.addFeature(featureHull);
					}
				}
			}
		});
	}

	/**
	 * Check if the layer has a filter.
	 * @returns {boolean} True if the layer has a filter, false otherwise.
	 */
	public haveFilter(): boolean {
		return this.getLayer().get("filter") !== null;
	}

	/**
	 * Check if the layer can be filtered.
	 * @returns {boolean} True if the layer can be filtered, false otherwise.
	 */
	public canFilter(): boolean {
		return true;
	}

	/**
	 * Get the type of the layer.
	 * @returns {string} The type of the layer ("GOClusterLayer").
	 */
	public type(): string {
		return "GOCLusterLayer";
	}

	/**
	 * Get features from a specified URL.
	 * @param {string} url - The URL from which to fetch features.
	 * @returns {CancelablePromise<Feature[]|Feature>} A CancelablePromise that resolves to an array of features or a single feature.
	 * @method
	 */
	public getFeaturesfromUrl(
		url: string
	): CancelablePromise<Feature[] | Feature> {
		return new CancelablePromise<Feature[] | Feature>((resolve) => {
			GoAxiosServices.sendRequest({
				url,
				method: "GET",
				timeout: 20000,
			}).then((response: AxiosResponse) => {
				resolve(response.data);
			});
		});
	}

	/**
	 * This function allow to get the geometry types.
	 * @param {string} type - The type to check.
	 * @returns {boolean} Return true if the type is a geometry type.
	 * @method
	 */
	private isGeometryType(type: string): boolean {
		return (
			type.toUpperCase() === "GEOMETRY" ||
			type.toUpperCase() === "MULTIPOINT" ||
			type.toUpperCase() === "POINT"
		);
	}

	/**
	 * Transform an extent from one projection to another.
	 * @param {Object} options - The transformation options.
	 * @param {Extent} options.extent - The extent to transform.
	 * @param {string} options.srcProjection - The source projection.
	 * @param {string} options.toProjection - The target projection.
	 * @returns {Extent} The transformed extent.
	 */
	transformExtend = memorize(
		({
			extent,
			srcProjection,
			toProjection,
		}: {
			extent: Extent;
			srcProjection: string;
			toProjection: string;
		}): Extent => transformExtent(extent, srcProjection, toProjection)
	);

	/**
	 * Asynchronous data loader for the layer.
	 * @param {Extent} extent - The extent to load data for.
	 * @returns {Promise<any>} A promise that resolves with the loaded data.
	 */
	// eslint-disable-next-line
	public async loader(extent: Extent): Promise<any> {
		if (this.consumeCancellationFlag()) {
			return;
		}

		if (!this.layer) {
			console.warn(
				`[${this.layerOpts.id}] Cluster loader triggered before layer initialization. Skipping current load cycle.`
			);
			return;
		}

		const layerSource = this.layer.getSource();
		if (!layerSource) {
			console.warn(
				`[${this.layerOpts.id}] Cluster loader cannot proceed because the source is not ready yet.`
			);
			return;
		}

		await nextFrame();
		let url: string;
		let url_Own: string;

		const mapProjection = this.map
			.getView()
			.getProjection()
			.getCode()
			.split(":")
			.pop()
			?.toString();

		if (mapProjection !== this.layerProjection) {
			extent = this.transformExtend({
				extent,
				srcProjection: `EPSG:${mapProjection}`,
				toProjection: `EPSG:${this.layerProjection}`,
			});
		}

		const geomName: string = await this.layer.get("geometryName");
		const service = `/geoserver/wfs?service=WFS&version=1.0.0&request=GetFeature&typename=${this.layerOpts.layerName}`;

		const outputFormat = `outputFormat=application/json&srsname=EPSG:${mapProjection}`;
		const bBox = `cql_filter=BBOX(${geomName},${extent.join(",")})&`;
		const filterNoBbox = "cql_filter=";
		const filter: string = this.layer.get("filter");

		let propertyName = "";
		if (this.layerOpts.propertyName) {
			propertyName = `&propertyName=${geomName},${this.layerOpts.propertyName.toString()}`;
		}

		// Check URL origin of data
		let geoserverUrl = GoStore.getState().geoserverAuthData?.url;
		if (this.layerOpts.url && this.layerOpts.layerName) {
			geoserverUrl = this.layerOpts.url;
		}

		if (this.layer.get("filter")) {
			if (this.layer.get("bbox")) {
	
				const bBoFilter = bBox.replace("&", "");
				url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&${bBoFilter}${encodeURIComponent(
					filter
				)}`;
				url = url + "&";
				url_Own = url.toString();
			} else {
				url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&${filterNoBbox}${encodeURIComponent(
					filter
				)}`;
				url = url + "&";
				url_Own = url.toString();
			}
		} else if (this.useBboxFilter) {
			url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&${bBox}`;
			url_Own = url.toString();
		} else {
			url = `${geoserverUrl}${service}&${outputFormat}${propertyName}&`;
			url_Own = url.toString();
		}

		// Add geoparams if provided
		if (this.layerOpts.geoparams) {
			url += `${this.layerOpts.geoparams}&`;
			url_Own = url;
		}

		try {
			const responsePromise = this.trackRequest(
				memorize((url_Own) =>
					GoGeoServices.getGISApiRestFeatureCollection({
						url: getUrlWithoutParams(url_Own),
						data: getParamsFromUrl(url_Own),
						customGeoserverURL: this.layerOpts.url ? url_Own : null,
						serviceType: "wfs",
					})
				)(url_Own)
			);

			const response = (await responsePromise) as AxiosResponse;

			if (
				typeof response.data === "string" &&
				response.data.includes("?xml") &&
				response.data.includes("ServiceExceptionReport")
			) {
				if (layerSource.get("firstTime") == undefined) {
					// Error Handler
					GoError.goErrorsHandlers.internalErrorHandler.set(
						constants.ErrorSeverity.Warning,
						constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
						internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER
					);
					this.emit(GOLayerEvents.LAYER_ERROR, {
						id: this.getOptions().id,
						error: internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER,
					});
					layerSource.set("firstTime", true);
				}
			} else {
				const { layerOpts, map } = this;

				this.emit(GOFeaturesEvents.FEATURES_LOAD_STARTED, {
					mapId: this.mapId,
					layerId: layerOpts.id,
				});

				if (response.data) {
					layerSource.getSource().clear();
					layerSource
						.getSource()
						.addFeatures(
							layerSource
								.getSource()
								.getFormat()
								.readFeatures(response.data)
								.filter((feat) =>
									feat.getGeometry()
								) as Feature<Geometry>[]
						);
					layerSource.set("firstTime", false);
				}

				this.emit(GOFeaturesEvents.FEATURES_LOAD_ENDED, {
					mapId: this.mapId,
					layerId: layerOpts.id,
				});

				return response;
			}
		} catch (e) {
			if (!layerSource.get("firstTime")) {
				// Error handler
				GoError.goErrorsHandlers.internalErrorHandler.set(
					constants.ErrorSeverity.Error,
					constants.internalErrors.INTERNAL_ERROR_FIELD,
					internalErrosMsg.INTERNAL_ERROR_FIELD +
						`${this.layerOpts.id}: ${e.message}`
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					id: this.getOptions().id,
					error:
						internalErrosMsg.INTERNAL_ERROR_FIELD +
						`${this.layerOpts.id}: ${e.message}`,
				});
				layerSource.set("firstTime", true);
			}
		}
	}

	/**
	 * Check if all features in a cluster have the same geometry.
	 * @param {Array<Feature>} features - The features in the cluster.
	 * @returns {boolean} True if all features have the same geometry, false otherwise.
	 */
	private checkSameGeom(features): boolean {
		let sameGeom = true;
		const latFirstGeom = features[0].getGeometry().getCoordinates()[0];
		const lonFirstGeom = features[0].getGeometry().getCoordinates()[1];
		features.forEach((feature) => {
			if (
				feature.getGeometry().getCoordinates()[0] !== latFirstGeom ||
				feature.getGeometry().getCoordinates()[1] !== lonFirstGeom
			) {
				sameGeom = false;
				return sameGeom;
			}
		});
		return sameGeom;
	}

	/**
	 * Add features to the layer.
	 * @param {Object} data - The data to add to the layer.
	 * @param {boolean} clear - Whether to clear existing features before adding new ones.
	 */
	public addFeatures(data, clear?: boolean): void {
		const { layerOpts, map } = this;

		this.emit(GOFeaturesEvents.FEATURES_LOAD_STARTED, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});

		if (clear) {
			this.layer?.getSource().getSource().clear();
		}

		if (this.layerProjection) {
			this.layer
				?.getSource()
				.getSource()
				.addFeatures(
					new GeoJSON({
						dataProjection: "EPSG:" + this.layerProjection,
						featureProjection: "EPSG:" + this.toProjection,
					})
						.readFeatures(data.default)
						.filter((feat) => feat.getGeometry())
				);
		} else {
			this.layer
				.getSource()
				.getSource()
				.addFeatures(
					new GeoJSON()
						.readFeatures(data.default)
						.filter((feat) => feat.getGeometry())
				);
		}

		this.emit(GOFeaturesEvents.FEATURES_LOAD_ENDED, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});
	}

	/**
	 * Listener for the layer's prerender event to handle style changes.
	 * @param {Object} event - The prerender event object.
	 */
	public layerRenderListener(event) {
		const geometryType = this.layer
			.getSource()
			.getFeatures()[0]
			.getGeometry()
			.getType();
		if (
			this.getStyleFunction().styleType !==
			this.getGeometryType(geometryType)
		) {
			// Error Handler
			GoError.goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.internalErrors
					.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER,
				internalErrosMsg.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER +
					this.getAlias() +
					`geometry. Loaded Default style`
			);
			this.emit(GOLayerEvents.LAYER_ERROR, {
				id: this.getOptions().id,
				error:
					internalErrosMsg.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER +
					this.getAlias() +
					`geometry. Loaded Default style`,
			});
		}
		event.target.un("prerender", this.layerRenderListener);
	}

	/**
	 * This function allow to set the radius of the layer
	 * @param {number} size - The radius size to set
	 * @returns {void} Return nothing
	 * @method
	 */
	private radiousStyler(size: number): void {
		const keysSize = Array.from(this.styleMap.keys());
		const labelsColor = Array.from(this.styleLabel.keys());
		let positionTop = 0;
		let positionBot = 0;
		let position = 0;
		let styleOwn;
		let labelColored;
		keysSize.forEach((element: number) => {
			if (element > size) {
				positionTop = element;
			} else if (element < size) {
				positionBot = element;
			} else {
				position = keysSize.indexOf(size);
			}
		});

		if (positionBot === 0 && positionTop === 0 && position === 0) {
			styleOwn = this.styleMap.get(keysSize.pop());
			labelColored = this.styleLabel.get(labelsColor.pop());
		} else if (positionBot === 0 && positionTop !== 0 && position === 0) {
			styleOwn = this.styleMap.get(keysSize[0]);
			labelColored = this.styleLabel.get(labelsColor[0]);
		} else if (positionBot !== 0 && positionTop !== 0 && position === 0) {
			styleOwn = this.styleMap.get(positionBot);
			labelColored = this.styleLabel.get(positionBot);
		} else if (position !== 0) {
			styleOwn = this.styleMap.get(keysSize[position]);
			labelColored = this.styleLabel.get(labelsColor[position]);
		} else {
			styleOwn = this.styleMap.get(keysSize.pop());
			labelColored = this.styleLabel.get(labelsColor.pop());
		}

		const radius = styleOwn.radius;
		const stroke: Stroke = new Stroke({
			color: styleOwn.stroke.toString(),
		});
		const fill: Fill = new Fill({
			color: styleOwn.fill.toString(),
		});

		const labelFieldColor = styleOwn?.labelFieldColor;
		const labelColor = labelColored;
		this.radiousStyle = {
			radius,
			stroke,
			fill,
			labelFieldColor,
			labelColor,
		};
	}

	/**
	 * Define the cluster style for features in the layer.
	 * @param {Feature<Geometry>} feature - The cluster feature.
	 * @returns {Array<Style>} An array of styles for the cluster feature.
	 */
	public clusterStyleFunction(feature: Feature<Geometry>): Array<Style> {
		const featuresCluster = feature.get("features");
		const size = featuresCluster.length;
		let value = 0;
		let radius = size;
		const layerOpts = this.getOptions();

		if (this.maxResolutionCluster) {
			this.olMap.getView().getResolution() < this.maxResolutionCluster
				? this.layer.getSource().setDistance(0)
				: this.layer.getSource().setDistance(layerOpts.distance);
		}

		const sameGeometry =
			size == 1 ? true : this.checkSameGeom(featuresCluster);

		if (layerOpts.clusterField) {
			//default annotation will be SUM of attribute
			featuresCluster.forEach((f) => {
				value += f.getProperties()[layerOpts.clusterField];
			});
			radius = value;
		}

		//Create visibility conditional annotation
		let visibilityAnnotation = true;
		if (layerOpts.visibilityAnnotationDef == false) {
			visibilityAnnotation = false;
		}
		//Create fontSize conditional annotation
		let fontSizeAnnotation = "10px";
		if (layerOpts.fontSizeAnnotationDef) {
			fontSizeAnnotation = layerOpts.fontSizeAnnotationDef;
		}
		//Create offsetX conditional annotation
		let offsetXAnnotation = 0;
		if (layerOpts.offsetXAnnotationDef) {
			offsetXAnnotation = layerOpts.offsetXAnnotationDef;
		}
		//Create offsetY conditional annotation
		let offsetYAnnotation = layerOpts.defaultClusterFunction ? 0 : 8;
		if (layerOpts.offsetYAnnotationDef) {
			offsetYAnnotation = layerOpts.offsetYAnnotationDef;
		}

		//Create conditional fontColor
		let fontColorAnnotation =
			sameGeometry && !layerOpts.defaultClusterFunction
				? "#444444"
				: "#FFFFFF";
		if (layerOpts.fontColorAnnotationDef) {
			fontColorAnnotation = layerOpts.fontColorAnnotationDef;
		}

		//Create annotation default
		const annotationDefault = {
			visibility: visibilityAnnotation,
			type: layerOpts.clusterField ? "SUM" : "SIZE",
			attribute: null,
			condition: null,
			value: layerOpts.clusterField
				? value.toString()
				: size > 99
				? "+99"
				: size.toString(),
			showOnZero: false,
			text: {
				offsetX: offsetXAnnotation,
				offsetY: offsetYAnnotation,
				fontColor: fontColorAnnotation,
				fontSize: fontSizeAnnotation,
			},
			showTextOnUnique: layerOpts.showTextOnUnique
				? layerOpts.showTextOnUnique
				: false,
		};
		const annotations = this.getLayerStyle().getAnnotations();
		annotations.addNewAnnotation("default", annotationDefault);

		let style;

		if (layerOpts.styleSVG || layerOpts.styleSVGCluster) {
			//Definition of scale and anchor
			const anchor = layerOpts.styleSVGCluster?.anchor
				? layerOpts.styleSVGCluster?.anchor
				: layerOpts.styleSVG?.anchor
				? layerOpts.styleSVG?.anchor
				: [0.5, 0.5];

			const scale = layerOpts.styleSVGCluster?.scale
				? layerOpts.styleSVGCluster?.scale
				: layerOpts.styleSVG?.scale
				? layerOpts.styleSVG?.scale
				: 1;

			let srcPrint;
			if (size == 1 && this.svg) {
				//Unique SVG
				srcPrint = this.svg;
			} else if (size == 1 && (this.svg == "" || !this.svg)) {
				//SVG Conditional
				const field = layerOpts.styleSVGConditional.field;
				const value = feature.get("features")[0].getProperties()[field];
				const styleValues = this.styleValues;
				let icon;
				styleValues.forEach((styleUserMap, key) => {
					const keyNew = key.replaceAll("##valor##", value);
					if (evaluateCondition(keyNew)) {
						icon = styleUserMap.image_.iconImage_.src_;
					}
				});

				srcPrint = icon;
			} else if (size > 1 && sameGeometry) {
				srcPrint = this.svgClusterInd;
			} else {
				srcPrint = this.svgCluster;
			}

			//default
			if (!srcPrint) {
				srcPrint =
					"data:image/svg+xml;charset=utf-8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20fill%3D%22none%22%20height%3D%2248px%22%20viewBox%3D%220%200%2048%2053%22%20width%3D%2245px%22%3E%0A%20%20%20%20%0A%20%20%20%20%3Cdefs%3E%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%3Cfilter%20color-interpolation-filters%3D%22sRGB%22%20filterUnits%3D%22userSpaceOnUse%22%20height%3D%2253%22%20id%3D%22filter0_ddd_2198_70283%22%20width%3D%2248%22%20x%3D%220%22%20y%3D%220%22%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeFlood%20flood-opacity%3D%220%22%20result%3D%22BackgroundImageFix%22%3E%3C%2FfeFlood%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeColorMatrix%20in%3D%22SourceAlpha%22%20result%3D%22hardAlpha%22%20type%3D%22matrix%22%20values%3D%220%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%20127%200%22%3E%3C%2FfeColorMatrix%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeOffset%20dy%3D%221%22%3E%3C%2FfeOffset%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeGaussianBlur%20stdDeviation%3D%221.5%22%3E%3C%2FfeGaussianBlur%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeComposite%20in2%3D%22hardAlpha%22%20operator%3D%22out%22%3E%3C%2FfeComposite%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeColorMatrix%20type%3D%22matrix%22%20values%3D%220%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200.16%200%22%3E%3C%2FfeColorMatrix%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeBlend%20in2%3D%22BackgroundImageFix%22%20mode%3D%22normal%22%20result%3D%22effect1_dropShadow_2198_70283%22%3E%3C%2FfeBlend%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeColorMatrix%20in%3D%22SourceAlpha%22%20result%3D%22hardAlpha%22%20type%3D%22matrix%22%20values%3D%220%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%20127%200%22%3E%3C%2FfeColorMatrix%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeOffset%20dy%3D%222%22%3E%3C%2FfeOffset%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeGaussianBlur%20stdDeviation%3D%221%22%3E%3C%2FfeGaussianBlur%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeComposite%20in2%3D%22hardAlpha%22%20operator%3D%22out%22%3E%3C%2FfeComposite%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeColorMatrix%20type%3D%22matrix%22%20values%3D%220%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200.16%200%22%3E%3C%2FfeColorMatrix%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeBlend%20in2%3D%22effect1_dropShadow_2198_70283%22%20mode%3D%22normal%22%20result%3D%22effect2_dropShadow_2198_70283%22%3E%3C%2FfeBlend%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeColorMatrix%20in%3D%22SourceAlpha%22%20result%3D%22hardAlpha%22%20type%3D%22matrix%22%20values%3D%220%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%20127%200%22%3E%3C%2FfeColorMatrix%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeOffset%3E%3C%2FfeOffset%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeGaussianBlur%20stdDeviation%3D%221%22%3E%3C%2FfeGaussianBlur%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeComposite%20in2%3D%22hardAlpha%22%20operator%3D%22out%22%3E%3C%2FfeComposite%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeColorMatrix%20type%3D%22matrix%22%20values%3D%220%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200%200.16%200%22%3E%3C%2FfeColorMatrix%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeBlend%20in2%3D%22effect2_dropShadow_2198_70283%22%20mode%3D%22normal%22%20result%3D%22effect3_dropShadow_2198_70283%22%3E%3C%2FfeBlend%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CfeBlend%20in%3D%22SourceGraphic%22%20in2%3D%22effect3_dropShadow_2198_70283%22%20mode%3D%22normal%22%20result%3D%22shape%22%3E%3C%2FfeBlend%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%3C%2Ffilter%3E%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%3C%2Fdefs%3E%0A%20%20%20%20%0A%20%20%20%20%3Cg%20filter%3D%22url(%23filter0_ddd_2198_70283)%22%3E%0A%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%3Ccircle%20cx%3D%2224%22%20cy%3D%2223%22%20fill%3D%22%23E37272%22%20id%3D%22border%22%20r%3D%2216%22%3E%3C%2Fcircle%3E%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%3C%2Fg%3E%0A%20%20%20%20%0A%20%20%20%20%3Crect%20fill%3D%22%23ffffff%22%20height%3D%2226%22%20rx%3D%2213%22%20width%3D%2226%22%20x%3D%2211%22%20y%3D%2210%22%3E%3C%2Frect%3E%0A%20%20%20%20%0A%20%20%20%20%3Cg%20id%3D%22icono%22%20transform%3D%22translate(16.250%2C%2016.250)%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20scale(1.300%2C%201.300)%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22%3E%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%3Cpath%20d%3D%22M10.7812%201.28125L7.03125%205.03125L10.75%208.75C11.0625%209.03125%2011.0625%209.5%2010.75%209.78125C10.4688%2010.0938%2010%2010.0938%209.71875%209.78125L5.96875%206.0625L2.25%209.78125C1.96875%2010.0938%201.5%2010.0938%201.21875%209.78125C0.90625%209.5%200.90625%209.03125%201.21875%208.71875L4.9375%205L1.21875%201.28125C0.90625%201%200.90625%200.53125%201.21875%200.21875C1.5%20-0.0625%201.96875%20-0.0625%202.28125%200.21875L6%203.96875L9.71875%200.25C10%20-0.0625%2010.4688%20-0.0625%2010.7812%200.25C11.0625%200.53125%2011.0625%201%2010.7812%201.28125Z%22%20fill%3D%22%23000000%22%3E%3C%2Fpath%3E%0A%20%20%20%20%20%20%20%20%09%0A%20%20%20%20%3C%2Fg%3E%0A%3C%2Fsvg%3E";
			}

			style = new Style({
				image: new Icon({
					src: srcPrint.includes("data:image/svg+xml;charset=utf-8,")
						? srcPrint
						: "data:image/svg+xml;charset=utf-8," +
						  encodeURIComponent(srcPrint),
					opacity: 1,
					anchor: anchor,
					scale: scale,
				}),
			});
		} else if (layerOpts.defaultClusterFunction) {
			this.radiousStyler(radius);
			style = new Style({
				image: new CircleStyle({
					radius: this.radiousStyle.radius,
					stroke: this.radiousStyle.stroke,
					fill: this.radiousStyle.fill,
				}),
			});
		}

		//Add annotations to array of styles
		let styleReturn: Array<Style>;
		styleReturn =
			layerOpts.clusterField || size >= 1
				? memorize((feature, style) =>
						annotations.__addAnnotations(feature, style)
				  ).bind(this)(feature, style)
				: [style];

		if (annotations.getExtAnnotations().size > 0) {
			styleReturn = annotations.__addExtAnnotations(feature, style);
		}

		return styleReturn;
	}

	// Function to get counter values
	public getCounterValues(): number {
		return this.clusterCounter;
	}

	// Function to get the layer
	public getLayer(): VectorLayer<VectorSource> {
		return this.layer;
	}

	// Function to get the visibility status of the layer
	public getVisibility(): boolean {
		return this.layer.getVisible();
	}
	/**
	 * This method allow to get the geometry type.
	 * @param {import('types').GeometryType} layerGeomType - Description of the condition
	 * @returns {GOGeometryType} Return the geometry type.
	 * @method
	 */
	private getGeometryType(layerGeomType): GOGeometryType {
		let GOGeomType: GOGeometryType;
		switch (layerGeomType) {
			case "Point": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "MultiPoint": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			default: {
				// Error Handler
				goErrorsHandlers.internalErrorHandler.set(
					constants.ErrorSeverity.Warning,
					constants.internalErrors.INTERNAL_ERROR_GEOMETRY_NOT_FOUND,
					internalErrosMsg.INTERNAL_ERROR_GEOMETRY_NOT_FOUND
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					error: internalErrosMsg.INTERNAL_ERROR_GEOMETRY_NOT_FOUND,
				});
			}
		}
		return GOGeomType;
	}

	public getFilter(): string {
		if (this.getLayer().get("filter")) {
			return this.getLayer().get("filter");
		}
		return "";
	}
	/**
	 * Sets icons for the layer based on provided styles.
	 * @returns {Promise<void>} A promise that resolves when the icons are set.
	 * @method
	 */
	public setIcons() {
		// Destructure style options from this.layerOpts
		const { styleSVG, styleSVGClusterInd, styleSVGCluster } =
			this.layerOpts;

		if (styleSVG) this.svg = GoIcons.getSvgString(styleSVG);

		if (styleSVGClusterInd)
			this.svgClusterInd = GoIcons.getSvgString(styleSVGClusterInd);

		if (styleSVGCluster)
			this.svgCluster = GoIcons.getSvgString(styleSVGCluster);
	}

	/**
	 * Sets icons for the layer based on the provided styles.
	 * @param {string} [styleSVG] - SVG icon for individual features.
	 * @param {string} [styleSVGClusterInd] - SVG icon used when two features share the same coordinates.
	 * @param {string} [styleSVGCluster] - SVG icon displayed inside the cluster marker.
	 * @returns {Promise<void>} A promise that resolves when all icons have been set.
	 * @method
	 */
	public setCustomIcons(styleSVG?, styleSVGClusterInd?, styleSVGCluster?) {
		// Destructure style options from this.layerOpts

		if (styleSVG) this.svg = GoIcons.getSvgString(styleSVG);

		if (styleSVGClusterInd)
			this.svgClusterInd = GoIcons.getSvgString(styleSVGClusterInd);

		if (styleSVGCluster)
			this.svgCluster = GoIcons.getSvgString(styleSVGCluster);
	}
}

---
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\GoGeoJsonLayer\index.ts
import * as types from "@goTypes/index";
import { GOStagingMap } from "@map/index";
import { GoAxiosServices } from "@services/index";
import * as constants from "@utils/classes/GoError/constants";
import * as internalErrosMsg from "@utils/classes/GoError/errrosMsg/internalErros.msg";
import * as stylesErrorsMsg from "@utils/classes/GoError/errrosMsg/stylesErrors.msg";
import * as goErrorsHandlers from "@utils/classes/GoError/goErrorsHandlers";
import { internalErrorHandler } from "@utils/classes/GoError/goErrorsHandlers";
import {
	GOFeaturesEvents,
	GOGeometryType,
	GOLayerEvents,
} from "@utils/constants";
import { memorize } from "@utils/functions/mathFunctions";
import { GoIcons, StyleUtils } from "@utils/index";
import { AxiosResponse } from "axios";
import { CancelablePromise } from "cancelable-promise";
import { Feature } from "ol";
import GeoJSON from "ol/format/GeoJSON";
import VectorLayer from "ol/layer/Vector";
import VectorImageLayer from "ol/layer/VectorImage";
import RenderEvent from "ol/render/Event";
import { Vector } from "ol/source";
import VectorSource from "ol/source/Vector";
import "ol/style/Stroke";
import { GOStyledLayer } from "../GoStyledLayer";

/**
 * This class is used for the geoJson layer.
 * @class GOGeoJsonLayer
 * @augments  GOStyledLayer
 * @constructor
 * @param {import('types').layerOptions} layerOpts - The layer options.
 * @property {string} layerProjection
 * @param {string} topicId - The topic id.
 * @property {Style | undefined} styleUserGeojson
 * @property {string} toProjection
 */
export class GOGeoJsonLayer extends GOStyledLayer {
	public layerProjection: string | undefined;
	public toProjection: string | undefined;
	protected mapId: string | undefined;

	constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {
		super(stagingMap, layerOpts);
		this.mapId = stagingMap.getId();

		this.layerProjection = this.layerOpts.layerProjection
			? this.layerOpts.layerProjection.split(":").pop()?.toString()
			: "4326";
		this.toProjection = this.layerOpts.toProjection
			? this.layerOpts.toProjection.split(":").pop()?.toString()
			: undefined;

		this._initLayer();
	}

	private _initLayer() {
		const { layerOpts } = this;
		const { styleSVGConditional } = layerOpts;

		if (styleSVGConditional) {
			this.setStyleValues(styleSVGConditional.styleInfoSVG);
		}

		this.layerOpts = layerOpts;
		this.mapStyle.set("initStyle", new Map());

		const opacity = layerOpts.opacity || 1;
		const visibility: boolean =
			layerOpts.defaultVisibility === null ||
			layerOpts.defaultVisibility === undefined
				? true
				: layerOpts.defaultVisibility;

		if (layerOpts.url && layerOpts.data) {
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.id}: The "url" and "data" parameters cannot be used at the same time.`
			);
		}

		const vectorSource = new Vector({});

		if (layerOpts.disableImageEffect) {
			this.layer = new VectorLayer({
				source: vectorSource,
				visible: visibility,
				opacity,
			});
		} else {
			this.layer = new VectorImageLayer({
				source: vectorSource,
				visible: visibility,
				opacity,
			});
		}

		if (layerOpts.url) {
			this.getFeaturesfromUrl(layerOpts.url).then((features) => {
				try {
					//Validating data is GeoJSON.
					if (
						!features ||
						!features.type ||
						(features.type === "FeatureCollection" &&
							!Array.isArray(features.features))
					) {
						throw new Error(
							"The provided resource is not a valid GeoJSON"
						);
					}
					// Add features to the layer
					if (this.layerProjection && this.toProjection) {
						vectorSource.addFeatures(
							new GeoJSON({
								dataProjection: "EPSG:" + this.layerProjection,
								featureProjection: "EPSG:" + this.toProjection,
							})
								.readFeatures(features)
								.filter((feat) => feat.getGeometry())
						);
					} else {
						vectorSource.addFeatures(
							new GeoJSON()
								.readFeatures(features)
								.filter((feat) => feat.getGeometry())
						);
					}
				} catch (error: any) {
					// Manejo de error si el GeoJSON no es válido
					goErrorsHandlers.stylesErrorHandler.set(
						constants.ErrorSeverity.Error,
						constants.stylesErrors.STYLES_ERROR_CANT_LOAD,
						`${layerOpts.id}: The provided resource is not a valid GeoJSON.`
					);
					this.emit(GOLayerEvents.LAYER_ERROR, {
						error: error,
						layerId: layerOpts.id,
						mapId: this.mapId,
					});
					return;
				}

				if (layerOpts.sldStyle) {
					this.mapStyle
						.get("initStyle")
						?.set("sld", layerOpts.sldStyle);
					this.getStyleFromSLD(layerOpts.sldStyle).then((style) => {
						try {
							this.styleFunction = style;
							this.layer.on(
								"prerender",
								this.layerRenderListener
							);
							const olStyle = memorize((...rest) =>
								style.styleFunction(...rest)
							);
							// Save style in the stylelog for future use
							// FIXME: This is a temporal solution, when the migration is finished this will be removed
							this.layerStyle.addRetrocompatibilityInitalStyle(
								"sld",
								layerOpts.sldStyle,
								olStyle
							);
							// end save style
							this.layer.setStyle(olStyle);
							this.emit(GOLayerEvents.LAYER_STYLE_CHANGED, {
								mapId: this.mapId,
								layerId: this.layer.id,
								layer: this,
								style: this.layer.getStyle(),
							});
						} catch (error) {
							if (error instanceof Error) {
								// Error Handler
								goErrorsHandlers.stylesErrorHandler.set(
									constants.ErrorSeverity.Warning,
									constants.stylesErrors
										.STYLES_ERROR_CANT_LOAD,
									stylesErrorsMsg.STYLES_ERROR_CANT_LOAD,
									error
								);
							}
							this.emit(GOLayerEvents.LAYER_ERROR, {
								error: error,
							});
						}
					});
				}

				if (layerOpts.styleSVG) {
					try {
						this.mapStyle
							.get("initStyle")
							?.set("svg", layerOpts.styleSVG);
						const style = GoIcons.getSvgStyle(layerOpts.styleSVG);

						// Save style in the stylelog for future use
						// FIXME: This is a temporal solution, when the migration is finished this will be removed
						this.layerStyle.addRetrocompatibilityInitalStyle(
							"svg",
							layerOpts.styleSVG,
							style
						);
						// end save style
						this.layer.setStyle(style);
						this.emit(GOLayerEvents.LAYER_STYLE_CHANGED, {
							mapId: this.mapId,
							layerId: layerOpts.id,
							layer: this,
							style: this.layer.getStyle(),
						});
					} catch (error) {
						if (error instanceof Error) {
							// Error Handler
							goErrorsHandlers.stylesErrorHandler.set(
								constants.ErrorSeverity.Warning,
								constants.stylesErrors.STYLES_ERROR_CANT_LOAD,
								stylesErrorsMsg.STYLES_ERROR_CANT_LOAD,
								error
							);
						}
						this.emit(GOLayerEvents.LAYER_ERROR, {
							error: error,
						});
					}
				}
			});
		} else if (layerOpts.data) {
			this.addFeatures(layerOpts.data, true);
		}

		this.layer.setZIndex(layerOpts.zIndex);
		this.layer.set("id", layerOpts.id);
		this.layer.set("type", this.type());
		this.layer.setZIndex(layerOpts.zIndex);

		if (layerOpts.minResolution) {
			this.setMinResolution(layerOpts.minResolution);
		}

		if (layerOpts.maxResolution) {
			this.setMaxResolution(layerOpts.maxResolution);
		}
		this.setLayerStyle();
		this.layerPromise = Promise.resolve(this.layer);
	}

	/**
	 * This function alloow to refresh the layer
	 * @returns {void} Return nothing
	 * @method refresh
	 */
	public refresh(): Promise<Vector<Feature>> {
		return new Promise((resolve) => {
			const { url } = this.layerOpts;
			if (url) {
				this.getFeaturesfromUrl(url).then((features) => {
					const vectorSource = new Vector({
						features: new GeoJSON({
							dataProjection: "EPSG:" + this.layerProjection,
							featureProjection: "EPSG:" + this.toProjection,
						})
							.readFeatures(features)
							.filter((feat) => feat.getGeometry()),
					});

					this.layer.setSource(vectorSource);
					resolve(vectorSource);
				});
			} else {
				// Search fr the features in the layer and update the source with the new features array
				const features = this.layer.getSource().getFeatures();
				const vectorSource = new Vector({
					features: features,
				});
				this.layer.setSource(vectorSource);
				resolve(vectorSource);
			}
		});
	}

	/**
	 * Changes the style of the layer.
	 * @param {types.styleFromUser} style - The new style to apply.
	 */
	public changeStyle(style: types.styleFromUser): void {
		try {
			const geometry = this.getGeometryType(
				this.layer.getSource().getFeatures()[0].getGeometry().getType()
			);

			if (geometry === style.type) {
				this.layer.setStyle(
					memorize((style) => StyleUtils.newStyle(style))(style)
				);
				this.emit(GOLayerEvents.LAYER_STYLE_CHANGED, {
					mapId: this.mapId,
					layerId: this.layer.id,
					layer: this,
					style: this.layer.getStyle(),
				});
			} else {
				// Error Handler
				goErrorsHandlers.stylesErrorHandler.set(
					constants.ErrorSeverity.Warning,
					constants.stylesErrors.STYLES_ERROR_CANT_CREATE_STYLES,
					stylesErrorsMsg.STYLES_ERROR_CANT_CREATE_STYLES +
						`Cap:${geometry} Geometry style: ${style.type}.`
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					error:
						stylesErrorsMsg.STYLES_ERROR_CANT_CREATE_STYLES +
						`Cap:${geometry} Geometry style: ${style.type}.`,
				});
			}
		} catch (error: any) {
			// Error Handler
			goErrorsHandlers.stylesErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.stylesErrors.STYLES_ERROR_GEOMETRY_STYLE,
				error.message,
				error
			);
			this.emit(GOLayerEvents.LAYER_ERROR, {
				error: error,
			});
		}
	}

	/**
	 * Determines if the layer can be filtered.
	 * @returns {boolean} Always returns `false` for this layer type.
	 */
	public canFilter(): boolean {
		return false;
	}

	/**
	 * Returns the type of the layer.
	 * @returns {string} The type of the layer, which is "GOGeoJsonLayer".
	 */
	public type(): string {
		return "GOGeoJsonLayer";
	}

	/**
	 * Removes all features from the layer.
	 * @returns {void}
	 */
	public removeFeatures(): void {
		this.layer?.getSource().clear();
	}

	/**
	 * Adds features to the layer.
	 * @param {any} data - The data to be added to the layer, typically in GeoJSON format.
	 * @param {boolean} [clear=false] - Optional flag to clear existing features before adding new ones.
	 * @returns {void}
	 */
	public addFeatures(data, clear?: boolean): void {
		const { layerOpts } = this;

		if (
			typeof data !== "object" ||
			data === null ||
			!("default" in data) ||
			typeof data.default !== "object" ||
			data.default.type !== "FeatureCollection"
		) {
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				`${this.layerOpts.id}: The provided data is not a valid GeoJSON.`
			);
			return;
		}

		this.emit(GOFeaturesEvents.FEATURES_LOAD_STARTED, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});

		if (clear) {
			this.layer?.getSource().clear();
		}

		if (this.layerProjection) {
			this.layer?.getSource().addFeatures(
				new GeoJSON({
					dataProjection: "EPSG:" + this.layerProjection,
					featureProjection: "EPSG:" + this.toProjection,
				})
					.readFeatures(data.default)
					.filter((feat) => feat.getGeometry())
			);
		} else {
			this.layer
				?.getSource()
				.addFeatures(
					new GeoJSON()
						.readFeatures(data.default)
						.filter((feat) => feat.getGeometry())
				);
		}

		this.emit(GOFeaturesEvents.FEATURES_LOAD_ENDED, {
			mapId: this.mapId,
			layerId: layerOpts.id,
		});
	}

	/**
	 * This function allow to get the type of geometry of the layer
	 * @param {string} type - The type of geometry
	 * @returns {boolean} Return true if the type is supported
	 * @method
	 */
	private isGeometryType(type: string): boolean {
		return (
			type.toUpperCase() === "GEOMETRY" ||
			type.toUpperCase() === "MULTILINESTRING" ||
			type.toUpperCase() === "LINESTRING" ||
			type.toUpperCase() === "MULTIPOINT" ||
			type.toUpperCase() === "POINT" ||
			type.toUpperCase() === "POLYGON" ||
			type.toUpperCase() === "MULTIPOLYGON"
		);
	}

	/**
	 * Creates a GeoJSON string representation of the features in the layer.
	 * @returns {string} The GeoJSON string representing the features in the layer.
	 */
	public createGeoJson(): string {
		const writer = new GeoJSON();
		const geojsonStr = writer.writeFeatures(
			this.layer.getSource().getFeatures()
		);
		return geojsonStr;
	}

	/**
	 * Event listener for the "prerender" event of the layer.
	 * @param {RenderEvent} event - The render event.
	 * @returns {void}
	 */
	public layerRenderListener(event: RenderEvent): void {
		const geometryType = this.layer
			.getSource()
			.getFeatures()[0]
			.getGeometry()
			.getType();
		if (
			this.getStyleFunction().styleType !==
			this.getGeometryType(geometryType)
		) {
			// Error Handler
			goErrorsHandlers.internalErrorHandler.set(
				constants.ErrorSeverity.Warning,
				constants.internalErrors
					.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER,
				internalErrosMsg.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER +
					this.getAlias() +
					`geometry. Loaded Default style`
			);
			this.emit(GOLayerEvents.LAYER_ERROR, {
				error:
					internalErrosMsg.INTERNAL_ERROR_SLD_GEOMETRY_DONT_MATCH_LAYER +
					this.getAlias() +
					`geometry. Loaded Default style`,
			});
		}
		event.target.un("prerender", this.layerRenderListener);
	}

	/**
	 * Get the VectorLayer associated with this instance.
	 * @returns {VectorLayer<VectorSource>} The VectorLayer object.
	 * @method
	 */
	public getLayer(): VectorLayer<VectorSource> {
		return this.layer;
	}

	/**
	 * Get the name of the layer.
	 * @returns {string} The name of the layer.
	 * @method
	 */
	public getLayerName(): string {
		return this.layerOpts.layerName;
	}

	/**
	 * Get the name of the geometry used in the layer.
	 * @returns {Promise<string>} A Promise that resolves to the name of the geometry.
	 * @method
	 */
	public getGeometryName(): Promise<string> {
		return this.layer.get("geometryName");
	}

	/**
	 * Get the URL of the layer.
	 * @returns {string} The URL of the layer.
	 * @method
	 */
	public getUrl(): string {
		return this.layerOpts.url;
	}
	
	/**
	 * Get the data of the layer (required use data parameter via config), GeoJSON data source filled via config.
	 * @returns {string} The data source of the layer.
	 * @method
	 */
	public getDataLayer(): string {
		return this.layerOpts.data;
	}

	/**
	 * Get the visibility status of the layer.
	 * @returns {boolean} `true` if the layer is visible, `false` otherwise.
	 * @method
	 */
	public getVisibility(): boolean {
		return this.layer.getVisible();
	}

	/**
	 * Get features from a specified URL.
	 * @param {string} url - The URL from which to fetch features.
	 * @returns {CancelablePromise<Feature[]|Feature>} A CancelablePromise that resolves to an array of features or a single feature.
	 * @method
	 */
	public getFeaturesfromUrl(
		url: string
	): CancelablePromise<Feature[] | Feature> {
		return new CancelablePromise<Feature[] | Feature>((resolve) => {
			GoAxiosServices.sendRequest({
				url,
				method: "GET",
				timeout: 20000,
			}).then((response: AxiosResponse) => {
				resolve(response.data);
			});
		});
	}

	/**
	 * Get the geometry type based on a given layer geometry type.
	 * @param {string} layerGeomType - The layer's geometry type.
	 * @returns {GOGeometryType} The corresponding GOGeometryType.
	 * @method
	 */
	public getGeometryType(layerGeomType): GOGeometryType {
		let GOGeomType: GOGeometryType;
		switch (layerGeomType) {
			case "Point": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "MultiPoint": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "LineString": {
				GOGeomType = GOGeometryType.LINE_STRING;
				break;
			}
			case "MultiLineString": {
				GOGeomType = GOGeometryType.LINE_STRING;
				break;
			}
			case "Polygon": {
				GOGeomType = GOGeometryType.POLYGON;
				break;
			}
			case "MultiPolygon": {
				GOGeomType = GOGeometryType.POLYGON;
				break;
			}
			default: {
				// Error Handler
				internalErrorHandler.set(
					constants.ErrorSeverity.Error,
					constants.internalErrors
						.INTERNAL_ERROR_GEOMETRY_NOT_SUPORTED,
					internalErrosMsg.INTERNAL_ERROR_GEOMETRY_NOT_SUPORTED
				);
				this.emit(GOLayerEvents.LAYER_ERROR, {
					error: internalErrosMsg.INTERNAL_ERROR_GEOMETRY_NOT_SUPORTED,
				});
			}
		}
		return GOGeomType;
	}
}

---
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\GOWMS\index.ts
import * as types from "@goTypes/index";
import { GOStagingMap } from "@map/index";
import { GoAxiosServices, GoGeoServices, GoStore } from "@services/index";
import { goErrorsHandlers } from "@utils/classes/GoError";
import * as constants from "@utils/classes/GoError/constants";
import { internalErrosMsg } from "@utils/classes/GoError/errrosMsg";
import { INTERNAL_ERROR_CAN_LOAD_LAYER } from "@utils/classes/GoError/errrosMsg/internalErros.msg";
import { internalErrorHandler } from "@utils/classes/GoError/goErrorsHandlers";
import { GOLayerEvents } from "@utils/constants";
import AuthTileWMS from "@utils/ext-libs/ol/AuthTileWMS";
import { blobToDataURL } from "@utils/index";
import * as SLDReader from "@utils/sldreader/index";
import { AxiosResponse } from "axios";
import { CancelablePromise } from "cancelable-promise";
import TileLayer from "ol/layer/Tile";
import TileSource from "ol/source/Tile";
import { GOLayer } from "../GoLayer";
import {
	getObjectParamsFromUrl
} from "@utils/functions";

/**
 * This class is used to create a layer.
 * @class GOWMSLayer
 * @augments  GOLayer
 * @constructor
 * @param {layerOptions} layerOpts - The layer options.
 * @param {string} topicId - The topic id.
 */
export class GOWMSLayer extends GOLayer {
	public layerProjection: string | undefined;

	constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {
		super(stagingMap, layerOpts);
		this.layerProjection = layerOpts.layerProjection
			? layerOpts.layerProjection.split(":").pop()?.toString()
			: "4326";
		this._initLayer();
	}

	private _initLayer(): void {
		const { layerOpts } = this;
		let zIndex;

		if (!layerOpts.zIndex) {
			zIndex = 0;
		} else {
			zIndex = layerOpts.zIndex;
		}

		const visibility: boolean =
			layerOpts.defaultVisibility === null ||
			layerOpts.defaultVisibility === undefined
				? true
				: layerOpts.defaultVisibility;

		if (!layerOpts.layerName) {
			// Error Handler
			internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.layerName}: parameter layerName not found`
			);
			this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
				error:
					INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.layerName}: parameter layerName not found`,
			});
		}
		if (!layerOpts.url) {
			// Error Handler
			internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.layerName}: parameter url not found`
			);
			this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
				error:
					INTERNAL_ERROR_CAN_LOAD_LAYER +
					`${layerOpts.layerName}: parameter url not found`,
			});
		}

		const wmsUrl = layerOpts.url;

		let geoparams: Record<string, string> = {};

		if (layerOpts.geoparams) {
			geoparams = layerOpts.geoparams.split("&").reduce(
				(params, param) => {
					const [key, value] = param.split("=");
					params[key] = value;
					return params;
				},
				{} as Record<string, string>
			);
		}

		if (layerOpts.sldStyle) {
			// Toma el nombre del sld al final de la url para pasarle el style
			const url = layerOpts.sldStyle.url;
			const styleName = url.split("/").pop()?.replace(".sld", "");
			geoparams["STYLES"] = styleName || "";
		}

		this.layer = new TileLayer({
			preload: Infinity,
			source: new AuthTileWMS({
				url: wmsUrl,
				params: {
					LAYERS: layerOpts.layerName,
					VERSION: layerOpts.version ? layerOpts.version : "1.3.0",
					...geoparams,
				},
			}),
			visible: visibility,
		});

		if (layerOpts.minResolution) {
			this.setMinResolution(layerOpts.minResolution);
		}

		if (layerOpts.maxResolution) {
			this.setMaxResolution(layerOpts.maxResolution);
		}

		this.layer.setZIndex(zIndex);
		this.layer.set("id", layerOpts.id);
		this.layer.set("type", this.type());

		this.layerPromise = Promise.resolve(this.layer);
	}

	/**
	 * Returns the type of the layer.
	 *
	 * @returns {string} The type of the layer, which is "GoWMS".
	 */
	public type(): string {
		return "GoWMS";
	}

	/**
	 * Determines if the layer supports filtering.
	 *
	 * @returns {boolean} `false`, as WMS layers do not support filtering.
	 */
	public canFilter(): boolean {
		return false;
	}

	/**
	 * Retrieves the TileLayer instance associated with this layer.
	 *
	 * @returns {TileLayer<TileSource>} The TileLayer instance.
	 */
	public getLayer(): TileLayer<TileSource> {
		return this.layer;
	}

	/**
	 * Retrieves the visibility status of the layer.
	 *
	 * @returns {boolean} `true` if the layer is visible, `false` otherwise.
	 */
	public getVisibility(): boolean {
		return this.layer.getVisible();
	}

	public getLayerDescription(): string {
		return `WMS Layer`;
	}

	/**
	 * Retrieves the legend graphic for the WMS layer.
	 *
	 * @param {string} [layerOptsLegend] - Optional legend parameters to append to the URL.
	 * @returns {Promise<{legend: string | null, url_Own: string}>} A promise that resolves to an object containing the legend URL and the request URL.
	 */
	public async getLegend(layerOptsLegend: string) {
		const geoserverUrl = this.layerOpts.url
			? new URL(this.layerOpts.url).origin
			: `${GoStore.getState().geoserverAuthData?.url}`;
		const { enviroment } = GoStore.getState();
		
		let sldStyleToApply = "";

		if (this.layerOpts.geoparams) {
			const geoparams: Record<string, string> =
				this.layerOpts.geoparams.split("&").reduce(
					(params, param) => {
						const [key, value] = param.split("=");
						params[key] = value;
						return params;
					},
					{} as Record<string, string>
				);
			sldStyleToApply = geoparams.STYLES;
		}

		if(this.layerOpts.sldStyle){
			sldStyleToApply = this.layerOpts.sldStyle.url
				.split("/")
				.pop()
				?.split(".")[0] || sldStyleToApply;
		}

		const legendUrl = `${geoserverUrl}/geoserver/wms?REQUEST=GetLegendGraphic&FORMAT=image/png&WIDTH=20&HEIGHT=20&LAYER=${this.layerOpts.layerName}&STYLE=${sldStyleToApply}&LEGEND_OPTIONS=${layerOptsLegend}`;

		try {
			const axiosConfig = {
				url: `${enviroment}/gis-apirest/api/geoserver/sld`,
				method: "POST",
				timeout: 20000,
				headers: {
					"Accept": "application/json, text/plain, */*",
					"Content-Type": "application/json",
				},
				data: getObjectParamsFromUrl(`${legendUrl}`),
				responseType: "arraybuffer",
			};

			// eslint-disable-next-line @typescript-eslint/no-explicit-any
			const response = await GoAxiosServices.sendRequest(
				axiosConfig as any,
				false
			);

			let blob: Blob | null = null;

			if (response.data instanceof ArrayBuffer) {
				blob = new Blob([response.data], { type: "image/png" });
			} else if (typeof response.data === "string") {
				const bytes = new Uint8Array(response.data.length);
				for (let i = 0; i < response.data.length; i++) {
					bytes[i] = response.data.charCodeAt(i) & 0xff;
				}
				blob = new Blob([bytes], { type: "image/png" });
			} else if (response.data instanceof Blob) {
				blob = response.data;
			}

			if (blob) {
				return await blobToDataURL(blob);
			}

			return null;
		} catch (err) {
			console.error("Error cargando leyenda:", err);
			return null;
		}
	}

	/**
	 * Retrieves the SLD (Styled Layer Descriptor) configuration for the layer.
	 *
	 * @param {types.styleOptions} sldStyle - The style options including the SLD URL.
	 * @param {string} idLayer - The ID of the layer.
	 * @returns {CancelablePromise<types.sldConfiguration>} A promise that resolves to the SLD configuration.
	 */
	public getSLD(
		sldStyle: types.styleOptions,
		idLayer: string
	): CancelablePromise<types.sldConfiguration> {
		return new CancelablePromise((resolve) => {
			GoAxiosServices.sendRequest({
				url: sldStyle.url,
				method: "GET",
				timeout: 20000,
			}).then((response: AxiosResponse) => {
				const sldObject = SLDReader.Reader(response.data);
				try {
					const data: types.sldConfiguration = {
						sldObject: sldObject,
						idLayer: idLayer,
					};
					resolve(data);
				} catch (error: unknown) {
					const errorObject =
						error && typeof error === "object" ? error : undefined;
					goErrorsHandlers.internalErrorHandler.set(
						constants.ErrorSeverity.Warning,
						constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
						internalErrosMsg.INTERNAL_ERROR_CAN_LOAD_LAYER,
						errorObject
					);
					this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
						error: error,
					});
				}
			});
		});
	}
}

---
FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Layers\GoWFS\index.ts
import * as types from "@goTypes/index";
import { GOStagingMap } from "@map/index";
import { goErrorsHandlers } from "@utils/classes/GoError";
import * as constants from "@utils/classes/GoError/constants";
import { INTERNAL_ERROR_CAN_LOAD_LAYER } from "@utils/classes/GoError/errrosMsg/internalErros.msg";
import * as goErrorsHandlers_1 from "@utils/classes/GoError/goErrorsHandlers";
import { internalErrorHandler } from "@utils/classes/GoError/goErrorsHandlers";
import { GOGeometryType, GOLayerEvents, GOLayerType } from "@utils/constants";
import { memorize } from "@utils/functions/mathFunctions";
import { StyleUtils, nextFrame } from "@utils/index";
import axios, { AxiosResponse, CancelToken } from "axios";
import { CancelablePromise } from "cancelable-promise";
import { Feature, Map as OlMap } from "ol";
import { Extent } from "ol/extent";
import EsriJSON from "ol/format/EsriJSON";
import GeoJSON from "ol/format/GeoJSON";
import Geometry from "ol/geom/Geometry";
import VectorLayer from "ol/layer/Vector";
import VectorImageLayer from "ol/layer/VectorImage";
import { bbox as bboxStrategy } from "ol/loadingstrategy";
import { default as Vector, default as VectorSource } from "ol/source/Vector";
import "ol/style/Stroke";
import uniqid from "uniqid";
import { transformExtent } from "ol/proj";
import { GOStyledLayer } from "../GoStyledLayer";
import { requestManager } from "@services/GoAxiosServices/functions/sendRequestFun";

/**
 * @classdesc
 * Allows to load a WFS layer directly from the config.
 * The WFS can come from Esri or from Geoserver
 * ```ts
 * config example:
     {
                    "id": "redesgeodesicas",
                    "alias": "Redes Geodesicas",
                    "layerType": "GO_WFS_LAYER",
                    "isBaseLayer": false,
                    "opacity": 1,
                    "url":"https://www.ign.es/wfs/redes-geodesicas?TYPENAME=RED_ERGNSS",
                    "layerProjection": "4326",
                    "urlFolder": "Agua potable/redesgeodesicas/",
                    "esri":false,
                    "layerName": "redesgeodesicas",
                    "defaultVisibility": true,
                    "zIndex": 9999,
                    "styleWFS":{
                        "radius":3,
                        "fillcolor":"orange",
                        "strokecolor":"black"
                    }
                },
                {
                    "id": "EsriTestToken",
                    "alias": "Esri Test token",
                    "layerType": "GO_WFS_LAYER",
                    "isBaseLayer": false,
                    "opacity": 1,
                    "url":"https://srvesripre.aguasdevalencia.es/server/rest/services/UtilityNetworkGO/FeatureServer/0",
                    "user":"SergioAzCa",
                    "password":"Aguas2021",
                    "layerProjection": "4326",
                    "WFStoken":"h47BtW9lZnD7UoWJGieM4sZacpH_20GJlgaTogmgdKJh2-lVM0NvzIaN3qtQA_7ZZgAiGajU0Hwi1MCvwaLAws6mmSkr4stmMrR4zxwGxqrb1p_zrvee8CJCcskj4swR45Vp2R4_3dq6GIcEM7uMXRD-80HQhZr_PXMW5rt2RiNhu17-IsaDimXwvan-qDT_YVYwoW0-EpFYtalQhr8c1DEl8HRXbwXYrEozMqL1d_dT28rnQB-t7t2NXm72bHby",
                    "esri":true,
                    "urlFolder": "Agua potable/Poligonos/",
                    "layerName": "Poligonos",
                    "defaultVisibility": true,
                    "zIndex": 9999
                },
                {
                    "id": "EsriTestNoToken",
                    "alias": "Esri test no token",
                    "layerType": "GO_WFS_LAYER",
                    "isBaseLayer": false,
                    "opacity": 1,
                    "url":"https://sampleserver6.arcgisonline.com/arcgis/rest/services/USA/MapServer/1",
                    "layerProjection": "4326",
                    "esri":true,
                    "urlFolder": "Agua potable/Poligonos/",
                    "layerName": "Poligonos",
                    "defaultVisibility": true,
                    "zIndex": 9999
                },

```
* @param {esri} boolean String to know if the wfs comes from Esri or from Geoserver.
* @param {user} string String with the user in case that we need user and pass to get the data.
* @param {password} string String with the password in case that we need user and pass to get the data.
* @augments  GOStyledLayer
* @api
*/
export class GOWFSLayer extends GOStyledLayer {
	private vectorSource: VectorSource | undefined;
	protected geomName!: string;
	private map: OlMap;
	public layerProjection: string | undefined;
	protected token = "";
	public useBboxFilter;
	protected wait = false;
	protected zIndex: number;

	constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {
		super(stagingMap, layerOpts);
		this.map = this.olMap;
		this.layerProjection = layerOpts.layerProjection ? layerOpts.layerProjection.split(":").pop()?.toString() : "4326";
		this.useBboxFilter = layerOpts.useBboxFilter !== undefined ? layerOpts.useBboxFilter : true;
		this.zIndex = layerOpts.zIndex || 1;
		this._initLayer();
	}

	private transformExtend = memorize(
		({
			extent,
			srcProjection,
			toProjection,
		}: {
			extent: Extent;
			srcProjection: string;
			toProjection: string;
		}): Extent => transformExtent(extent, srcProjection, toProjection)
	);

	private _initLayer() {
		const { layerOpts } = this;
		if (layerOpts.styleSVGConditional) {
			this.setStyleValues(layerOpts.styleSVGConditional.styleInfoSVG);
		}

		this.mapStyle.set("initStyle", new Map());
		this.mapStyle.get("initStyle")!.set("", null);
		const opacity = layerOpts.opacity || 1;

		const visibility: boolean =
			layerOpts.defaultVisibility === null ||
			layerOpts.defaultVisibility === undefined
				? true
				: layerOpts.defaultVisibility;

		this.validateLayerConfig(layerOpts);

		// eslint-disable-next-line
		const ref = this;
		if (layerOpts.esri) {
			this.vectorSource = new Vector({
				format: new EsriJSON(),
				loader: this.loader.bind(this),
				strategy: bboxStrategy,
			});
		} else {
			const ref = this;
			this.vectorSource = new VectorSource({
				format: new GeoJSON(),
				url: function (extent) {
					const mapProjection = ref.map.getView().getProjection().getCode().split(":").pop()?.toString();
					if (mapProjection !== ref.layerProjection) {
						extent = ref.transformExtend({
							extent,
							srcProjection: `EPSG:${mapProjection}`,
							toProjection: `EPSG:${ref.layerProjection}`,
						});
					}
					return (
						ref.layerOpts.url +
						"&TYPENAME=" +
						ref.layerOpts.layerName +
						"&SERVICE=WFS&REQUEST=GetFeature&VERSION=1.0.0&outputFormat=application/json&srsname=EPSG:" +
						ref.layerProjection +
						"&" +
						"bbox=" +
						extent.join(",")
					);
				},
				strategy: bboxStrategy,
			});
		}

		this.layer = new VectorLayer({
			source: this.vectorSource,
			visible: visibility,
			opacity,
		});

		this.applyConfigToLayer(layerOpts);

		this.layerPromise = Promise.resolve(this.layer);
	}

	/**
	 * Loads data from the WFS service, including generating an ESRI token if necessary.
	 *
	 * @param {Extent} extent - The extent to use when querying for features.
	 * @returns {Promise<AxiosResponse<any>>} A promise that resolves to the response from the WFS service.
	 */
	// eslint-disable-next-line
	public async loader(extent: Extent): Promise<unknown> {
		if (this.consumeCancellationFlag()) {
			return;
		}

		if (!this.layer) {
			console.warn(
				`[${this.layerOpts.id}] WFS loader triggered before layer initialization. Skipping current load cycle.`
			);
			return;
		}

		const layerSource = this.layer.getSource();
		if (!layerSource) {
			console.warn(
				`[${this.layerOpts.id}] WFS loader cannot proceed because the layer source is not ready yet.`
			);
			return;
		}

		await nextFrame();
		const res = await this.getTokenUrlFromEsriWFS();
		if (res && res.data && res.data.authInfo) {
			const tokenServicesUrl = res.data.authInfo.tokenServicesUrl;
			// Attempt to get token only if credentials are provided
			const tokenResponse = await this.gettokenfromEsriWFS(tokenServicesUrl);
			if (tokenResponse && tokenResponse.data) {
				const token = tokenResponse.data.token;
				if (token && token !== "") {
					this.token = token;
				}
			}
			// If no token obtained (e.g., no credentials), proceed without it
		}
		const url: string | undefined = this.layerOpts.url;

		const urlesri =
			url +
			"/query/?f=json&" +
			"returnGeometry=true&spatialRel=esriSpatialRelIntersects&geometry=" +
			extent.join(",") +
			"&geometryType=esriGeometryEnvelope&inSR=" +
			this.layerProjection +
			"&OutSR=" +
			this.layerProjection;
		let config: { headers?: { Authorization: string }; cancelToken?: CancelToken } = {};
		if (this.token && this.token != "") {
			config = {
				headers: {
					Authorization: "Bearer " + this.token,
				},
			};
		}
		const requestId = uniqid();

		config.cancelToken = requestManager.createCancelToken(requestId);

		const response = await axios.get(urlesri, config).finally(() => {
			requestManager.remove(requestId);
		});

		if (
			typeof response.data === "string" &&
			response.data.includes("?xml") &&
			response.data.includes("ServiceExceptionReport")
		) {
			if (!layerSource.get("firstTime")) {
				goErrorsHandlers_1.internalErrorHandler.set(
					constants.ErrorSeverity.Error,
					constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
					INTERNAL_ERROR_CAN_LOAD_LAYER +
						`Error en la precarga de los datos`
				);
				layerSource.set("firstTime", true);
			}
		} else {
			if (response.data && response.data.features.length) {
				layerSource.clear();
				layerSource.addFeatures(
					layerSource
						.getFormat()
						.readFeatures(response.data)
						.filter((feat) =>
							feat.getGeometry()
						) as Feature<Geometry>[]
				);
			} else {
				goErrorsHandlers.internalErrorHandler.set(
					constants.ErrorSeverity.Info,
					constants.internalErrors.INTERNAL_ERROR_DATA,
					`Layer ${this.layerOpts.alias} response WFS data is empty`
				);
				this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
					error: `Layer ${this.layerOpts.alias} response WFS data is empty`,
				});
			}
		}
		layerSource.set("firstTime", false);
		return response;
	}

	/**
	 * Validate the layer configuration
	 * @param layerOpts - types.layerOptions
	 */
	public validateLayerConfig(layerOpts: types.layerOptions) {
		if (!layerOpts.layerName) {
			throw new Error(
				`No se puede cargar la capa ${layerOpts.layerName}: parametro layerName no encontrado`
			);
		}
		if (!layerOpts.url) {
			throw new Error(
				`No se puede cargar la capa ${layerOpts.url}: parametro url no encontrado`
			);
		}
		if (!this.layerProjection) {
			throw new Error(
				`No se puede cargar la capa ${layerOpts.layerName}: parametro layerProjection no encontrado`
			);
		}
	}

	/**
	 * Checks if the data has been loaded within a certain time interval.
	 *
	 * @param {number} [timeInterval=1000] - The time interval (in milliseconds) to check for data loading.
	 * @param {number} [limit=0] - The number of times to check before giving up.
	 * @returns {CancelablePromise<boolean>} A promise that resolves to `true` if data is loaded, `false` otherwise.
	 */
	public isLoadedData(
		timeInterval = 1000,
		limit = 0
	): CancelablePromise<boolean> {
		return new CancelablePromise((resolve, reject) => {
			try {
				// eslint-disable-next-line
				const intervals: Array<any> = [];
				let times = 0;
				const interval = setInterval(() => {
					times++;
					intervals.push(interval);
					const features = this.layer.getSource().getFeatures();
					if (limit === times) {
						this.clearAllIntervals(intervals);
						resolve(false);
					} else if (features.length) {
						this.clearAllIntervals(intervals);
						resolve(true);
					}
				}, timeInterval);
			} catch (error) {
				reject();
			}
		});
	}

	/**
	 * Refreshes the data in the layer by forcing a refresh of the source.
	 */
	public refresh(): void {
		this.layer.getSource().refresh({ force: true });
	}

	/**
	 * Changes the style of the layer based on user-defined style options.
	 *
	 * @param {types.styleFromUser} style - The style options to apply to the layer.
	 */
	public changeStyle(style: types.styleFromUser): void {
		try {
			const geometry = this.getGeometryType(
				this.layer.getSource().getFeatures()[0].getGeometry().getType()
			);

			if (geometry === style.type) {
				this.layer.setStyle(
					memorize((style) => StyleUtils.newStyle(style))(style)
				);

				this.eventEmitter.fire(GOLayerEvents.LAYER_STYLE_CHANGED, {
					layer: this,
					style: this.layer.getStyle(),
				});
			} else {
				goErrorsHandlers_1.stylesErrorHandler.set(
					constants.ErrorSeverity.Error,
					constants.stylesErrors.INTERNAL_ERROR_CHANGE_STYLE,
					INTERNAL_ERROR_CAN_LOAD_LAYER +
						`El estilo definido no tiene tipo : ${style.type} `
				);
			}
		} catch (e) {
			goErrorsHandlers_1.stylesErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.stylesErrors.INTERNAL_ERROR_CHANGE_STYLE,
				INTERNAL_ERROR_CAN_LOAD_LAYER + `Error al cambiar el estilo`
			);
		}
	}

	/**
	 * Checks if the layer has a filter applied.
	 *
	 * @returns {boolean} `true` if a filter is applied to the layer, otherwise `false`.
	 */
	public hasFilter(): boolean {
		return this.getLayer().get("filter") !== null;
	}

	/**
	 * Determines if the layer can be filtered.
	 *
	 * @returns {boolean} `true` if the layer can be filtered, otherwise `false`.
	 */
	public canFilter(): boolean {
		return true;
	}

	/**
	 * Returns the type of the layer.
	 *
	 * @returns {string} The type of the layer.
	 */
	public type(): string {
		return GOLayerType.GO_WFS_LAYER;
	}

	/**
	 * Exports the features of the layer in GeoJSON format.
	 * @returns {string | null} A GeoJSON string representing the features in the layer, or `null` if no features are present.
	 */
	public exportGeoJsonFormatFeatures() {
		// eslint-disable-next-line
		const geom: Array<any> = [];
		const writer = new GeoJSON();
		this.layer.getSource().forEachFeature(function (feature) {
			geom.push(new Feature(feature.getGeometry()));
		});
		if (geom.length) {
			const geoJsonStr = writer.writeFeatures(geom);
			return geoJsonStr;
		} else {
			return null;
		}
	}

	/**
	 * Checks if the given type is a valid geometry type.
	 * @param {string} type - The geometry type to validate.
	 * @returns {boolean} `true` if the type is a known geometry type, otherwise `false`.
	 */
	protected isGeometryType(type: string): boolean {
		return (
			type.toUpperCase() === "GEOMETRY" ||
			type.toUpperCase() === "MULTILINESTRING" ||
			type.toUpperCase() === "LINESTRING" ||
			type.toUpperCase() === "MULTIPOINT" ||
			type.toUpperCase() === "POINT" ||
			type.toUpperCase() === "POLYGON" ||
			type.toUpperCase() === "MULTIPOLYGON"
		);
	}

	/**
	 * Handles the layer's render events to check for style and geometry type consistency.s
	 * This method checks if the style type set for the layer matches the geometry type of the features. If there is a mismatch, it logs an error. The method is then unregistered from the `prerender` event.
	 * @param {any} event - The render event object.
	 */
	// eslint-disable-next-line
	public layerRenderListener(event: any): void {
		const geometryType = this.layer
			.getSource()
			.getFeatures()[0]
			.getGeometry()
			.getType();
		if (
			this.getStyleFunction().styleType !==
			this.getGeometryType(geometryType)
		) {
			goErrorsHandlers_1.internalErrorHandler.set(
				constants.ErrorSeverity.Error,
				constants.internalErrors.INTERNAL_ERROR_GEOMETRY_NOT_FOUND,
				INTERNAL_ERROR_CAN_LOAD_LAYER +
					`No coincide el tipo de estilo ${
						this.getStyleFunction().styleType
					} con el tipo de geometría : ${this.getGeometryType(
						geometryType
					)}`
			);
		}
		event.target.un("prerender", this.layerRenderListener);
	}

	/**
	 * It takes an array of intervals and clears them all.
	 * @param {any[]} intervals - any[]
	 */
	// eslint-disable-next-line
	private clearAllIntervals(intervals: any[]) {
		// eslint-disable-next-line
		intervals.forEach((interval: any) => {
			clearInterval(interval);
		});
	}

	protected async getTokenUrlFromEsriWFS(): Promise<AxiosResponse | undefined> {
		const url: string | undefined = this.layerOpts.url;
		return await this.getEsritokenServicesUrl(url as string);
	}

	/**
	 * This function return the Esri token
	 * @return - token
	 */
	protected async gettokenfromEsriWFS(esritokenServicesUrl: string) {
		const user = this.layerOpts.user;
		const pass = this.layerOpts.password;
		if (!user || !pass || user.trim() === "" || pass.trim() === "") {
			// No credentials provided; skip token request for public layers
			internalErrorHandler.set(
				constants.ErrorSeverity.Info,
				constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
				`Layer ${this.layerOpts.alias}: No Esri credentials provided; assuming public access`
			);
			return null; // Return null to indicate no token needed
		}
		return await this.getEsriToken(user, pass, esritokenServicesUrl);
	}

	/**
	 * This function return the Esri token
	 * @return - token
	 */
	protected getEsriToken(
		user: string,
		pass: string,
		tokenServicesUrl: string
	) {
		try {
			const baseurlsplit = tokenServicesUrl.split("/");
			if (baseurlsplit && baseurlsplit.length) {
				const protocol = baseurlsplit[0];
				const host = baseurlsplit[2];
				const baseurl = protocol + "//" + host;

				// Use GET with query params instead of POST FormData
				const params = new URLSearchParams({
					f: "json",
					username: user,
					password: pass,
					referer: baseurl,
				});

				return axios.get(`${tokenServicesUrl}?${params.toString()}`);
			}
		} catch (ex) {
			console.log("Authentication failed due to unexpected error.");
		}
	}

	/**
	 * This function return the Esri url to generate an Esri Token
	 * @return - tokenURLEsri
	 */
	protected getEsritokenServicesUrl(url: string) {
		try {
			const spliturl = url.split("/services/");
			if (spliturl && spliturl.length) {
				if (spliturl[0] != "" && spliturl[0] != undefined) {
					const tokenurl = spliturl[0] + "/info?f=pjson";
					// eslint-disable-next-line
					const config: any = {
						method: "get",
						url: tokenurl,
					};
					return axios(config);
				}
			}
		} catch (ex) {
			console.log("Authentication failed due unexpected error.");
		}
	}

	/**
	 * Retrieves the OpenLayers vector layer instance.
	 *
	 * This method returns the `VectorLayer` instance that is associated with this layer.
	 *
	 * @returns {VectorLayer<VectorSource>} The OpenLayers vector layer.
	 */
	public getLayer(): VectorLayer<VectorSource> {
		return this.layer;
	}

	/**
	 * Retrieves the name of the layer.
	 *
	 * This method returns the name of the layer as specified in the layer options.
	 *
	 * @returns {string} The name of the layer.
	 */
	public getLayerName(): string {
		return this.layerOpts.layerName as string;
	}

	/**
	 * Asynchronously retrieves the geometry name from the layer's metadata.
	 *
	 * This method fetches the geometry name stored in the layer's metadata. It returns a promise that resolves to the geometry name.
	 *
	 * @returns {Promise<string>} A promise that resolves to the geometry name.
	 */
	public async getGeometryName(): Promise<string> {
		const response = await this.layer.get("geometryName");
		return response;
	}

	/**
	 * Retrieves the projection code of the layer.
	 *
	 * This method returns "4326" if no projection is specified.
	 *
	 * @returns {string} The projection code of the layer, or `4326` if not specified.
	 */
	public getLayerProjection(): string {
		return this.layerProjection || "";
	}

	/**
	 * Retrieves the URL associated with the layer.
	 *
	 * This method returns the URL from which the layer's data is fetched.
	 *
	 * @returns {string} The URL associated with the layer.
	 */
	public getUrl(): string {
		return this.layerOpts.url || "";
	}

	/**
	 * Checks whether the layer is visible.
	 *
	 * This method returns a boolean indicating whether the layer is currently visible on the map.
	 *
	 * @returns {boolean} `true` if the layer is visible, otherwise `false`.
	 */
	public getVisibility(): boolean {
		return this.layer.getVisible();
	}

	/**
	 * Retrieves the filter applied to the layer.
	 *
	 * This method returns the filter string associated with the layer, formatted according to the bounding box (`bbox`) if it exists. If no filter is applied, it returns an empty string.
	 *
	 * @returns {string} The filter string, or an empty string if no filter is applied.
	 */
	public getFilter(): string {
		if (this.getLayer().get("filter")) {
			return this.formatFilter(
				this.getLayer().get("filter"),
				this.getLayer().get("bbox")
			);
		}

		return "";
	}

	/**
	 * It takes a GeometryType and returns a GOGeometryType.
	 * @param {GeometryType} layerGeomType - GeometryType
	 * @returns A string.
	 */
	protected getGeometryType(layerGeomType): GOGeometryType {
		let GOGeomType: GOGeometryType;
		switch (layerGeomType) {
			case "Point": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "MultiPoint": {
				GOGeomType = GOGeometryType.POINT;
				break;
			}
			case "LineString": {
				GOGeomType = GOGeometryType.LINE_STRING;
				break;
			}
			case "MultiLineString": {
				GOGeomType = GOGeometryType.LINE_STRING;
				break;
			}
			case "Polygon": {
				GOGeomType = GOGeometryType.POLYGON;
				break;
			}
			case "MultiPolygon": {
				GOGeomType = GOGeometryType.POLYGON;
				break;
			}
			default: {
				throw new Error("Geometria no soportada");
			}
		}
		return GOGeomType;
	}

	/**
	 * This function allow to get the layer description.
	 * @returns A promise that resolves to a string.
	 */
	public getLayerDescription(): CancelablePromise<string> {
		if (this.layerOpts.esri) {
			// For Esri layers, use REST API with ?f=pjson
			const url = `${this.layerOpts.url}?f=pjson`;
			return new CancelablePromise((resolve) => {
				axios.get(url).then((response: AxiosResponse) => {
					let geometryField = "";
					try {
						// In Esri JSON response, find the field with type === "esriFieldTypeGeometry"
						response.data.fields.forEach((field: { name: string; type: string }) => {
							if (field.type === "esriFieldTypeGeometry") {
								geometryField = field.name;
								this.layer.set("geometryType", field.type);
							}
						});
						if (!geometryField) {
							internalErrorHandler.set(
								constants.ErrorSeverity.Error,
								constants.internalErrors.INTERNAL_ERROR_GEOMETRY_NOT_FOUND_LAYER_ID,
								`Layer ${this.layerOpts.alias} GeometryField not found in Esri response`
							);
							this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
								error: `Layer ${this.layerOpts.alias} GeometryField not found in Esri response`,
							});
						}
						resolve(geometryField);
					} catch (e) {
						internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
							`Layer ${this.layerOpts.alias} error parsing Esri layer description: ${e}`
						);
						this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
							error: `Layer ${this.layerOpts.alias} error parsing Esri layer description: ${e}`,
						});
					}
				});
			});
		} else {
			// For GeoServer or other, use WFS as before
			const url =
				this.layerOpts.url +
				"/wfs?service=WFS&version=1.0.0&request=describeFeatureType&typename=" +
				this.layerOpts.layerName +
				"&outputFormat=application/json&";

			return new CancelablePromise((resolve) => {
				axios.get(url).then((response: AxiosResponse) => {
					let geometryField = "";
					try {
						response.data.featureTypes[0].properties.forEach(
							// eslint-disable-next-line
							(element: any) => {
								if (this.isGeometryType(element.localType)) {
									geometryField = element.name;
									this.layer.set(
										"geometryType",
										element.localType
									);
								}
							}
						);
						if (!geometryField) {
							internalErrorHandler.set(
								constants.ErrorSeverity.Error,
								constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
								`Layer ${this.layerOpts.alias} GeometryField is null`
							);
							this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
								error: `Layer ${this.layerOpts.alias} GeometryField is null`,
							});
						}

						resolve(geometryField);
					} catch (e) {
						internalErrorHandler.set(
							constants.ErrorSeverity.Error,
							constants.internalErrors.INTERNAL_ERROR_CAN_LOAD_LAYER,
							`Layer ${this.layerOpts.alias} have an error in get geometry type of layer error: ${e}`
						);
						this.eventEmitter.fire(GOLayerEvents.LAYER_ERROR, {
							error: `Layer ${this.layerOpts.alias} have an error in get geometry type of layer error: ${e}`,
						});
					}
				});
			});
		}
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
