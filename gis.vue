<template>
   
  <div>
       
    <div
      id="viewDiv"
      :style="`height: ${height}vh; margin: 20px auto`"
    ></div>
     
  </div>
</template>
<script>
// js 代码
import { loadModules, setDefaultOptions } from "esri-loader"; //
import { arcgisurl } from '@/utils/config.js'
// import  init  from '@/utils/arcgis_js_v415_api/arcgis_js_api/library/4.15/init.js'
// import  main  from '@/utils/arcgis_js_v415_api/arcgis_js_api/library/4.15/esri/themes/light/main.css'
import {
  popupStormSystepoint,
  popupStormSysteline,
  popupSewageSystpoint,
  popupSewageSystline,
} from "@/utils/gis.js";
export default {
  props:{
    height: {
      type: Number,
      default: 83,
    },
  },
  data() {
    return {
      name: "测试路由22",
      msg: "测试demo2",
      path: [],
      line: undefined,
      geocode: undefined,
      getPoint: undefined,
      map:null,
    };
  },
  created: function () {
    this.geocode = new T.Geocoder();
  },
  mounted: function () {
    const _this = this;
    this.$nextTick(function () {
      _this.addmap()
    });
  },
  methods: {
    addmap(){
      const protocol = location.protocol
      const host = location.host
      let options = {
        url: `/forward-assets/init.js`,
        css: `/forward-assets/esri/themes/light/main.css`
      }
      setDefaultOptions({ css: true });
      loadModules([
        "esri/layers/WebTileLayer",
        "esri/Map",
        "esri/views/MapView",
        "esri/geometry/SpatialReference",
        "esri/layers/support/TileInfo",
        "esri/geometry/Extent",
      ],options).then(([
          WebTileLayer,
          Map,
          MapView,
          SpatialReference,TileInfo,Extent
      ])=>{
        var tiandituBaseUrl = "http://{subDomain}.tianditu.gov.cn"; //天地图服务地址
          var token = "5bddd36ea64d0d3afaa13d11669678ad"; //天地图管网申请token
          var map = new Map();
          this.map=map
          /**
           basemap可选择值 “topo”，“streets”，“satellite”，“hybrid”，“dark-gray”，“gray”，“national-geographic”，“oceans”，“osm”，“terrain”，“dark-gray-vector”，“gray-vector”，“streets-vector”，“streets-night-vector”，“streets-navigation-vector”，“topo-vector”，“streets-refief-vector”
           */
          this.view = new MapView({
            //这个是让地图显示在id为viewDiv的标签上，是id叫他不是其他的
            container: "viewDiv",
            map: map,
            center: [118.57090783287676, 24.90968022098196],
            zoom: 16,
          });
          //球面墨卡托投影矢量底图
          //将天地图底图添加到map上
          //矢量标注(球面墨卡托投影)
          //标注添加到map上
          // map.add(tiledLayerAnno);
          const resolutions = [1.40625,0.703125,0.3515625,0.17578125,0.08789062500000015,0.043945312500000076,0.021972656250000007,0.010986328125000019,0.0054931640625000095,0.0027465820312500017,0.0013732910156250009,0.0006866455078124989,0.00034332275390624946,0.00017166137695312503,0.00008583068847656251,0.000042915344238281406,0.000021457672119140642,0.000010728836059570307,0.000005364418029785168,0.000002682209014892578,0.000001341104507446289];
          const scales = [
            469812622.339724,
            234906311.169862,
            117453155.584931,
            58726577.7924655,
            29363288.89623275,
            14681644.448116375,
            7340822.224058188,
            3670411.112029094,
            1835205.556014547,
            917602.7780072734,
            458801.3890036367,
            229400.69450181836,
            114700.34725090918,
            57350.17362545459,
            28675.086812727295,
            14337.543406363648,
            7168.771703181824,
            3584.385851590912,
            1792.192925795456,
            896.096462897728,
            448.048231448864
          ]
          const spatialReference = new SpatialReference({ wkid:4326 })
          const fullExtent = new Extent(-180, -90, 180, 90, new SpatialReference({ wkid: 4326 }))
          const tileInfo = new TileInfo({
            "rows" : 256,
            "cols" : 256,
            "origin" : {
                "x" : -180,
                "y" : 90
            },
            "spatialReference" : {
                "wkid" : 4326
            },
            "lods": [
              {"level": "0", "scale": scales[0], "resolution": resolutions[0]},
              {"level": "1", "scale": scales[1], "resolution": resolutions[1]},
              {"level": "2", "scale": scales[2], "resolution": resolutions[2]},
              {"level": "3", "scale": scales[3], "resolution": resolutions[3]},
              {"level": "4", "scale": scales[4], "resolution": resolutions[4]},
              {"level": "5", "scale": scales[5], "resolution": resolutions[5]},
              {"level": "6", "scale": scales[6], "resolution": resolutions[6]},
              {"level": "7", "scale": scales[7], "resolution": resolutions[7]},
              {"level": "8", "scale": scales[8], "resolution": resolutions[8]},
              {"level": "9", "scale": scales[9], "resolution": resolutions[9]},
              {"level": "10", "scale": scales[10], "resolution": resolutions[10]},
              {"level": "11", "scale": scales[11], "resolution": resolutions[11]},
              {"level": "12", "scale": scales[12], "resolution": resolutions[12]},
              {"level": "13", "scale": scales[13], "resolution": resolutions[13]},
              {"level": "14", "scale": scales[14], "resolution": resolutions[14]},
              {"level": "15", "scale": scales[15], "resolution": resolutions[15]},
              {"level": "16", "scale": scales[16], "resolution": resolutions[16]},
              {"level": "17", "scale": scales[17], "resolution": resolutions[17]},
              {"level": "18", "scale": scales[18], "resolution": resolutions[18]},
              {"level": "19", "scale": scales[19], "resolution": resolutions[19]},
              {"level": "20", "scale": scales[20], "resolution": resolutions[20]}
            ]
          })
          let qzLayer = new WebTileLayer({
            urlTemplate:location.origin +`/wmts/dfc/services/ogc/wmts/QZVEC2017?service=WMTS&request=GetTile&version=1.0.0&layer=QZVEC2017&style=default&format=image/png&TileMatrixSet=CustomCRS4326ScaleQZVEC2017&TileMatrix={level}&TileRow={row}&TileCol={col}` ,
            spatialReference,
            fullExtent,
            tileInfo
           })
           map.add(qzLayer);
          setTimeout(() => {
            this.test1();
          }, 100);
      })
    },
    test1: function () {
      // lazy load the required ArcGIS API for JavaScript modules and CSS
      loadModules([
        "esri/layers/MapImageLayer",
        "esri/Graphic",
        "esri/layers/GraphicsLayer",
        "esri/layers/FeatureLayer",
      ]).then(
        ([
          MapImageLayer,
          Graphic,
          GraphicsLayer,
          FeatureLayer,
        ]) => {



          // console.log(this.view);


          //gis图层
          var gissever = new MapImageLayer({
            url:  location.origin + arcgisurl+"/arcgis/rest/services/psxt_1/MapServer",
          });

          //将gis图层添加到map上
          this.map.add(gissever);

          const trailheads0 = new FeatureLayer({
            url:  location.origin + arcgisurl+"/arcgis/rest/services/psxt_1/MapServer/0",
            outFields: [
              "Exp_No",
              "StormSyste",
              "Type",
              "X_Coor",
              "Y_Coor",
              "High",
              "WellDeep",
              "Code",
              "WellShape",
              "WellSize",
              "WellMateri",
              "Address",
            ],
            popupTemplate: popupStormSystepoint,
            labelsVisible:false,
          });

          const trailheads1 = new FeatureLayer({
            url:  location.origin + arcgisurl+"/arcgis/rest/services/psxt_1/MapServer/1",
            outFields: [
              "Lno",
              "Grade",
              "S_Point",
              "S_Deep",
              "In_Elev",
              "E_Point",
              "E_Deep",
              "Out_Elev",
              "StormSyste",
              "Type",
              "Code",
              "Material",
              "ShapeType",
              "PSize",
              "PipeLength",
              "FlowDir",
              "Address",
            ],
            popupTemplate: popupStormSysteline,
            labelsVisible:false,
          });

          const trailheads3 = new FeatureLayer({
            url:  location.origin + arcgisurl+"/arcgis/rest/services/psxt_1/MapServer/2",
            outFields: [
              "Exp_No",
              "SewageSyst",
              "Type",
              "X_Coor",
              "Y_Coor",
              "High",
              "WellDeep",
              "Code",
              "WellShape",
              "WellSize",
              "WellMateri",
              "Address",
            ],
            popupTemplate: popupSewageSystpoint,
            labelsVisible:false,
          });

          const trailheads4 = new FeatureLayer({
            url:  location.origin + arcgisurl+"/arcgis/rest/services/psxt_1/MapServer/3",
            outFields: [
              "Lno",
              "Grade",
              "S_Point",
              "S_Deep",
              "In_Elev",
              "E_Point",
              "E_Deep",
              "Out_Elev",
              "SewageSyst",
              "Type",
              "Code",
              "Material",
              "ShapeType",
              "PSize",
              "PipeLength",
              "FlowDir",
              "Address",
            ],
            popupTemplate: popupSewageSystline,
            labelsVisible:false,
          });

          this.map.add(trailheads0);
          this.map.add(trailheads1);
          this.map.add(trailheads3);
          this.map.add(trailheads4);
          //创建对象

          //点击事件
          this.view.on("click", (res) => {
            var location = {
              latitude: undefined,
              longitude: undefined,
              positionDetail: undefined,
            };
            location.latitude = res.mapPoint.latitude
            location.longitude = res.mapPoint.longitude
            // graphicsLayer.removeAll();
            this.geocode.getLocation(
              new T.LngLat(location.longitude, location.latitude),
              (rel) => {
                // console.log("geocode", rel);
                location.positionDetail = rel.formatted_address
                  ? rel.formatted_address
                  : "不知名地址";
                location.position = rel.formatted_address
                  ? rel.formatted_address
                  : "不知名地址";
                this.$emit("location", location);
              }
            );
            // console.log("on", res);
            // console.log("geocode", location.position);
          });

          // 查询事件
          this.getPoint = (location) => {
            this.geocode.getPoint(location, (res) => {
              // console.log(res)
              // graphicsLayer.removeAll();
              this.remove()
              this.view.goTo({
                center: [res.location.lon, res.location.lat],
              });
              this.addpoint([res.location.lon, res.location.lat], [255, 0, 0]);
            });
          };

          //点、线图层
          const graphicsLayer = new GraphicsLayer();
          this.map.add(graphicsLayer);

          this.line = (path) => {
            this.view.goTo({
              center: [path[0][0], path[0][1]],
            });
            const simpleMarkerSymbol = {
            type: "simple-line",
            color: [68, 105, 225], // Orange
            width: 3,
          };
            this.remove()
            this.addpoint(path[0], [39, 255, 0]);
            this.addpoint(path[path.length - 1], [255, 0, 0]);
            const polyline = {
              type: "polyline",
              paths: path,
            };
            const pointGraphic = new Graphic({
              geometry: polyline,
              symbol: simpleMarkerSymbol,
            });
            graphicsLayer.add(pointGraphic);
          };
          console.log(this.line)
          //清除图层信息
          this.remove=()=>{
            graphicsLayer.removeAll();
          }
          // 添加点
          this.addpoint = (points, color,time) => {
            const point = {
              //Create a point
              type: "point",
              longitude: points[0],
              latitude: points[1],
            };
            const simpleMarkerSymbolpoint = {
              type: "simple-marker",
              color ,
            };
            const popupTemplate={
                title: "签到时间",
                content:`${time}<br>`,
              }

            const pointGraphicpoint = new Graphic({
              geometry: point,
              symbol: simpleMarkerSymbolpoint,
              popupTemplate:time?popupTemplate:null
            });
            graphicsLayer.add(pointGraphicpoint);
          };
        }
      );
    },
  },
};
</script>
<style scoped>
  #viewDiv {
  margin: 20px auto;
  border: 1px solid #000;
}
</style>
