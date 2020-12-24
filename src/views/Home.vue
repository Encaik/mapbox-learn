<template>
  <div class="home">
    <div id="mapbox"></div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
export default {
  name: "Home",
  data() {
    return {
      map: null
    };
  },
  mounted() {
    let imgUrl =
      "http://t0.tianditu.gov.cn/DataServer?tk=fd5811f9ecd524b79241a5523eb8c3f7";
    mapboxgl.accessToken =
      "pk.eyJ1IjoiZW5jYWlrIiwiYSI6ImNrajJsY2NiZjI3aTAycnAzMmxjNHhkc2kifQ.AB2MHM2K_uAkMIHZYsm_dg";
    this.map = new mapboxgl.Map({
      container: "mapbox",
      style: {
        //设置版本号，一定要设置
        version: 8,
        //添加来源
        sources: {
          tdtImg: {
            type: "raster",
            tiles: [imgUrl + "&T=img_w&x={x}&y={y}&l={z}"],
            tileSize: 256
          }
        },
        layers: [
          {
            id: "tdtImg",
            type: "raster",
            source: "tdtImg",
            minzoom: 0,
            maxzoom: 18
          }
        ]
      },
      minZoom: 0,
      maxZoom: 17
    });
  }
};
</script>

<style lang="scss" scoped>
.home,
#mapbox {
  width: 100%;
  height: 100%;
}
</style>
