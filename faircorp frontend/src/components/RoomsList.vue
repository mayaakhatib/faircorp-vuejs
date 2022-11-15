<template>
  <div class="rooms-list pt-3">
      <br>
  <room-new @room-added="addRoom"></room-new>
  <br>
    <rooms-list-item 
      v-for="room in rooms"
      :room="room"
      :key="room.id"
      @room-updated="updateRoom"
    >
    </rooms-list-item>
  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomsListItem from './RoomsListItem';

import RoomNew from './RoomNew.vue';
export default {
  components: {
    RoomsListItem,
    RoomNew
  },
  name: 'RoomsList',
  data() {
    return {
      rooms: []
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/rooms`);
    let rooms = response.data;
    this.rooms = rooms;
  },
  methods: {
  updateRoom(newRoom) {
    /* Find the place of room object ith the same Id in the array, and replace it */
    let index = this.rooms.findIndex(room => room.id === newRoom.id);
    this.rooms.splice(index, 1, newRoom);
  },
 // deleteRoom(roomId) {
   // let index = this.rooms.findIndex(room => room.id === roomId);
   // if (index > -1) {
   //   this.rooms.splice(index, 1);
   // }
 // },
  addRoom(newRoom) {
    this.rooms.push(newRoom);
  }
}
}
</script>