<template>
  <div class="window border border-secondary rounded p-2 mb-2 bg-white" :class="{expanded: isExpanded}">
    
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="window-name fw-bold pe-3">{{window.name}}</div>
      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex">
        <ul class="list">
        <li class="open-status ms-4" :class="{open: isWindowOpen, closed: !isWindowOpen}">
        <template v-if="isWindowOpen">
          <span class="icon">&#x2B24;</span> Window Open
        </template>
        <template v-else>
          <span class="icon">&#x2716;</span> Window Closed
        </template>
      </li>
      <li> <span class="right">{{window.roomName}} </span></li>
    </ul>
        <button type="button" class="btn btn-secondary me-2" @click="switchWindow">{{ isWindowOpen ? 'Close' : 'Open' }} window</button>
        <button type="button" class="btn p-2 btn-danger me-2" v-on:click="deleteWindow">Delete window</button>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'WindowsListItem',
  props: ['window'],
  data: function() {
    return {
      isExpanded: false
    }
  }, 
  computed: {
    isWindowOpen: function() {
      return this.window.windowStatus === 'OPEN'; 
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchWindow() {
      let response = await axios.put(`${API_HOST}/api/windows/${this.window.id}/switch`);
      let updatedWindow = response.data;
      this.$emit('window-updated', updatedWindow);
    },
    async deleteWindow(){
      await axios.delete(`${API_HOST}/api/windows/${this.window.id}`);
      location.reload()
    }
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
      
      
    }
  }

  &.closed {
    color: #dc3545;
  }
}

.window {
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
