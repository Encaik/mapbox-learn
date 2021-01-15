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
          },
          geoServer: {
            type: "geojson",
            data:
              "http://172.16.3.36:8080/geoserver/cite/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=cite:hangzhou_erase&maxFeatures=50&outputFormat=application%2Fjson"
          }
        },
        layers: [
          {
            id: "tdtImg",
            type: "raster",
            source: "tdtImg",
            minzoom: 0,
            maxzoom: 18
          },
          {
            id: "geoServer",
            type: "fill",
            source: "geoServer",
            paint: {
              "fill-color": [
                "match",
                ["get", "NAME99"],
                "杭州",
                "rgba(255,0,255,0)",
                "rgba(0,0,0,0.7)"
              ],
              "fill-outline-color": "rgba(95,120,202,1)"
            }
          }
        ]
      },
      minZoom: 0,
      maxZoom: 17,
      maxBounds: [
        [73.33, 3.51],
        [135.05, 53.33]
      ]
    });
    this.map.on("load", () => {
      this.map.flyTo({
        center: [this.$route.query.lon, this.$route.query.lat],
        zoom: 9
      });
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
