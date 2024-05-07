<template>
   <div id="map-container"></div>
</template>

<script>
import { Map, View } from 'ol'
import { Tile as TileLayer } from 'ol/layer'
import { get } from 'ol/proj';
import { getWidth, getTopLeft } from 'ol/extent'
import { WMTS } from 'ol/source'
import WMTSTileGrid from 'ol/tilegrid/WMTS'
  
export const projection = get("EPSG:4326");  // 在view中配置的投影坐标系
const projectionExtent = projection.getExtent();
const size = getWidth(projectionExtent) / 256;
const resolutions = [];
for (let z = 2; z < 19; ++z) {
  resolutions[z] = size / Math.pow(2, z);
}
export default {
  name: 'HelloWorld',
  data() {
    return {
  
    }
  },
  mounted(){
    // this.initMap('SL') // 加载矢量底图
    this.initMap('YX') // 加载影像底图
    // this.initMap('DX') // 加载地形晕渲
  },
  methods:{
    initMap(layerType = 'SL') {
      const TIANDI_KEY = 'd9ff29e0f0dea2f8a9829d3f1972929e'
  
      // 对应的值是固定的
      const layerTypeMap = {
        'SL': ['vec', 'cva'], // [矢量底图, 矢量注记]
        'YX': ['img', 'cia'], // [影像底图, 影像注记]
        'DX': ['ter', 'cta']  // [地形晕渲, 地形注记]
      }
  
      // c: 经纬度投影 w: 球面墨卡托投影
      const matrixSet = 'w'
  
      new Map({
        target: 'map-container',
        layers: [
          // 底图
          new TileLayer({
            // 这里用WMTS作为数据源  也就是瓦片地图 下面这些都是它的配置
            source: new WMTS({
              url: `http://t{0-6}.tianditu.com/${layerTypeMap[layerType][0]}_${matrixSet}/wmts?tk=${TIANDI_KEY}`,
              layer: layerTypeMap[layerType][0],
              matrixSet: matrixSet,
              style: "default",
              crossOrigin: 'anonymous', // 解决跨域问题 如无该需求可不添加
              format: "tiles",
              wrapX: true,
              // 定义瓦片网格的详细信息
              tileGrid: new WMTSTileGrid({
                origin: getTopLeft(projectionExtent),
                //resolutions: res.slice(0, 15),
                resolutions: resolutions,
                matrixIds: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14']
              })
            })
          }),
          // 标注
          new TileLayer({
            source: new WMTS({
              url: `http://t{0-6}.tianditu.com/${layerTypeMap[layerType][1]}_${matrixSet}/wmts?tk=${TIANDI_KEY}`,
              layer: layerTypeMap[layerType][1],
              matrixSet: matrixSet,
              style: "default",
              crossOrigin: 'anonymous', // 解决跨域问题 如无该需求可不添加
              format: "tiles",
              wrapX: true,
              tileGrid: new WMTSTileGrid({
                origin: getTopLeft(projectionExtent),
                resolutions: resolutions,
                matrixIds: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14']
              })
            })
          })
        ],
        // View主要管理视图的可视参数  比如中心点，分辨率，
        view: new View({
          center: [169.40, 65.35],
          projection: projection,
          zoom: 3,
          maxZoom: 17,
          minZoom: 1
        })
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#map-container {
  margin: auto;
  width: 80vw;
  height: 80vh;
}
</style>
