<template>
  <div class="planet-card">
    <div class="planet-card__img-container" v-if="planetIconUrl != null">
      <img class="planet-card__img" :src="planetIconUrl" :alt="`${planet.name} planet`">
    </div>
    <div class="planet-card__details-container" v-if="planet.name != null">
      <h2 class="planet-card__planet-name" :style="labelFontStyle">{{planet.name}}</h2>
      <p class="planet-card__details-label" :style="labelFontStyle">Population</p>
        <p class="planet-card__details-values">{{planet.population}}</p>
      <p class="planet-card__details-label" :style="labelFontStyle">Climate</p>
        <p class="planet-card__details-values">{{planet.climate}}</p>
      <p class="planet-card__details-label" :style="labelFontStyle">Terrain</p>
        <p class="planet-card__details-values">{{planet.terrain}}</p>
      <p class="planet-card__details-label" :style="labelFontStyle">Featured in Films</p>
      <ul v-if="planet.filmTitles.length > 0" class="planet-card__details-film-list">
        <li
          v-for="(film, index) in planet.filmTitles" 
          :key="index"
          class="planet-card__details-values planet-card__details-film-list-item"
        >
          {{film}}
        </li>
      </ul>
      <p v-else class="planet-card__details-values">-</p>
    </div>

    <button class="button" @click="fetchNewPlanet" :disabled="loading">
      <img class="button__icon" src="../assets/shuffle.svg"/>
    </button>
  </div>
</template>

<script>
import PlanetsStyle from "../planets/PlanetsStyle.js";

export default {
  props: {
    planet: {
               type: Object,
               required: true,
               default: () => {},
            },
    loading: {
              type: Boolean,
              default: false
    }
  },
  computed:{
    planetIconUrl(){
      if(this.planet.climate == null)
        return null;

      const hasMoreThanOneClimate = this.planet.climate.indexOf(',') > -1 ? true : false;
      const mainClimate = this.planet.climate.trim().substring(this.planet.climate.length, hasMoreThanOneClimate ? this.planet.climate.lastIndexOf(',') + 2 : 0);
      return require(`../assets/${mainClimate}.svg`);
    },

    labelFontStyle(){
      if(this.planet.climate == null)
        return '#FFF';

      const hasMoreThanOneClimate = this.planet.climate.indexOf(',') > -1 ? true : false;
      const mainClimate = this.planet.climate.trim().substring(this.planet.climate.length, hasMoreThanOneClimate ? this.planet.climate.lastIndexOf(',') + 2 : 0);
      return {
        color: PlanetsStyle[mainClimate].fontColor
      }
    }
  },
  methods: {
    fetchNewPlanet(){
      this.$emit('fetchNewPlanet');
    }
  }
}
</script>

<style scoped>
  .planet-card{
    display: flex;
    background: #272626;
    border-radius: 1rem;
    width: 70vw;
    max-width: 80rem;
    height: 60vh;
    padding: 3rem;
    position: relative;
  }

  .planet-card__img-container{
    width: 50%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  .planet-card__img{
    width: 75%;
    height: 100%;
  }

  .planet-card__details-container{
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .planet-card__planet-name{
    text-transform: uppercase;
    font-weight: bold;
    color: rgb(215, 215, 215);
    font-size: 5rem;
    line-height: 4.5rem;
    margin-bottom: 2rem;
  }

  .planet-card__details-label{
    font-size: 1.5rem;
  }

  .planet-card__details-film-list{
    list-style: none;
  }

  .planet-card__details-values{
    color: #fff;
    font-weight: bold;
    font-size: 2rem;
    margin-left: 2rem;
    text-transform: capitalize;
    margin-bottom: 1rem;
  }

  .planet-card__details-values:last-child{
    margin-bottom: 0;
  }

  .planet-card__details-film-list-item{
    margin-bottom: 0;
  }

  .button{
    background: white;
    border: none;
    cursor: pointer;
    height: 8rem;
    width: 8rem;
    border-radius: 50%;
    position: absolute;
    top: 100%;
    left: 100%;
    transform: translate(-50%, -50%);
    transition: left 1s;
  }

  .button__icon{
    width: 50%;
  }

  @media only screen and (max-width: 580px) {
    .planet-card__img-container{
      display: none;
    }

    .planet-card__details-container{
      width: 100%;
    }
    .button{
      left:50%;
    }
  }
</style>
