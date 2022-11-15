<template>
  <div class="room border border-secondary rounded shadow-sm p-2 mb-2 bg-white" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand">
      
      <div class="room-name fw-bold pe-3">{{room.name}}</div>
      
      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      <div class="details d-flex justify-content-between m-10">
        <ul class="list">
          <li>Floor&#35;: <span class="right">{{room.floor}}</span></li>
          <li>Target Temperature: <span class="right">{{room.targetTemperature}} &#x2103;</span></li>
          <li>Current Temperature: <span class="right">{{room.currentTemperature}} &#x2103;</span></li>
          
        </ul>
        <a href="" class="del" v-on:click="deleteRoom">Delete This Room</a>
      </div>

    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'RoomsListItem',
  props: ['room'],
  data() {
    return {
      isExpanded: false,
      //isShow: false,
      //currentTemp: 0,
      //targetTemp: 0,
    }
  }, 
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    //showWindows(){
      showRooms(){
      this.isShow = !this.isShow;
      alert(this.room.name)
    },
    async deleteRoom()
        {
          await axios.delete(
              `${API_HOST}api/rooms/${this.room.id}`
          );
          this.$emit("room-deleted", this.room.id);
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

.del{
    cursor: pointer;
    color: #dc3545;
    font-size: 22px;
  }
  .list {
    padding: 10px;
    width: 50%;
    list-style: none;

    margin-left: -5px;
  }
  .list li {
   font-size: 22px;
   font-weight: bolder;
   text-align: left;
  }
  .right{
    float: right;
  }
.room {
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
