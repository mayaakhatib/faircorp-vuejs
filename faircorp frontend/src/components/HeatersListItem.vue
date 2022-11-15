<template>
  <div class="heater border border-secondary rounded shadow-sm p-2 mb-2 bg-white" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" v-on:click="toggleExpand">
      <div class="heater-name fw-bold pe-3">{{heater.name}}</div>
      
      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
  
      <div class="details d-flex">
        <ul class="list">

        <li class="on-status ms-4" :class="{on: isHeaterOn, off: !isHeaterOn}">
        <template v-if="isHeaterOn">
          <span class="icon">&#x2B24;</span> ON
        </template>
        <template v-else>
          <span class="icon">&#x2716;</span> OFF
        </template>
      </li>
      <li class="right">{{heater.roomName}}</li>
        </ul>
        <button type="button" class="btn p-2 btn-secondary me-2" v-on:click="switchheater">Turn {{ isHeaterOn ? 'OFF' : 'ON' }} This Heater</button>
        <button type="button" class="btn p-2 btn-danger me-2" v-on:click="deleteheater">Delete This Heater</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'heatersListItem',
  props: ['heater'],
  data: function() {
    return {
      isExpanded: false
    }
  }, 
  computed: {
    isHeaterOn: function() {
      return this.heater.heaterStatus === 'ON'; 
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchheater() {
      let response = await axios.put(`${API_HOST}/api/heaters/${this.heater.id}/switch`);
      let updatedheater = response.data;
      this.$emit('heater-updated', updatedheater);
    },
    async deleteheater(){
      await axios.delete(`${API_HOST}/api/heaters/${this.heater.id}`);
      location.reload()
    },
  }
}
</script>



<style lang="scss" scoped>
.on-status {
  .icon {
    position: relative;
  }
  &.on {
    color: #00ff00;
    .icon {
      font-size: 12px;
      top: -3px;
    }
  }
  &.off {
    color: #ff0000;
  }
}
.heater {
  .top-row {
    cursor: pointer;
  }
}

.list {
    padding: 10px;
    width: 50%;
    list-style: none;
list-style-position: initial;
    margin-left: -5px;
  }
  .list li {
   font-size: 22px;
   font-weight: bolder;
   text-align: left;
  }

  .right{
    margin-left: 40px;
  }

</style>
