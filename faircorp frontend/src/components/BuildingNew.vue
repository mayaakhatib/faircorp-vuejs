<template>
    <div class="card">
      <div class="card-header">
        <h2 class="mb-0">
          <button
            class="btn btn-link btn-block text-left d-flex"
            type="button"
            @click="toggleExpand"
          >
            <div class="mr-2">Create New Building</div>
            <div class="expand-button ml-auto">
              {{ isExpanded ? "&#9660;" : "&#9658;" }}
            </div>
          </button>
        </h2>
      </div>
  
      <template class="collapse show" v-if="isExpanded">
        <form class="p-4">
          <div class="form-group">
            <label for="building_name">Building Name: </label>
            <input
              class="form-control"
                  v-model="building_name"
                  type="text"
                  id="building_name"
                  name="building_name"
              placeholder="building"
            />
          </div>
  
          <div class="form-group">
            <label for="building_outside_temperature"
                  >Building Outside Temperature:
                </label>
            <input
              class="form-control"
                  type="number"
                  v-model="building_outside_temperature"
                  id="building_outside_temperature"
                  name="building_outside_temperature"
            />
          </div>
          <button type="button" class="btn btn-primary" @click="addBuilding">
            Create Building
          </button>
        </form>
      </template>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  import { API_HOST } from "../config";
  
  export default {
    name: "BuildingNew",
    data: function () {
      return {
        isExpanded: false,
        building_name: "",
        building_outside_temperature: "",
      };
    },
    created: async function () {
      //let response = await axios.get(`${API_HOST}/api/buildings`);
      //let buildings = response.data;
      //this.buildings = buildings;
      //this.room_building = buildings[0];
    },
    methods: {
      toggleExpand() {
        this.isExpanded = !this.isExpanded;
      },
      async addBuilding() {
        let newBuilding = {
          name: this.building_name,
          outsideTemperature: this.building_outside_temperature,

        };
        let headers = {
          "Content-Type": "application/json",
        };
        let response = await axios.post(`${API_HOST}/api/buildings`, newBuilding, {
          headers,
        });
        this.$emit("building-added", response.data);
      },
    },
  };
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
  
  .room {
    .top-row {
      cursor: pointer;
    }
  }
  </style>
  