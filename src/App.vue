<template>
  <div id="app" class="app" :style="backgroundStyle">
    <img class="logo" src="./assets/logo.svg"/>
    <planet-info :planet="current_planet" @fetchNewPlanet="fetchNewPlanet"/>
  </div>
</template>

<script>
import PlanetInfo from './components/PlanetInfo.vue';
import PlanetsStyle from "./planets/PlanetsStyle.js";

export default {
  name: 'app',
  data() {
    return {
      MAX_PLANET_ID: 61,
      current_planet_id: 0,
      current_planet: {},
      loading: false,
      gradientFirstColor: '#b7bdbd',
      gradientLastColor: '#3e4244',
    }
  },
  components: {
    PlanetInfo
  },
  methods: {
    fetchNewPlanet() {
      this.loading = true;
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

      Promise.all(films.map(filmUrl => this.fetchMovieNames(filmUrl)))
        .then(filmTitles => {
            this.current_planet = {
              name,
              climate,
              population,
              terrain,
              filmTitles
            };

            const hasMoreThanOneClimate = this.current_planet.climate.indexOf(',') > -1 ? true : false;
            const mainClimate = this.current_planet.climate.trim().substring(this.current_planet.climate.length, hasMoreThanOneClimate ? this.current_planet.climate.lastIndexOf(',') + 2 : 0);

            this.gradientFirstColor = PlanetsStyle[mainClimate].gradientFirstColor;
            this.gradientLastColor = PlanetsStyle[mainClimate].grandientEndColor;

            this.loading = false;
        });
    },
    fetchMovieNames(movieUrl){
      return fetch(movieUrl)
        .then(response => response.json())
        .then(film => film.title);
    }
  },
  computed: {
    backgroundStyle() {
      return {
        background: `radial-gradient(at 50% 20%, ${this.gradientFirstColor} 0%, ${this.gradientLastColor} 100%)`,
      };
    }
  }
}
</script>

<style>
  *{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  html{
    font-size: 62.5%;
  }
  
  .app{
    height: 100vh;
    width: 100%;
    background-color: orangered;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-family: 'Raleway', sans-serif;
    transition: background 1s;
  }

  .logo{
    height: 13rem;
    margin-bottom: 3rem;
  }

  @media only screen and (max-width: 925px) {
    html{
      font-size: 60%;
    }
  }

  @media only screen and (max-width: 850px) {
    html{
      font-size: 55%;
    }
  }

  @media only screen and (max-width: 785px) {
    html{
      font-size: 50%;
    }
  }

  @media only screen and (max-width: 580px) {
    html{
      font-size: 45%;
    }
  }
</style>
