<template>
  <div class="table-view">
    <table 
      class="table-view-venues"
    >
      <tr class="table-view-venues-header-fix">
        <th @click="filterColumn('name')" title="Sort">Name</th>
        <th @click="filterColumn('city')" title="Sort">City</th>
        <th @click="filterColumn('zipcode')" title="Sort">Postcode</th>
        <th @click="filterColumn('address')" title="Sort">Address</th>
        <th @click="filterColumn('startdate')" title="Sort">Start year</th>
      </tr>
      <tr 
        v-for="(venue, index) in filteredVenues" :key="index"
        @click="openModalInfo(index)"
      >
        <td>{{ venue.name }}</td>
        <td>{{ venue.city }}</td>
        <td>{{ venue.zipcode }}</td>
        <td>{{ venue.address }}</td>
        <td>{{ venue.startdate }}</td>
      </tr>
    </table>
    
    <modal-window
      v-if="showVenueDetailModal"
      :venue="selectedVenue"
      @closeModal="showVenueDetailModal=false"
    />
  </div>
</template>

<script>
import ModalWindow from './ModalWindow.vue'

export default {
  name: 'TableView',
  components: {
    ModalWindow
  },
  props: {
    venues: Array
  },
  data () {
    return {
      selectedVenueIndex: 0,
      selectedVenue: [],
      filteredVenues: [],
      showVenueDetailModal: false
    }
  },
  watch: {
    venues: {
      handler: function() {
        this.filteredVenues = this.venues
      },
      deep: true
    },
  },
  methods: {
    openModalInfo (index) {
      this.selectedVenueIndex = index
      this.selectedVenue = this.venues[index]
      this.showVenueDetailModal= true
      
      this.$emit("venue-select", this.selectedVenueIndex)
    },
    filterColumn (nameOfColumn) {
      this.filteredVenues.sort((a, b) => a[nameOfColumn] > b[nameOfColumn] ? 1 : -1)
    }
  },
  mounted () {
    this.filteredVenues = this.venues
  },
  beforeUpdate () {
    this.filteredVenues = this.venues
  }
}
</script>

<style scoped lang="scss">
  .table-view {
    margin-top: 20px;
    width: 100%;

    display: flex;
    flex-direction: column;
    justify-content: center;
  
    &-venues {
      height: 100px;
      overflow-y: auto;
      position: relative;
      border-collapse: collapse;
      width: 100%;
 
      &-header-fix {
        position: sticky;
        top: 0;
      }

      td, th {
        border: 1px solid #ddd;
        padding: 8px;
      }

      tr:nth-child(even) {
        background-color: #f2f2f2;
      }

      tr:hover {
        background-color: #ddd;
      }

      th {
        padding-top: 12px;
        padding-bottom: 12px;
        text-align: left;
        background-color: palevioletred;
        color: white;
      }

      tr {
        cursor: pointer;
      }
    }
  }
</style>
