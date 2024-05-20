<template>
  <div>
    <div class="container">
      <div style="float: left"><b-button variant="outline-dark" size="sm" class="refresh"
          @click="refresh()">Refresh</b-button></div>
      <div style="float: right" >center: {{center}}, zoom: {{ zoom }}, <span class="btn btn-dark btn-sm">total: {{ markers.length }}</span></div>
      <div style="clear: both;"></div>

      <LeafletMap :markers="markers" :zoom="zoom" :center="center"/>
    </div>
  </div>
</template>


<script>
// @ is an alias to /src
import LeafletMap from '@/components/LeafletMap.vue'

export default {
  name: 'SearchView',
  components: {
    LeafletMap
  },
  data() {
    return {

      /**
       * Coordinates API
       */
      apiCoordinates: "http://" + location.hostname + ":" + process.env.VUE_APP_LOCAL_PROXY_PORT + process.env.VUE_APP_API_COORDINATES,

      /**
       * Map zoom
       */
      zoom : 9,

      markers: [
          // { lat: 50.23694871652907 , lng: 19.01825272895186 , label: "center " },
          // { lat: 50.23694871652907 , lng: 17.725811458173524, label: "west " },
          // { lat: 50.23694871652907 , lng: 20.31069399973014, label: "east " },
          // { lat: 51.06369525698675 , lng: 19.01825272895186 , label: "nort" },
          // { lat: 49.410202176071394 , lng: 19.01825272895186 , label: "south" },
      ],

      center : [50.23, 19.02]
    };
  },
  mounted() {
    //
  },
  methods: {
    zoomUpdated (value) {
            this.zoom = value;
    },
    centerUpdated(value){
            this.center = value;
    },

    async refresh() {

      const request = {
        method: "GET",
        headers: {
          "Accept": "application/json",
          "Content-Type": "application/json",
        },
        mode: "cors"
      };
      let [lat,lng] = this.center;

      const queryParams = "?lat="+lat+"&lng="+lng+"&zoom="+this.zoom;
      const url = this.apiCoordinates + queryParams;
      // const url = "http://minikube.local:30303" + process.env.VUE_APP_API_COORDINATES + queryParams;
      
      // console.log("Calling " + url);
      fetch(url, request)
        .then(async response => {
          const data = await response.json();
          // console.log("Got response: " + data);
          this.markers = data.coordinates;
          this.errorMessage = "";
        }).catch(error => {
          this.errorMessage = "There was an error while getting coordinates" + error;
        })

    },
  }
}
</script>

<style scoped>
.container {
  width: 50vw;
  margin-top: 10px;
}

.refresh {
  margin-bottom: 2px;
  border: 1px solid #fff;
  border-bottom: 2px solid #343A40;

}

</style>


