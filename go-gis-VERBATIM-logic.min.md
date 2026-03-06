FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Utils\types\types.ts
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
export type translation = {
  lang: constants.languages;
  value: string;
};
export type tocGroupLegend = {
  id: string;
  name: string;
  active: boolean;
  legend: Array<tocElementLegend>;
};
export type tocElementLegend = {
  id: string;
  url: string;
  name: string;
  active: boolean;
  defaultVisibility: boolean;
};
export type staging = { map: OlMap; view: View; isLindek: boolean };
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
export type styleOptions = {
  url: string;
  mapUnits?: boolean;
  origin?: string;
  geometryFilter?: filterNoCQL;
};
export type mapOptionsBasic = {
  id: string;
  div: string;
  optionView: optionsView;
  interactions?: LayerInteractions;
};
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
export type CloseTool = ToolBase & {
  active: boolean;
  filterLocationName: string;
  dataPipes: Array<string>;
  dataValves: Array<string>;
};
export type UpDown = ToolBase & {
  active: boolean;
  dataPipes: Array<string>;
  dataWells: Array<string>;
  filterLocationName: string;
};
export type basicInteraction = {
  active: boolean;
  idInteraction: string;
};
export type dataTools = {
  type: constants.dataToolTypes;
  data: GeoJSON | string;
};
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
export type configOptions = {
  GOMapOptions: Array<GOMapOptions>;
};
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
export type sldStyleFunction = {
  styleFunction: any; // eslint-disable-line
  styleType: constants.GOGeometryType;
};
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
export type polygonSectorParams = {
  idMap: string;
  polygonSectorId: string;
  gisApiService: string;
  polygonSectorParams: polygonSectorDataParams;
};
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
export type opticalFiberParams = {
  idMap: string;
  idOpticalFiber: string;
  params: opticalFiberDataParams;
};
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
export type fiberParameters = {
  emisor_geometry: Array<number>;
  initial_offset: number;
  final_reading: number;
  step_distance: number;
  line_geometry: Array<Array<number>>;
  obstacles_data: Array<fiberObstacles>;
};
export type fiberObstacles = {
  element_tye: string;
  definition_type: string;
  value: Array<Array<number>>;
  offset_default: number;
};
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
export type filterNoCQL = { AND: boolean; filter: [filterGeometry] };
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
export interface clusterAnnotation extends Annotation, BaseClusterAnnotation {}
export interface clusterExtAnnotation
  extends ExtAnnotation,
    BaseClusterAnnotation {}
export interface ExtAnnotation extends Annotation {
  source: GeoJSON;
  layerId?: string;
  layerValue?: number;
  fieldId: string;
  fieldIdExt: string;
}
export type textAnnotation = {
  label?: string;
  offsetX?: number;
  offsetY?: number;
  fontColor?: string;
  fontSize?: string;
  stroke?: Stroke;
  textPlacement?: TextPlacement;
};
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
// FILE: src/goapi/Map/GoStagingMap.ts
declare global {
  interface Window {
    _langStorageListenerRegistered?: boolean;
  }
}
export class GOStagingMap {
  constructor(
    id: string,
    map: OlMap,
    view: GoView,
    mapInteractions: GoInteractions,
    mapOpts: GOMapOptions
  ) {
    this.id = id;
    this.map = map;
    this._mapOpts = mapOpts;
    this.view = view;
    this.layers = new Map<string, GOLayer>();
    this.filters = new Map<string, string>();
    this.mapInteractions = mapInteractions;
    this.layerInteractions = new Map<string, GoLayerInteractions>();
    this.setLanguageFirstRender();
  }
  public unmount() {}
  public get mapOpts() {}
  public getId(): string {}
  public getTheme() {}
  public addLayer(idLayer: string, layer: GOLayer | BaseLayer): void {}
  public removeLayer(idLayer: string): void {}
  public getView(): GoView {}
  public getMap(): OlMap {}
  public setOwnToc(ownToc: GoOwnToc): void {}
  public setTheme(theme: themes) {}
  public getOwnToc() {}
  public setMapLayerPreviewer(mapLayerPreviewer: MapLayerPreviewer): void {}
  public getMapLayerPreviewer() {}
  public getUIDisplay() {}
  public setUIDisplay(uiDisplay: UIDisplay): void {}
  public getLayer(idLayer: string): any {}
  public getLayers(): Map<string, GOLayer | BaseLayer> {}
  public getNumLayers(): number {}
  public addFilter(idLayer: string, filter: string, bbox: boolean): void {}
  public removeFilter(idLayer: string): void {}
  public setInteractions(interactionsOptions: LayerInteractions): void {}
  public getInteractions() {}
  public getMapImage(format: string, fileName?: string) {}
  public addOrUpdateTranslations(
    translation: Record<string, Record<SupportedLangs, string>>
  ): void {
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
    Object.keys(translation).forEach((key) => {
      const translationsByLanguage = translation[key];
      Object.keys(translationsByLanguage).forEach((lang) => {
        const langKey = lang as SupportedLangs;
        if (!this._mapOpts.translations?.[langKey]) {
          if (this._mapOpts.translations) {
            this._mapOpts.translations[langKey] = {};
          }
        }
        if (this._mapOpts.translations?.[langKey]) {
          this._mapOpts.translations[langKey][key] =
            translationsByLanguage[langKey];
        }
      });
    });
  }
  public getConfigTranslation(key: string): string {}
  public getI18nService(): II18nService {}
}
---
// FILE: src/goapi/UIMap/types.ts
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
// FILE: src/goapi/Interactions/Features/index.ts
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
// FILE: src/goapi/Interactions/functions/getFeaturesByLayer.ts
export { getFeaturesByLayer } from "../Features/interact/GetFeaturesByLayer";
---
// FILE: src/goapi/Services/GoStore/index.ts
export class GoStore {
  public static state: GoState = {};
  public static getState = (): GoState => {}
  public static dispatch = (payload): GoState => {}
}
---
// FILE: src/goapi/Utils/constants/index.ts
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
export enum GoLayerStyleType {
  VECTOR = "VECTOR",
  GEOJSON = "GEOJSON",
  CLUSTER = "CLUSTER",
}
export enum GoLayerSourceType {
  GEOSERVER = "GEOSERVER",
  GEOJSON = "GEOJSON",
}
export enum GoStyleVariant {
  JSON = "JSON",
  JSON_CONDITIONAL = "JSON_CONDITIONAL",
  SVG = "SVG",
  SVG_CONDITIONAL = "SVG_CONDITIONAL",
  WEBGL_JSON = "WEBGL_JSON",
  WEBGL_JSON_CONDITIONAL = "WEBGL_JSON_CONDITIONAL",
  WEBGL_SVG = "WEBGL_SVG",
  WEBGL_SVG_CONDITIONAL = "WEBGL_SVG_CONDITIONAL",
  SLD = "SLD",
}
export enum RegistedEnviroments {
  PRODUCTION = "PRODUCTION",
  DEVELOPMENT = "DEVELOPMENT",
  TEST = "TEST",
}
export enum RegistedApps {
  WORK_ORDERS = "WORK_ORDERS",
  GIS_API_DOCS = "GIS_API_DOCS",
}
export enum RegistedClients {
  HOUSTON = "HOUSTON",
  IDRICA = "IDRICA",
}
export enum HexConditions {
  AVG = "AVG",
  SUM = "SUM",
  COUNT = "COUNT",
}
export enum GOLayerEvents {
  LAYER_ADDED = "LAYER_ADDED",
  LAYER_ERROR = "LAYER_ERROR",
  LAYER_LOADED = "LAYER_LOADED",
  LAYER_LOADING = "LAYER_LOADING",
  LAYER_REMOVED = "LAYER_REMOVED",
  LAYER_CHANGED = "LAYER_CHANGED",
  LAYER_STYLE_CHANGED = "LAYER_STYLE_CHANGED",
}
export enum GOFeaturesEvents {
  FEATURES_LOAD_STARTED = "FEATURES_LOAD_STARTED",
  FEATURES_LOAD_ENDED = "FEATURES_LOAD_ENDED",
  FEATURES_LOAD_ERROR = "FEATURES_LOAD_ERROR",
}
export enum GoOwnTocEvents {
  TOC_ADDED = "TOC_ADDED",
  TOC_ERROR = "TOC_ERROR",
  TOC_LOADED = "TOC_LOADED",
  LAYER_CHANGE_VISIBILITY = "LAYER_CHANGE_VISIBILITY",
  TOC_LOADING = "TOC_LOADING",
}
export enum GOMapEvents {
  MAP_ERROR = "MAP_ERROR",
  MAP_LOADED = "MAP_LOADED",
  MAP_LOADING = "MAP_LOADING",
  BASE_LAYER_CHANGED = "BASE_LAYER_CHANGED",
  LAYER_VISIBILITY_CHANGED = "layer-visibility-changed",
}
export enum GoInteractionEvents {
  PRINT = "PRINT",
  PRINT_ERROR = "PRINT_ERROR",
  ENTER_FULLSCREEN = "ENTER_FULLSCREEN",
  EXIT_FULLSCREEN = "EXIT_FULLSCREEN",
}
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
export enum dataToolTypes {
  JSON = "JSON",
  LAYER = "LAYER",
}
export enum GOGeometryType {
  POINT = "POINT",
  LINE_STRING = "LINE_STRING",
  POLYGON = "POLYGON",
  ALL = "ALL",
  NOTGEOM = "NOT_GEOM",
}
export enum GoTextType {
  LITERAL = "LITERAL",
  FIELDBASED = "FIELDBASED",
}
export enum GOLayerSet {
  ALL,
  VISIBLE,
  NO_VISIBLE,
  FILTERLAYERS,
  NOFILTERLAYERS,
}
export enum UpDownType {
  upstream = "upstream",
  downstream = "downstream",
}
export enum ExtentFactor {
  NULL = 0.0,
  FACTOR1 = 100000,
  FACTOR2 = 50000,
  FACTOR3 = 25000,
  FACTOR4 = 10000,
  FACTOR5 = 5000,
}
export enum GOOverloadFilterOption {
  OVERLOAD = "OVERLOAD",
  ANDFILTER = "AND",
  ORFILTER = "OR",
}
export enum GOSearchScope {
  VISIBLE_EXTENT,
  COMPLETE_EXTENT,
}
export enum GOTypeSelect {
  SELECT,
  DRAGBOX,
}
export enum GOGeocoderProvider {
  OSM = "OSM",
  IGN = "IGN",
}
export enum GOGeocoderServiceType {
  CANDIDATES = "CANDIDATES",
  FIND = "FIND",
}
export enum GOStyleOrigin {
  GEOSERVER = "GEOSERVER",
  OTHER = "OTHER",
}
export const GODateTodayLoader = "#TODAY#";
export const GO = "#TODAY#";
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
export enum measureGeometry {
  lineString = "LINESTRING",
  polygon = "POLYGON",
}
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
export enum typeOnOverlay {
  "change" = "change",
  "change:element" = "change:element",
  "change:map" = "change:map",
  "change:offset" = "change:offset",
  "change:position" = "change:position",
  "change:positioning" = "change:positioning",
  "propertychange" = "propertychange",
}
export enum densityToolType {
  HEATMAP = "HEATMAP",
  HEXGRID = "HEXGRID",
  ISOLINE = "ISOLINE",
  ISOBAND = "ISOBAND",
}
export enum languages {
  ES = "ES",
  EN = "EN",
}
export enum themes {
  DARK = "DARK",
  LIGHT = "LIGHT",
}
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
export enum VRSRSUIEvents {
  RANGE_SLIDER_VALUES_CHANGED = "RANGE_SLIDER_VALUES_CHANGED",
}
export enum pageSize {
  A5 = "a5",
  A4 = "a4",
  A3 = "a3",
  A2 = "a2",
  A1 = "a1",
  A0 = "a0",
}
export enum pageDimensionA5 {
  X = 148,
  Y = 210,
}
export enum pageDimensionA4 {
  X = 210,
  Y = 297,
}
export enum pageDimensionA3 {
  X = 297,
  Y = 420,
}
export enum pageDimensionA2 {
  X = 420,
  Y = 594,
}
export enum pageDimensionA1 {
  X = 594,
  Y = 841,
}
export enum pageDimensionA0 {
  X = 841,
  Y = 1189,
}
export enum IconMode {
  SELECTED = "selected",
  INACTIVE = "inactive",
  WARNING_YELLOW = "warning-yellow",
  WARNING_ORANGE = "warning-orange",
  ERROR = "error",
}
export enum IconShape {
  MARKER = "marker",
  PIN = "pin",
  CLUSTER = "cluster",
  BADGE = "badge",
  STACK = "stack",
}
export enum symbolType {
  IMAGE = "image",
  TRIANGLE = "triangle",
  CIRCLE = "circle",
  SQUARE = "square",
}
export enum webGLConditionType {
  CASE = "case",
  MATCH = "match",
}
export enum unitSystem {
  METRIC = "metric",
  IMPERIAL = "imperial",
}
export default {};
---
// FILE: src/goapi/Layers/common/terrainColors.ts
export const TERRAIN_RANGE_COLORS = {
  main: {
    high: "rgb(163, 1, 1)",
    medium: "rgb(136, 68, 68)",
    low: "rgb(75, 75, 75)",
    default: "rgb(128, 128, 128)",
  },
} as const;
---
// FILE: src/goapi/Interactions/geoTools/buffer/index.ts
export class GoBuffer {
  public style: Style | styleFromUser | undefined;
  public lookingForDataNames: Array<string>;
  public pipeLayerName: string;
  public stylePoint: ISVGIcon;
  public static readonly fill = new Fill( {}
  public static readonly stroke = new Stroke( {}
  public static readonly defaultStyle = new Style( {}
  public static is_json(str) {}
  constructor() {}
  public get serviceBuffer() {}
  public get paramsService() {}
  public get dataBuffer() {}
  public get distance() {}
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
  public getLayerBufferData(): VectorLayer<Feature<Geometry>> {}
  public get bufferLayer(): VectorLayer<Feature<Geometry>> {}
  public getLayerParams() {}
}
---
// FILE: src/goapi/Interactions/index.ts
export * from "./utils"; // Don't remove this line from this position
export * as Analysis from "./Analysis";
export * as ExternalsConnections from "./ExternalsConnections";
export * as Features from "./Features";
export * as Hydraulics from "./Hydraulics";
export * as Selection from "./Selection";
export * as Views from "./Views";
export * as Utils from "./utils";
export { GoInteractions };
class GoInteractions extends GOInteractionTool {
    string,
    Map<string, GoCloseTools>
  >();
    string,
    Map<string, GoDensityTool>
  >();
    string,
    Map<string, GoDrawSelectByPolygon>
  > = new Map<string, Map<string, GoDrawSelectByPolygon>>();
    string,
    Map<string, GoDrawSelectByPolygon>
  > = new Map();
    string,
    Map<string, GoEditFeatures>
  >();
    string,
    Map<string, GoflowLinesMove>
  >();
    string,
    Map<string, GoFullScreen>
  >();
    string,
    Map<string, string>
  >();
    string,
    Map<string, GoHighlight>
  >();
    string,
    Map<string, GoHover>
  >();
    string,
    Map<string, any> //eslint-disable-line
  >();
    string,
    Map<string, GoIntersect>
  >();
    string,
    Map<string, GoMeasure>
  >();
    string,
    Map<string, GoOverlay>
  >();
    string,
    Map<string, GoPrint>
  >();
    string,
    Map<string, GoRotation>
  >();
    string,
    Map<string, GoRouting>
  >();
    string,
    Map<string, GoSectorColor>
  >();
    string,
    Map<string, GoPolygonSector>
  >();
    string,
    Map<string, GoWatershed>
  >();
    string,
    Map<string, GoOpticalFiber>
  >();
    string,
    Map<string, GOInterpolation>
  >();
    string,
    Map<string, GOIsolines>
  >();
    new Map<string, Map<string, GoSelectByPolygon>>();
  public stagingMap: Map<string, GOStagingMap>;
    string,
    Map<string, GoHoverFeature>
  >();
    string,
    Map<string, GoUpDown>
  >();
    new Map<string, Map<string, GoWMSGetFeatureInfo>>();
  public buffer: Map<string, Map<string, GoBuffer>> = new Map<
    string,
    Map<string, GoBuffer>
  >();
  public closeToolData: types.CloseTool = {}
  public intersectData: types.dataTools;
  public layerHover: VectorLayer<VectorSource> | undefined;
  public nearData: Map<string, Map<string, GoNearData>> = new Map<
    string,
    Map<string, GoNearData>
  >();
  public nearDataAttributes: Map<string, Map<string, GoNearDataAttributes>> =
    new Map<string, Map<string, GoNearDataAttributes>>();
  public upDownToolData: types.UpDown = {}
  public thematicMap: Map<string, Map<string, GoThematicMap>> = new Map<
    string,
    Map<string, GoThematicMap>
  >();
  constructor(stagingMap: Map<string, GOStagingMap>) {}
  public getIFCRegistry(): Map<string, Map<string, GoIFCComponents>> {}
  public unmount(): void {}
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
  public createStyle(style: types.styleFromUser): Style {}
    idMap: string,
    errorMethod: string,
    interaction: Map<string, any>
  ): boolean {
    if (!interaction.has(idMap)) {
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
    idMap: string,
    errorMethod: string,
    idInteraction: string,
    interaction: Map<string, any>
  ): boolean {
    if (!interaction.get(idMap).has(idInteraction)) {
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
  public getOlMap(idMap: string): OlMap | undefined {}
}
interface GoInteractions {
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
  getFeaturesByLayer(
    params: types.GetFeaturesByLayerParams
  ): Promise<types.GeoJSONFeatureCollection>;
  getAllFeaturesProperties({
    idMap,
    layerList,
  }: {
    idMap: string;
    layerList: types.allFeaturesProperties;
  }): Array<unknown>;
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
  addCustomZoom({ idMap }: { idMap: string }): HTMLElement | undefined;
  removeCustomZoom: ({ idMap }: { idMap: string }) => void;
  removeAllCustomZoom: ({ idMap }: { idMap: string }) => void;
  addCtrlScrollZoom({ idMap }: { idMap: string }): void;
  hasCtrlScrollZoom({ idMap }: { idMap: string }): boolean;
  removeCtrlScrollZoom({ idMap }: { idMap: string }): void;
  disableDoubleClickZoom({ idMap }: { idMap: string }): void;
  enableDoubleClickZoom({ idMap }: { idMap: string }): void;
  hasDisabledDoubleClickZoom({ idMap }: { idMap: string }): DoubleClickZoom;
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
  createDMD({ dmdId }: { dmdId: string }): GoDMD;
  getDMD({ dmdId }: { dmdId: string }): GoDMD;
  getDMDs(): Array<{ name: string; value: GoDMD }>;
  hasDMD({ dmdId }: { dmdId: string }): boolean;
  removeDMD({ dmdId }: { dmdId: string }): void;
  getMultiExtent({
    idMap,
    idLayers,
  }: {
    idMap: string;
    idLayers?: Array<string>;
  }): null | [number, number, number, number];
  on(type: string, callback: (...rest: unknown[]) => unknown): void;
  off(type: string, callback: (...rest: unknown[]) => unknown): void;
  once(type: string, callback: (...rest: unknown[]) => unknown): void;
  hasEvent(type: string): boolean;
  getEventHandlers(type: string): Array<(...rest: unknown[]) => unknown>;
  emit(type: string, ...rest: unknown[]): void;
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
GoInteractions.prototype.registerGeocodingHERE =
  functions.geocoding.registerGeocodingHERE;
GoInteractions.prototype.getDirectionFromCoordinates =
  functions.geocoding.getDirectionFromCoordinates;
GoInteractions.prototype.getDirectionFromCoordinatesProperties =
  functions.geocoding.getDirectionFromCoordinatesProperties;
GoInteractions.prototype.getDirectionFromCoordinatesHERE =
  functions.geocoding.getDirectionFromCoordinatesHERE;
GoInteractions.prototype.getCoordinatesFromDirection =
  functions.geocoding.getCoordinatesFromDirection;
GoInteractions.prototype.getCoordinatesFromDirectionProperties =
  functions.geocoding.getCoordinatesFromDirectionProperties;
GoInteractions.prototype.getCoordinatesFromDirectionHERE =
  functions.geocoding.getCoordinatesFromDirectionHERE;
GoInteractions.prototype.suggestAddress = functions.geocoding.suggestAddress;
GoInteractions.prototype.autoSuggestAddress =
  functions.geocoding.autoSuggestAddress;
GoInteractions.prototype.getAllsensorsFromNexus =
  functions.nexusInfo.getAllsensorsFromNexus;
GoInteractions.prototype.getHistoricInfoFromNexus =
  functions.nexusInfo.getHistoricInfoFromNexus;
GoInteractions.prototype.addOverlay = functions.overlay.addOverlay;
GoInteractions.prototype.hasOverlay = functions.overlay.hasOverlay;
GoInteractions.prototype.removeOverlay = functions.overlay.removeOverlay;
GoInteractions.prototype.getFeaturesByLayer =
  functions.getFeaturesByLayer.getFeaturesByLayer;
GoInteractions.prototype.getAllFeaturesProperties =
  functions.getAllFeaturesProperties.getAllFeaturesProperties;
GoInteractions.prototype.addHighlight = functions.highlight.addHighlight;
GoInteractions.prototype.hasHighlight = functions.highlight.hasHighlight;
GoInteractions.prototype.getHighlight = functions.highlight.getHighlight;
GoInteractions.prototype.removeHighlight = functions.highlight.removeHighlight;
GoInteractions.prototype.removeHighlightIfExist =
  functions.highlight.removeHighlightIfExist;
GoInteractions.prototype.changeHighlightZIndex =
  functions.highlight.changeHighlightZIndex;
GoInteractions.prototype.changeStyleOfHighlight =
  functions.highlight.changeStyleOfHighlight;
GoInteractions.prototype.restoreHighlightStyles =
  functions.highlight.restoreHighlightStyles;
GoInteractions.prototype.addWMSGetFeatureInfo =
  functions.wmsFeatureInfo.addWMSGetFeatureInfo;
GoInteractions.prototype.hasWMSGetFeatureInfo =
  functions.wmsFeatureInfo.hasWMSGetFeatureInfo;
GoInteractions.prototype.getWMSGetFeatureInfo =
  functions.wmsFeatureInfo.getWMSGetFeatureInfo;
GoInteractions.prototype.removeWMSGetFeatureInfo =
  functions.wmsFeatureInfo.removeWMSGetFeatureInfo;
GoInteractions.prototype.hasHoverFeature =
  functions.hoverFeature.hasHoverFeature;
GoInteractions.prototype.getLayerHover = functions.hoverFeature.getLayerHover;
GoInteractions.prototype.mouseHover = functions.hoverFeature.mouseHover;
GoInteractions.prototype.removeHover = functions.hoverFeature.removeHover;
GoInteractions.prototype.hasGetFeature = functions.hoverFeature.hasGetFeature;
GoInteractions.prototype.addTooltipHoverFeature =
  functions.hoverFeature.addTooltipHoverFeature;
GoInteractions.prototype.hasToolTipHover =
  functions.hoverFeature.hasToolTipHover;
GoInteractions.prototype.getTooltipHoverFeature =
  functions.hoverFeature.getTooltipHoverFeature;
GoInteractions.prototype.removeTooltipHoverFeature =
  functions.hoverFeature.removeTooltipHoverFeature;
GoInteractions.prototype.changeHoverZIndex =
  functions.hoverFeature.changeHoverZIndex;
GoInteractions.prototype.addCloseTool = functions.closeTool.addCloseTool;
GoInteractions.prototype.clearCloseTool = functions.closeTool.clearCloseTool;
GoInteractions.prototype.addExtraDataCloseToolInteractions =
  functions.closeTool.addExtraDataCloseToolInteractions;
GoInteractions.prototype.addExtraDataUpDownStreamInteractions =
  functions.closeTool.addExtraDataUpDownStreamInteractions;
GoInteractions.prototype.hasCloseTool = functions.closeTool.hasCloseTool;
GoInteractions.prototype.getCloseTool = functions.closeTool.getCloseTool;
GoInteractions.prototype.removeCloseTool = functions.closeTool.removeCloseTool;
GoInteractions.prototype.addUpDownStream =
  functions.updownstream.addUpDownStream;
GoInteractions.prototype.hasUpDownStream =
  functions.updownstream.hasUpDownStream;
GoInteractions.prototype.removeUpDownStream =
  functions.updownstream.removeUpDownStream;
GoInteractions.prototype.clearUpDownStream =
  functions.updownstream.clearUpDownStream;
GoInteractions.prototype.addIntersect = functions.intersect.addIntersect;
GoInteractions.prototype.hasIntersect = functions.intersect.hasIntersect;
GoInteractions.prototype.clearIntersect = functions.intersect.clearIntersect;
GoInteractions.prototype.removeIntersect = functions.intersect.removeIntersect;
GoInteractions.prototype.refreshGeoTools = functions.geoTools.refreshGeoTools;
GoInteractions.prototype.addDrawSelectByPolygon =
  functions.selectByPoligon.addDrawSelectByPolygon;
GoInteractions.prototype.hasDrawSelectByPolygon =
  functions.selectByPoligon.hasDrawSelectByPolygon;
GoInteractions.prototype.setActiveDrawSelectByPolygon =
  functions.selectByPoligon.setActiveDrawSelectByPolygon;
GoInteractions.prototype.removeDrawSelectByPolygon =
  functions.selectByPoligon.removeDrawSelectByPolygon;
GoInteractions.prototype.addSelectByPolygon =
  functions.selectByPoligon.addSelectByPolygon;
GoInteractions.prototype.hasSelectByPolygon =
  functions.selectByPoligon.hasSelectByPolygon;
GoInteractions.prototype.removeSelectByPolygon =
  functions.selectByPoligon.removeSelectByPolygon;
GoInteractions.prototype.addDrawSelectByPolygonInCluster =
  functions.selectByPoligon.addDrawSelectByPolygonInCluster;
GoInteractions.prototype.hasDrawSelectByPolygonInCluster =
  functions.selectByPoligon.hasDrawSelectByPolygonInCluster;
GoInteractions.prototype.setActiveDrawSelectByPolygonInCluster =
  functions.selectByPoligon.setActiveDrawSelectByPolygonInCluster;
GoInteractions.prototype.removeDrawSelectByPolygonInCluster =
  functions.selectByPoligon.removeDrawSelectByPolygonInCluster;
GoInteractions.prototype.addSelectByPolygonInCluster =
  functions.selectByPoligon.addSelectByPolygonInCluster;
GoInteractions.prototype.hasSelectByPolygonInCluster =
  functions.selectByPoligon.hasSelectByPolygonInCluster;
GoInteractions.prototype.removeSelectByPolygonInCluster =
  functions.selectByPoligon.removeSelectByPolygonInCluster;
GoInteractions.prototype.hasRotation = functions.rotation.hasRotation;
GoInteractions.prototype.removeRotation = functions.rotation.removeRotation;
GoInteractions.prototype.addRotation = functions.rotation.addRotation;
GoInteractions.prototype.disableRotation = functions.rotation.disableRotation;
GoInteractions.prototype.getRotation = functions.rotation.getRotation;
GoInteractions.prototype.addMouseRotation = functions.rotation.addMouseRotation;
GoInteractions.prototype.addCustomRotateCompass =
  functions.rotation.addCustomRotateCompass;
GoInteractions.prototype.hasPrint = functions.print.hasPrint;
GoInteractions.prototype.removePrint = functions.print.removePrint;
GoInteractions.prototype.addPrint = functions.print.addPrint;
GoInteractions.prototype.getPrint = functions.print.getPrint;
GoInteractions.prototype.addMeasure = functions.measure.addMeasure;
GoInteractions.prototype.hasMeasure = functions.measure.hasMeasure;
GoInteractions.prototype.setActiveMeasure = functions.measure.setActiveMeasure;
GoInteractions.prototype.clearMeasure = functions.measure.clearMeasure;
GoInteractions.prototype.removeMeasure = functions.measure.removeMeasure;
GoInteractions.prototype.addFlowLines = functions.flowLines.addFlowLines;
GoInteractions.prototype.hasFlowLines = functions.flowLines.hasFlowLines;
GoInteractions.prototype.removeFlowLines = functions.flowLines.removeFlowLines;
GoInteractions.prototype.addDensityLayer =
  functions.densityLayer.addDensityLayer;
GoInteractions.prototype.hasDensityLayer =
  functions.densityLayer.hasDensityLayer;
GoInteractions.prototype.getDensityLayer =
  functions.densityLayer.getDensityLayer;
GoInteractions.prototype.removeDensityLayer =
  functions.densityLayer.removeDensityLayer;
GoInteractions.prototype.hasEditFeature = functions.feature.hasEditFeature;
GoInteractions.prototype.createFeature = functions.feature.createFeature;
GoInteractions.prototype.removeEditFeature =
  functions.feature.removeEditFeature;
GoInteractions.prototype.addEditFeature = functions.feature.addEditFeature;
GoInteractions.prototype.getEditFeatures = functions.feature.getEditFeatures;
GoInteractions.prototype.saveEditFeatures = functions.feature.saveEditFeatures;
GoInteractions.prototype.deleteFeatures = functions.feature.deleteFeatures;
GoInteractions.prototype.addCustomZoom = functions.customZoom.addCustomZoom;
GoInteractions.prototype.removeCustomZoom =
  functions.customZoom.removeCustomZoom;
GoInteractions.prototype.removeAllCustomZoom =
  functions.customZoom.removeAllCustomZoom;
GoInteractions.prototype.addCtrlScrollZoom =
  functions.customZoom.addCtrlScrollZoom;
GoInteractions.prototype.hasCtrlScrollZoom =
  functions.customZoom.hasCtrlScrollZoom;
GoInteractions.prototype.removeCtrlScrollZoom =
  functions.customZoom.removeCtrlScrollZoom;
GoInteractions.prototype.disableDoubleClickZoom =
  functions.customZoom.disableDoubleClickZoom;
GoInteractions.prototype.enableDoubleClickZoom =
  functions.customZoom.enableDoubleClickZoom;
GoInteractions.prototype.hasDisabledDoubleClickZoom =
  functions.customZoom.hasDisabledDoubleClickZoom;
GoInteractions.prototype.createIFC = functions.ifc.createIFC;
GoInteractions.prototype.hasIFCModel = functions.ifc.hasIFCModel;
GoInteractions.prototype.removeIFCModel = functions.ifc.removeIFCModel;
GoInteractions.prototype.createDMD = functions.dmd.createDMD;
GoInteractions.prototype.getDMD = functions.dmd.getDMD;
GoInteractions.prototype.getDMDs = functions.dmd.getDMDs;
GoInteractions.prototype.hasDMD = functions.dmd.hasDMD;
GoInteractions.prototype.removeDMD = functions.dmd.removeDMD;
GoInteractions.prototype.createCadastre = functions.cadastre.createCadastre;
GoInteractions.prototype.getCadastre = functions.cadastre.getCadastre;
GoInteractions.prototype.getCadastres = functions.cadastre.getCadastres;
GoInteractions.prototype.hasCadastre = functions.cadastre.hasCadastre;
GoInteractions.prototype.removeCadastre = functions.cadastre.removeCadastre;
GoInteractions.prototype.addThematicMap = functions.thematicMap.addThematicMap;
GoInteractions.prototype.hasThematicMap = functions.thematicMap.hasThematicMap;
GoInteractions.prototype.getThematicMap = functions.thematicMap.getThematicMap;
GoInteractions.prototype.removeThematicMap =
  functions.thematicMap.removeThematicMap;
GoInteractions.prototype.addNearData = functions.nearData.addNearData;
GoInteractions.prototype.hasNearData = functions.nearData.hasNearData;
GoInteractions.prototype.removeNearData = functions.nearData.removeNearData;
GoInteractions.prototype.addNearDataAttributes =
  functions.nearDataAttributes.addNearDataAttributes;
GoInteractions.prototype.hasNearDataAttributes =
  functions.nearDataAttributes.hasNearDataAttributes;
GoInteractions.prototype.removeNearDataAttributes =
  functions.nearDataAttributes.removeNearDataAttributes;
GoInteractions.prototype.addBuffer = functions.buffer.addBuffer;
GoInteractions.prototype.hasBuffer = functions.buffer.hasBuffer;
GoInteractions.prototype.removeBuffer = functions.buffer.removeBuffer;
GoInteractions.prototype.addSectorColor = functions.sectorColor.addSectorColor;
GoInteractions.prototype.hasSectorColor = functions.sectorColor.hasSectorColor;
GoInteractions.prototype.removeSectorColor =
  functions.sectorColor.removeSectorColor;
GoInteractions.prototype.addPolygonSector =
  functions.polygonSector.addPolygonSector;
GoInteractions.prototype.hasPolygonSector =
  functions.polygonSector.hasPolygonSector;
GoInteractions.prototype.removePolygonSector =
  functions.polygonSector.removePolygonSector;
GoInteractions.prototype.addOpticalFiber =
  functions.opticalFiber.addOpticalFiber;
GoInteractions.prototype.hasOpticalFiber =
  functions.opticalFiber.hasOpticalFiber;
GoInteractions.prototype.removeOpticalFiber =
  functions.opticalFiber.removeOpticalFiber;
GoInteractions.prototype.addInterpolation =
  functions.interpolation.addInterpolation;
GoInteractions.prototype.hasInterpolation =
  functions.interpolation.hasInterpolation;
GoInteractions.prototype.removeInterpolation =
  functions.interpolation.removeInterpolation;
GoInteractions.prototype.addIsolines = functions.isolines.addIsolines;
GoInteractions.prototype.hasIsolines = functions.isolines.hasIsolines;
GoInteractions.prototype.removeIsolines = functions.isolines.removeIsolines;
GoInteractions.prototype.addWatershed = functions.watershed.addWatershed;
GoInteractions.prototype.hasWatershed = functions.watershed.hasWatershed;
GoInteractions.prototype.removeWatershed = functions.watershed.removeWatershed;
GoInteractions.prototype.getMultiExtent = functions.selection.getMultiExtent;
GoInteractions.prototype.on = functions.events.on;
GoInteractions.prototype.off = functions.events.off;
GoInteractions.prototype.once = functions.events.once;
GoInteractions.prototype.hasEvent = functions.events.hasEvent;
GoInteractions.prototype.getEventHandlers = functions.events.getEventHandlers;
GoInteractions.prototype.emit = functions.events.emit;
GoInteractions.prototype.hasFullScreen = functions.fullScreen.hasFullScreen;
GoInteractions.prototype.removeFullScreen =
  functions.fullScreen.removeFullScreen;
GoInteractions.prototype.addFullScreen = functions.fullScreen.addFullScreen;
GoInteractions.prototype.getFullScreen = functions.fullScreen.getFullScreen;
GoInteractions.prototype.onFullScreenEnter =
  functions.fullScreen.onFullScreenEnter;
GoInteractions.prototype.onFullScreenExit =
  functions.fullScreen.onFullScreenExit;
GoInteractions.prototype.addRoutingHERE = functions.routingHERE.addRoutingHERE;
GoInteractions.prototype.addMultiRoutingHERE =
  functions.routingHERE.addMultiRoutingHERE;
---
// FILE: src/goapi/Toc/GoOwnToc.ts
const svgOpenedEye =
  '<svg class="icon-eye" viewBox="0 0 24 24" aria-hidden="true" focusable="false">' +
  '<path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z" stroke="currentColor" fill="none"/>' +
  '<circle cx="12" cy="12" r="3" stroke="currentColor" fill="none"/>' +
  "</svg>";
const svgClosedEye =
  '<svg class="icon-eye-off" viewBox="0 0 24 24" aria-hidden="true" focusable="false">' +
  '<path d="M2 4.27L3.28 3 21 20.72 19.73 22l-2.2-2.2A10.94 10.94 0 0 1 12 21C7 21 2.73 17.39 1 13.5a11.6 11.6 0 0 1 3.11-4.26L2 4.27zM12 6c3.31 0 6.31 1.66 8.19 4.5a10.9 10.9 0 0 1-2.34 3.13L9.41 6.19A9.6 9.6 0 0 1 12 6z" stroke="currentColor" fill="none"/>' +
  "</svg>";
export default class GoOwnToc {
  public stageToc: types.ownToc;
  public customLegend: GoCustomLegend;
  constructor(id: string, goMap: GoMap) {}
  public unmount(): void {}
  public removeFolderFromToc(stageToc: types.ownToc): boolean {}
  public createStageToc(config: any): void {}
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
    ownToc_container: HTMLDivElement,
    stageToc: types.ownToc
  ): void {
    ownToc_container.style.maxHeight = stageToc.maxHeight + "px";
  }
  public orderStageTocBySubFolder(): void {}
  public setVisibility(
    idLayer: string,
    visibility: boolean,
    reload?: boolean
  ): void {
    this.stageToc.layers.forEach((groupLayer) => {
      groupLayer.layer.forEach((lay) => {
        if (lay.id == idLayer && lay.visible == visibility) {
          return;
        } else if (lay.id == idLayer && lay.visible !== visibility) {
          lay.visible = visibility;
        }
      });
    });
    if (reload == null || reload) {
      this.createTocStructure();
    }
    MapEventManager.events.fire(GoOwnTocEvents.LAYER_CHANGE_VISIBILITY, {
      id: idLayer,
      type: "LAYER",
      mapId: this.stageToc.mapId,
      state: this.getEventTOCState(this.stageToc),
    });
  }
  public reloadFolderVisibility() {}
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
  public getLanguage(): string {}
  public setLanguage(idLanguage: string): void {}
  public getTheme(): constants.themes {}
  public setTheme(theme: constants.themes): void {}
  public removeLayer(idLayer: string): void {}
  public removeLayerLegend(idLayer: string): void {}
  public removeLayers(idLayers: Array<string>): void {}
  public removeLayersLegend(idLayers: Array<string>): void {}
  public addLayer(idLayer: string): void {}
  public addLayerLegend(idLayer: string): void {}
  public addLayers(idLayers: Array<string>): void {}
  public addNewLayer(layerConfig: types.layerOptions): void {}
  public addLayersLegend(idLayers: Array<string>): void {}
  public reload(config): void {}
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
// FILE: src/goapi/Interactions/Features/interact/GetFeaturesByLayer.ts
 FILE NOT FOUND  This file does not exist at the specified path. Both Features/index.ts and Interactions/functions/getFeaturesByLayer.ts reference this non-existent file.
---
