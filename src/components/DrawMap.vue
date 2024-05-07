<template>  
    <div id="map" ref="map"></div>  
  </template>  
    
  <script>  
  import Map from 'ol/Map';  
  import View from 'ol/View';  
  import TileLayer from 'ol/layer/Tile';  
  import OSM from 'ol/source/OSM';  
  import VectorLayer from 'ol/layer/Vector';  
  import VectorSource from 'ol/source/Vector';  
  import Draw from 'ol/interaction/Draw';  
  import GeoJSON from 'ol/format/GeoJSON';  
//   import { Feature } from 'ol';  
    
  export default {  
    name: 'MapComponent',  
    data() {  
      return {  
        map: null,  
        draw: null,  
        source: null,  
      };  
    },  
    mounted() {  
      this.initMap();  
      this.initDraw();  
    },  
    methods: {  
      initMap() {  
        this.map = new Map({  
          target: this.$refs.map,  
          layers: [  
            new TileLayer({  
              source: new OSM()  
            })  
          ],  
          view: new View({  
            center: [0, 0],  
            zoom: 2  
          })  
        });  
          
        this.source = new VectorSource();  
        const vectorLayer = new VectorLayer({  
          source: this.source  
        });  
        this.map.addLayer(vectorLayer);  
      },  
      initDraw() {  
        this.draw = new Draw({  
          source: this.source,  
          type: 'Polygon'  
        });  
        this.map.addInteraction(this.draw);  
          
        this.draw.on('drawend', this.handleDrawEnd);  
      },  
      handleDrawEnd(event) {  
        const feature = event.feature;  
        const geojson = new GeoJSON().writeFeature(feature);  
        // TODO: 发送geojson到服务器进行保存  
        console.log(geojson);  
      },  
      loadFeaturesFromServer() {  
        // TODO: 从服务器获取GeoJSON数据  
        // 假设你从服务器获取了GeoJSON数据  
        const geojsonData = {}; // 从服务器获取的数据  
          
        const format = new GeoJSON();  
        const features = format.readFeatures(geojsonData);  
        this.source.clear(); // 清除之前的特征  
        this.source.addFeatures(features); // 添加新特征  
      }  
    },  
    beforeDestroy() {  
      if (this.map) {  
        this.map.dispose();  
        this.map = null;  
      }  
      if (this.draw) {  
        this.map.removeInteraction(this.draw);  
        this.draw = null;  
      }  
    }  
  }
  </script>  
  <style scoped>  
  #map {  
    width: 100%;  
    height: 400px;  
  }  
  </style>