<template>
  <div style="map-view">
    <gmap-map
      class="map-view-gmap"
      :center="center"
      :zoom="10"
    >
      <gmap-marker
        v-for="(marker, index) in markers"
        :key="index"
        :position="marker.position"
        :title="marker.title"
        :icon="marker.icon"
        @click="center=marker.position"
      ></gmap-marker>
    </gmap-map>
  </div>
</template>
 
<script>
export default {
  name: "MapView",
  props: {
    venues: Array,
    focusVenueIndex: Number,
  },
  data() {
    return {
      center: { lat: 52.37403, lng: 4.88969 },
      zoomer: this.focusVenueIndex,
      markers: [],
      currentPlace: null,
      icon: {},
    };
  },
  watch: {
    focusVenueIndex () {
      this.center = {
        lat: this.venues[this.focusVenueIndex].latitude,
        lng: this.venues[this.focusVenueIndex].longitude 
      }

      this.markers[this.focusVenueIndex].icon = {
        url: require('./../assets/maps_icon02.png'),
        size: {width: 50, height: 50, f: 'px', b: 'px',},
        scaledSize: {width: 50, height: 50, f: 'px', b: 'px',}
      }
    },
    venues: {
      handler: function() {
        this.createMarkers()
      },
      deep: true
    },
  },
  methods: {
    createMarkers () {
      this.markers = []

      for (const venue of this.venues) {
          const info = `${venue.name}\n${venue.address}\n${venue.city}\n`
          
          this.markers.push({
            position: {
              lat: venue.latitude,
              lng: venue.longitude,
            },
            title: info,
          })
      }
    },
    setPlace(place) {
      this.currentPlace = place;
    },
  },
  beforeUpdate() {
    this.createMarkers()
    console.log("Update:", this.focusVenueIndex)
  },
  mounted() {
    this.createMarkers()
    console.log("Markers:", this.markers)
  },
}
</script>

<style scoped lang="scss">
.map-view {
  &-gmap {
    margin-top: 20px;
    width: 100%;
    height: 300px;
  }
}
</style>