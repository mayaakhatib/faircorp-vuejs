<template>
  <div class="heaters-list pt-3">
    <br>
    <heater-new @heater-added="addHeater"></heater-new>
    <br>
    <heaters-list-item 
      v-for="heater in heaters"
      :heater="heater"
      :key="heater.id"  
      @heater-updated="updateheater"
    >
    </heaters-list-item>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
import heatersListItem from './HeatersListItem';
import HeaterNew from './HeaterNew';

export default {
  components: {
    heatersListItem,
    HeaterNew
  },
  name: 'heatersList',
  data: function() {
    return {
      heaters: []
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/heaters`);
    let heaters = response.data;
    this.heaters = heaters;
  },
  methods: {
    updateheater(newheater) {
      let index = this.heaters.findIndex(heater => heater.id === newheater.id);
      this.heaters.splice(index, 1, newheater);
    },
    addHeater(newheater) {
    this.heaters.push(newheater);
  }
  }
}
</script>