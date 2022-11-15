<template>
  <div class="bodyy" id="app">    
    <header>
      <div class="title display-6 m-4">{{title}}</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <ul class="nav nav-pills">
        <li
          v-for="tab in tabs"
          v-bind:key="tab"
          v-bind:href="['#', {tab}]"
          v-bind:class="['nav-link', { active: currentTab === tab }]"
          data-toggle="pill"
          v-on:click="currentTab = tab"
        >
        {{tab}}
        </li>
      </ul>
      <component v-bind:is="currentTabComponent" class="tab"></component>
    </section>
  </div>
</template>

<script>
import MainNavigation from './components/MainNavigation.vue';
import WindowsList from './components/WindowsList.vue';
import RoomsList from './components/RoomsList.vue';
import BuildingsList from './components/BuildingsList.vue';
import HeatersList from './components/HeatersList.vue';


export default {
  name: 'App',
  components: {
    BuildingsList,
    RoomsList,
    WindowsList,
    HeatersList,    
  },
  data() {
    return {
      title: 'Welcome To Faircorp ',
      currentTab: "Buildings",
      tabs: ["Buildings", "Rooms","Windows","Heaters",]
      
    }
    
  },
  computed: {
          currentTabComponent(){
            return this.currentTab.toLowerCase() + "-list";
          }
        }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color:#2c3e50;
  //margin-top: 60px;
}
.nav-pills{
  cursor: pointer;
}
.nav-pills > .active, .nav-pills > .active:hover {
    background-color: #30cf2a;
}


.bodyy{
  //min-height: 100vh;
    //padding: 0;
    //margin: 0;
    background-image: url("@/assets/greenblue2.png");
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: cover;
    position: fixed;
    top: 0;
    bottom:0;
    left:0;
    width: 100%;
    height: 100%;
}
.title {
  color: rgb(253, 255, 254);
  font-size:2cm;
}

</style>
