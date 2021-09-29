<template>
  <div> 
    <fieldset class="filters">
        <legend>Filters:</legend>
        
        <label for="name">
            Name:
            <input 
                type="text" id="name" name="name" 
                autocomplete="none"
                v-model="nameSelected"
            >
        </label>

        <label for="zipcode">
            Post code:
            <select name="zipcode" v-model="zipcodeSelected">
                <option disabled value="">Select Zipcode</option>
                <option 
                    v-for="(zipcode, index) in zipcodes" :key="index"
                    :value="zipcode"
                >
                    {{ zipcode }}
                </option>
            </select>
        </label>

        <label for="startyear">
            Start year:
            <select name="startyear" v-model="yearSelected">
                <option disabled value="">Select Year</option>
                <option 
                    v-for="(year, index) in years" :key="index"
                    :value="year"
                >
                    {{ year }}
                </option>
            </select>
        </label>

        <label for="city">
            City:
            <select 
                id="city"
                name="city" 
                size="auto" 
                multiple
                v-model="citiesSelected"
            >
                <option 
                    v-for="(city, index) in cities" 
                    :key="index"
                    :value="city"
                >
                    {{ city }}
                </option>
            </select>
        </label>

        <button 
            class="filters-reset"
            @click="resetFilters()"
        >
            RESET
        </button>
    </fieldset>
  </div>
</template>

<script>
  export default {
    name: "FiltersPanel",
    props: {
      venues: Array
    },
    data() {
      return {
        cities: [],
        years: [],
        zipcodes: [],
        nameSelected: '',
        citiesSelected: [],
        zipcodeSelected: '',
        yearSelected: '',
        backVenues: this.venues,
        filteredVenues: this.venues,
      }
    },
    watch: {
        nameSelected: function () {
            this.reloadFilteredVenues()
            this.filteredVenues = this.filteredVenues.filter(venue => {
                return venue.name.toLowerCase().includes(this.nameSelected.toLowerCase())
            })
            this.$emit('filter-venues', this.filteredVenues)
        },
        citiesSelected: function () {
            this.reloadFilteredVenues()
            this.filteredVenues = this.filteredVenues.filter(venue => {
                return this.citiesSelected.includes(venue.city)
            })
            this.$emit('filter-venues', this.filteredVenues)
        },
        zipcodeSelected: function () {
            this.filterVenues("zipcode", this.zipcodeSelected)
        },
        yearSelected: function () {
            this.reloadFilteredVenues()
            this.filteredVenues = this.venues.filter(venue => {
                return venue.startdate >= this.yearSelected
            })
            this.$emit('filter-venues', this.filteredVenues)
        }
    },
    methods: {
        resetFilters () {
            this.filteredVenues = [...this.backVenues]
            this.$emit('filter-venues', this.filteredVenues)
        },
        reloadFilteredVenues () {
            this.filteredVenues = [...this.backVenues]
        },
        filterVenues (venueKey, filterName) {
            this.filteredVenues = [...this.backVenues]
            this.filteredVenues = this.filteredVenues.filter(venue => {
                return venue[venueKey].includes(filterName)
            })
            this.$emit('filter-venues', this.filteredVenues)
        }
    },
    created () {
        this.cities = []
        this.years = []
        this.zipcodes = []

        for (const venue of this.filteredVenues) {
            if (!this.cities.includes(venue.city)) {
                this.cities.push(venue.city)
            }
            if (!this.zipcodes.includes(venue.zipcode)) {
                this.zipcodes.push(venue.zipcode)
            }
            if (!this.years.includes(venue.startdate)) {
                this.years.push(venue.startdate)
            }
        }

        this.cities.sort()
        this.zipcodes.sort()
        this.years.sort()
    }
  }
</script>

<style scoped lang="scss">
    input, select {
        margin-right: 10px;
        width: 140px;
    }

    .filters {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-top: 10px;
        background-color: #fcf8e8;
        border: 2px solid #ffd417;

        &-reset {
            align-self: flex-end;
            padding: 5px;
            color: white;
            height: 25px;
            width: 80px;
            border: none;
            background-color: palevioletred;
        }
    }
</style>
