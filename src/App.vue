<template>
  <div id="app" class="w-screen flex flex-col ">

    <Page msg="the world" />
      <CesiumViewer :showTile="showBuilding" />
      <button 
      id="toggle-building"
      @click="toggleButton"
      class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded-b-2xl">
        {{ btnText }}
      </button>

  </div>
</template>

<script>
import Page from './components/Page.vue'
import CesiumViewer from './components/CesiumViewer.vue'

export default {
  name: 'App',
  components: {
    Page, CesiumViewer
  },
  data() {
    return {
      showBuilding: true,
      btnText: "Show",
    }
  },
  methods: {
    toggleButton() {
      this.showBuilding = !this.showBuilding
      this.btnText = this.showBuilding ? "Hide" : "Show"
    }
  }
}
</script>

<style src="./assets/tailwind.css" >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
