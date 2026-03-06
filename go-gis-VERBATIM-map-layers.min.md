FILE: c:\Users\serazca\Desktop\BitBucket\go-gis\src\goapi\Map\GoMap.ts
export class GoMap {
  public interactions: GoInteractions;
  constructor() {}
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
  public unmountMap(mapId: string) {}
  public addInteractions(id: string, interactions: types.interactions): void {}
  public createProjection(options: types.projOptions): void {}
  public getInteractions() {}
  public getProjection(idMap: string) {}
  public getView(idMap: string) {}
  public getCenter(idMap: string): olCoordinate.Coordinate | undefined {}
  public setCenter(idMap: string, center: [number, number]): void {}
  public getZoom(idMap: string): number | undefined {}
  public setZoom(idMap: string, zoom: number): void {}
  public goToExtent(idMap: string, extent: Extent) {}
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
  public getMap(idMap: string): OlMap | undefined {}
  public getAllStaging(): Map<string, GOStagingMap> {}
  public getStaging(idMap: string): GOStagingMap | undefined {}
  public setLanguage(lang: types.SupportedLangs): void {}
  public getTheme(): themes {}
  public setThemeAllMaps(theme: themes): void {}
  public getLanguage(): types.SupportedLangs {}
  public refreshMap(idMap: string): void {}
  public getLayers(idMap: string) {}
  public addLayer(idMap: string, layerOpts: types.layerOptions) {}
  public getState() {}
  public removeLayer(idMap: string, idLayer: string): void {}
  public refreshLayer(idMap: string, idLayer?: Array<string>): void {}
  public injectDataGeoJson(
    idMap: string,
    idLayer: string,
    clear: boolean,
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
  public existLayer(idMap: string, idLayer: string) {}
  public onMap(idMap: string, type: typeOn, callback: any) {}
  public bindViews(refView: string, viewsToBind?: Array<string>): void {}
  public unbindAllViews(option: any): void {}
    map: OlMap,
    centerTo: [number, number],
    duration?: number,
    zoom?: number
  ): void {
    const zoomTo = zoom || map.getView().getZoom();
    const durationTime = duration || 2000;
    let parts = 2;
    let called = false;
    const done = (complete: any) => {};
    function callback(complete: any) {}
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
  public addNewLayer(idMap: string, layerConfig: types.layerOptions) {}
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
  public layersLoader(layer: GOLayer) {}
  public on(eventName: string, callback): void {}
  public once(eventName: string, callback): void {}
  public off(eventName?: string, callback?): void {}
  public hasEvent(eventName: string): boolean {}
  public getEventHandlers(eventName: string) {}
  public emit(eventName: string, data): void {}
}
---
// FILE: src/goapi/Map/GoMapLoader.ts
export interface MapConfigLoader {
  mapConfig: types.configOptions;
  enviroment: string;
  userToken?: string;
  useBackendFeatures?: boolean;
  isExample?: boolean;
}
export class GoMapLoader {
  public static linkArray: Array<string> = [];
  public static dataBack: Array<any> = []; // eslint-disable-line
  public static defaultMapConfig: MapConfigLoader = {}
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
      GoStore.dispatch(mapState);
      return GoMapLoader.__loadGoMap(mapConfig, geoserverAuthData.url);
    }
  }
    enviroment: string,
    userToken: string
  ): Promise<any> {
    GoStore.dispatch({ token: userToken });
    return await GoAuth.getGeoServerAuth(enviroment);
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
        this.sendDataBack(mapOpts, goMap);
      }
      MapEventManager.events.fire(GOMapEvents.MAP_LOADED, {
        mapId: mapOpts.id,
      });
    }
    return goMap;
  }
  public static AddRegionEnv(env) {}
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
  public static checkEnviroment(geoserver: string): string {}
  public static checkUrlLayer(layer: string): string {}
  public static containsHttp(url: string): boolean {}
}
---
// FILE: src/goapi/Layers/GoLayer/index.ts
export abstract class GOLayer {
  public cqlFilterIsCanceled = false;
  public layerProjection: string | undefined;
  public olMap: OlMap;
  public useBboxFilter = false;
  constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {}
  public unmount(): void {}
  public getLayerUrl() {}
  public validateLayerConfig(layerOpts: types.layerOptions): void {}
  public get goMap(): GOStagingMap {}
  public abstract getLayer();
  public abstract getVisibility(): boolean;
  public abstract canFilter(): boolean;
  public isBaseLayer(): boolean {}
  public changeLayerOpacity(opacity: number) {}
  public onLayerLoaded(): Promise<unknown> {}
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
  public on(eventName: string, callback): void {}
  public once(eventName: string, callback): void {}
  public off(eventName?: string, callback?): void {}
  public emit(eventName: string, ...args): void {}
  public hasEvent(eventName: string): boolean {}
  public getEventHandlers(eventName: string) {}
  public onPropChange(obj, prop: string, callback): void {}
  public async applyConfigToLayer(layerOpts: layerOptions, bbox) {}
  public type(): string {}
  public hasFilter(): boolean {}
  public addFilter(filter: string, bbox = false) {}
  public formatFilter(filter: string, bbox?: boolean): string {}
  public removeFilter(): void {}
  public trackRequest<T>(
    request: CancelablePromise<T>
  ): CancelablePromise<T> {
    this.activeRequests.add(request as CancelablePromise<unknown>);
    request.finally(() => {
      this.activeRequests.delete(request as CancelablePromise<unknown>);
    });
    return request;
  }
  public cancelActiveRequests(): void {}
  public consumeCancellationFlag(): boolean {}
  public getOlMap(): OlMap {}
  public getLayerOpts(): layerOptions {}
  public getAlias(): string {}
  public getId(): string {}
  public getLayerName(): string {}
  public getUrl(): string {}
  public getLayerType(): string {}
  public getZIndex(): number {}
  public getMaxResolution(): number {}
  public getMinResolution(): number {}
  public async getFeaturesByAttribute(
    field: string,
    condition: GoFilterCondition,
    value: string
  ) {
    let selectedFeatures: Array<Feature<Geometry>> = [];
    const layerOpts = this.getLayerOpts();
    let searchByCqlFilter = false;
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
  public getExtent(): Array<number> | null {}
  public getOptions(): layerOptions {}
  public getLayerStyle(): GoStyle {}
  public getLayerProjection(): string {}
  public setVisibility(visibility: boolean): void {}
  public setZIndex(zIndex: number): void {}
  public setMaxResolution(maxResolution: number): void {}
  public setMinResolution(minResolution: number): void {}
  public setLayerStyle(style?: Array<GoGlobalStyle> | GoGlobalStyle) {}
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
          function (feature) {}
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
// FILE: src/goapi/Layers/GoVectorLayer/index.ts
export class GOVectorLayer extends GOStyledLayer {
  public layerProjection: string | undefined;
  public toProjection: string | undefined;
  public useBboxFilter: boolean;
  constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {}
  public isLoadedData(timeInterval = 1000, limit = 0) {}
  public refresh(): void {}
  public changeStyle(style: types.styleFromUser): void {}
  public unmount(): void {}
  public restoreLayer(): void {}
  public hasFilter(): boolean {}
  public canFilter(): boolean {}
  public type(): string {}
  public exportGeoJsonFormatFeatures() {}
  public isGeometryType(type: string): boolean {}
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
  public async loader(extent: Extent): Promise<any> {}
  public selectChboxToc(options): void {}
  public layerRenderListener(event): void {}
  public get(): void {}
  public getLayer(): VectorLayer<VectorSource> {}
  public getLayerName(): string {}
  public async getGeometryName(): Promise<string> {}
  public getLayerProjection(): string {}
  public getUrl(): string {}
  public getVisibility(): boolean {}
  public getFilter(): string {}
  public async getLegend(layerOptsLegend?: string): Promise<string | null> {}
  public getGeometryType(layerGeomType): GOGeometryType {}
  public getFeatureType(): string {}
  public getLayerDescription(): CancelablePromise<string> {}
  public setIcons() {}
  public setDefaultAnnotations(): void {}
}
---
// FILE: src/goapi/Layers/GoClusterLayer/index.ts
export class GOClusterLayer extends GOStyledLayer {
  public clusterCounter: number;
  public extAnnotation: Map<string, types.clusterExtAnnotation> = new Map();
  public layerProjection: string | undefined;
  public toProjection: string | undefined;
  public useBboxFilter: boolean;
  constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {}
  public clusterCallback(callback): void {}
  public refresh(): void {}
  public envelope(): void {}
  public haveFilter(): boolean {}
  public canFilter(): boolean {}
  public type(): string {}
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
  public async loader(extent: Extent): Promise<any> {}
  public addFeatures(data, clear?: boolean): void {}
  public layerRenderListener(event) {}
  public clusterStyleFunction(feature: Feature<Geometry>): Array<Style> {}
  public getCounterValues(): number {}
  public getLayer(): VectorLayer<VectorSource> {}
  public getVisibility(): boolean {}
  public getFilter(): string {}
  public setIcons() {}
  public setCustomIcons(styleSVG?, styleSVGClusterInd?, styleSVGCluster?) {}
}
---
// FILE: src/goapi/Layers/GoGeoJsonLayer/index.ts
export class GOGeoJsonLayer extends GOStyledLayer {
  public layerProjection: string | undefined;
  public toProjection: string | undefined;
  constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {}
  public refresh(): Promise<Vector<Feature>> {}
  public changeStyle(style: types.styleFromUser): void {}
  public canFilter(): boolean {}
  public type(): string {}
  public removeFeatures(): void {}
  public addFeatures(data, clear?: boolean): void {}
  public createGeoJson(): string {}
  public layerRenderListener(event: RenderEvent): void {}
  public getLayer(): VectorLayer<VectorSource> {}
  public getLayerName(): string {}
  public getGeometryName(): Promise<string> {}
  public getUrl(): string {}
  public getDataLayer(): string {}
  public getVisibility(): boolean {}
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
  public getGeometryType(layerGeomType): GOGeometryType {}
}
---
// FILE: src/goapi/Layers/GOWMS/index.ts
export class GOWMSLayer extends GOLayer {
  public layerProjection: string | undefined;
  constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {}
  public type(): string {}
  public canFilter(): boolean {}
  public getLayer(): TileLayer<TileSource> {}
  public getVisibility(): boolean {}
  public getLayerDescription(): string {}
  public async getLegend(layerOptsLegend: string) {}
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
// FILE: src/goapi/Layers/GoWFS/index.ts
export class GOWFSLayer extends GOStyledLayer {
  public layerProjection: string | undefined;
  public useBboxFilter;
  constructor(stagingMap: GOStagingMap, layerOpts: types.layerOptions) {}
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
  public async loader(extent: Extent): Promise<unknown> {}
  public validateLayerConfig(layerOpts: types.layerOptions) {}
  public isLoadedData(
    timeInterval = 1000,
    limit = 0
  ): CancelablePromise<boolean> {
    return new CancelablePromise((resolve, reject) => {
      try {
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
  public refresh(): void {}
  public changeStyle(style: types.styleFromUser): void {}
  public hasFilter(): boolean {}
  public canFilter(): boolean {}
  public type(): string {}
  public exportGeoJsonFormatFeatures() {}
  public layerRenderListener(event: any): void {}
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
  public getLayer(): VectorLayer<VectorSource> {}
  public getLayerName(): string {}
  public async getGeometryName(): Promise<string> {}
  public getLayerProjection(): string {}
  public getUrl(): string {}
  public getVisibility(): boolean {}
  public getFilter(): string {}
  public getLayerDescription(): CancelablePromise<string> {}
}
---
