<template>
    <div class="building border border-secondary rounded shadow-sm p-2 mb-2 bg-white" :class="{expanded: isExpanded}">
      <div class="top-row d-flex" v-on:click="toggleExpand">
        <div class="building-name fw-bold pe-3">{{building.name}}</div>
        <div class="building id-name text-muted">{{building.id}}</div>

        <div class="expand-button ms-auto">
          {{ isExpanded ? '&#9660;' : '&#9658;' }}
        </div>
      </div>
      <template v-if="isExpanded">
        <hr/>
        <div class="details d-flex">
          <button type="button" class="btn p-2 btn-danger me-2" v-on:click="deletebuilding">Delete This Building</button>
        </div>
      </template>
    </div>
  </template>




<script>

import axios from 'axios';
import { API_HOST } from '../config';
import RoomsListItem from './RoomsListItem.vue'

export default {
    name: 'BuildingsListItem',
    props: ['building'],
    data: function() {
    return {
      isExpanded: false
    }
    },     
    methods: {
        toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
        async deletebuilding(){
      await axios.delete(`${API_HOST}/api/buildings/${this.building.id}`);
      location.reload()
    },
    }
  }



</script>

<style lang="scss" scoped>

.open-status {
    .icon {
        position: relative;
    }

    &.open {
        color: #198754;
        .icon {
        font-size: 12px;
        top: -3px;
        }
    } 

    &.closed {
        color: #dc3545;
    }
}

.building {
    .top-row {
        cursor: pointer;
    }
}
.details>.p-2{
    background-color: #dc3545;
    color: azure;
    border-radius: 0.2em;
}
</style>
