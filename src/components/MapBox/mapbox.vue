<template>
  <div>
    <Radio-group @on-change="tagStyle">
      <Radio label="streets-v11">街道</Radio>
      <Radio label="light-v10">浅色</Radio>
      <Radio label="dark-v10">深色</Radio>
      <Radio label="outdoors-v11">户外</Radio>
      <Radio label="satellite-v9">卫星</Radio>
    </Radio-group>
    <Button @click="removeAllLayers">图层清除</Button>
    <div id="map"></div>
  </div>
</template>

<script>
export default {
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      console.log("11");
      this.$mapboxgl.accessToken =
        "pk.eyJ1IjoibGk3ODk1MDEiLCJhIjoiY2t0OGZnZDNuMTFvbzJ2cXdtcWQ4Ymd2ZSJ9.LkhaAox9QkNlfT88bS9D5g";
      this.map = new this.$mapboxgl.Map({
        container: "map", // container id
        style: "mapbox://styles/mapbox/light-v10", // stylesheet location
        center: [114.26752342322533, 30.57183078295523], // starting position [lng, lat]
        zoom: 9, // starting zoom
      });
      this.map.on("click", (e) => {
        console.log(e.lngLat);
      });
      //添加要素
      this.map.on("load", () => {
        this.map.addLayer({
          id: "testLayer",
          type: "fill",
          source: {
            type: "geojson",
            data: {
              type: "Feature",
              geometry: {
                type: "Polygon",
                coordinates: [
                  [
                    [114.35, 30.83],
                    [114.71, 30.56],
                    [114.28, 30.34],
                    [113.88, 30.55],
                    [114.35, 30.83],
                  ],
                ],
              },
              properties: {
                descripition: 
                "<strong>武汉<strong><p>初到武汉<p>"
                ,
              },
            },
          },
          layout: {},
          paint: {
            "fill-color": "#088",
            "fill-opacity": 0.8,
          },
        });
      });
      //悬浮弹出层
      this.map.on('mouseenter','testLayer',(e)=>{
        this.map.getCanvas().style.cursor = 'pointer'
        console.log(e.features)
      })
      this.map.on('mouseleave','testLayer',()=>{
        this.map.getCanvas().style.cursor = ''
      })
    },
    tagStyle(id) {
      this.map.setStyle("mapbox://styles/mapbox/" + id);
    },
    removeAllLayers() {
      console.log(this.map);
      const layers = this.map.getStyle().layers;
      for (let i = 0; i < layers.length; i++) {
        let id = layers[i].id;
        this.map.removeLayer(id);
      }
    },
  },
};
</script>

<style>
#map {
  /* position: absolute;
  top: 20px;
  bottom: 0;
  width: 100%; */
  height: 500px;
  /* overflow: hidden; */
}
</style>