<template>
  <div class="buildings-list pt-3">
    <br>
    <building-new @building-added="addBuilding"></building-new>
    <br>
    <buildings-list-item 
      v-for="building in buildings"
      v-bind:building="building"
      v-bind:key="building.id"
    >
    </buildings-list-item>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import BuildingsListItem from './BuildingsListItem';
import BuildingNew from './BuildingNew';

export default {
  components: {
    BuildingsListItem,
    BuildingNew
  },
  name: 'BuildingsList',
  data() {
    return {
      buildings: []
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/buildings`);
    let buildings= response.data;
    this.buildings = buildings;
  },
  methods: {
    updateBuilding(newBuilding) {
      /* Find the place of window object with the same Id in the array, and replace it */
      let index = this.buildings.findIndex(building => building.id === newBuilding.id);
      this.buildings.splice(index, 1, newBuilding);
    },
    addBuilding(newBuilding) {
    this.buildings.push(newBuilding);
  }
}
}
</script>
