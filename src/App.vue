<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" @click="fetchNewPlanet">
    <planet-info :planet="current_planet"/>
  </div>
</template>

<script>
import PlanetInfo from './components/PlanetInfo.vue'

export default {
  name: 'app',
  data() {
    return {
      MAX_PLANET_ID: 61,
      current_planet_id: 0,
      current_planet: {},
    }
  },
  components: {
    PlanetInfo
  },
  methods: {
    fetchNewPlanet() {
      this.current_planet_id = this.generateRandomPlanetId()
      fetch(`https://swapi.co/api/planets/${this.current_planet_id}`)
        .then(response => response.json())
        .then(planet => this.setCurrentPlanet(planet));
    },
    generateRandomPlanetId(){
      let generatedId = Math.floor((Math.random() * this.MAX_PLANET_ID) + 1);

      if(generatedId === this.current_planet_id)
        return this.generateRandomPlanetId();

      return generatedId;
    },
    setCurrentPlanet(planet){
      const { name, climate, population, terrain, films } = planet;
      
      this.current_planet = {
        name,
        climate,
        population,
        terrain,
        films
      }
    },
    
  },
  created(){
    this.fetchNewPlanet();
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
