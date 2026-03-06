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
