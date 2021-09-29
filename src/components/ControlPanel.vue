<template>
  <div>
    <fieldset class="control">
        <legend>Mode of view:</legend>
        <label for="map">Map:</label>
        <input 
            type="checkbox" 
            id="map" 
            name="map"
            v-model="isSetMapView"
            @change="$emit('set-map-view', isSetMapView)"
        >
        <label for="table">Table:</label>
        <input 
            type="checkbox" 
            id="table" 
            name="table"
            v-model="isSetTableView"
            @change="$emit('set-table-view', isSetTableView)"
        >
    </fieldset>
  </div>
</template>

<script>
  export default {
    name: "ControlPanel",
    props: {
    },
    data() {
      return {
        isSetMapView: true,
        isSetTableView: true,
      }
    }
  }
</script>

<style scoped lang="scss">
    .control {
        display: flex;
        flex-direction: row;
        justify-content: center;
        border: 2px solid gray;
        padding: 10px;
        justify-content: flex-start;
        background-color: rgb(240, 249, 255);
    }
</style>