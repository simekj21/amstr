<template>
  <div id="app" class="amstr">
    <h1>
      <hr>
      FOOD&COFFEE
      <br>
      AMSTERDAM
      <hr>
    </h1>
    <control-panel
      @set-map-view="isSetMapView = $event"
      @set-table-view="isSetTableView = $event"
    />
    <filters-panel
      :venues="venues"
      @filter-venues="filterVenues"
    />
    <map-view 
      v-if="isSetMapView"
      :venues="venues"
      :focus-venue-index="selectedVenueIndex"
    />
    <table-view
      v-if="isSetTableView"
      :venues="venues"
      @venue-select="selectedVenueIndex = $event"
    />
  </div>
</template>

<script>
import VenuesJson from './assets/establishment-data.json'
import ControlPanel from './components/ControlPanel'
import FiltersPanel from './components/FiltersPanel.vue'
import TableView from './components/TableView'
import MapView from './components/MapView.vue'

export default {
  name: 'App',
  components: {
    ControlPanel,
    FiltersPanel,
    TableView,
    MapView
  },
  data () {
    return {
      venuesData: VenuesJson,
      venues: [],
      backVenues: [],
      venue: {
        name: String,
        city: String,
        zipcode: String,
        address: String,
        latitude: Number,
        longitude: Number,
        startdate: String,
        URL: String,
        description: String,
        pictures: Array,
        display: Boolean,
      },
      isSetMapView: true,
      isSetTableView: true,
      selectedVenueIndex: 0,
    }
  },
  methods: {
    filterVenues (e) {
      this.venues = e
    },
  },
  created () {
    const maxOfVenues = 50
    let numOfVenus = 0
    
    this.venues = []
    this.backVenues = []

    for (const venueData of this.venuesData) {
      this.venue = {}
      this.venue.name = venueData.details.en.title
      this.venue.city = venueData.location.city
      this.venue.zipcode = venueData.location.zipcode
      this.venue.address = venueData.location.adress
      this.venue.latitude = Number.parseFloat(venueData.location.latitude.replace(",","."))
      this.venue.longitude = Number.parseFloat(venueData.location.longitude.replace(",","."))

      this.venue.startdate = venueData.dates.startdate ? venueData.dates.startdate.slice(6,10) : "-"
      this.venue.URL = venueData.urls[0]
      this.venue.description = venueData.details.en.shortdescription
      this.venue.pictures = venueData.media
      this.venues.push(this.venue)

      numOfVenus++
      if (numOfVenus >= maxOfVenues) {
        break
      }
    }
    
    this.backVenues = [...this.venues]
  }
}
</script>

<style>
  .amstr {
    display: flex;
    flex-direction: column;
    justify-content: center;
    
    font-family: Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    margin-top: 20px;
    font-size: 14px;
  }
</style>
