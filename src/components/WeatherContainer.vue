<template>
  <div class="cityContainer" id="weatherContainer">
    <Loader v-if="loading" />

    <div v-if="show" >
      <h1>{{name}}</h1>
      <div class="weather">
        <div class="weather__item">
          <samp>Feels Like</samp>
          <samp class="weather__text--big">{{main.feels_like}}</samp>
        </div>
        <div class="weather__item">
          <samp>Humidity</samp>
          <samp class="weather__text--big">{{main.humidity}}</samp>
        </div>
        <div class="weather__item">
          <samp>Pressure</samp>
          <samp class="weather__text--big">{{main.pressure}}</samp>
        </div>
      </div>
      <div class="weatherIcon">
        <div class="weatherIcon__image" v-bind:style="{backgroundImage: 'url(https://openweathermap.org/img/wn/'+ icon + '@2x.png'}"></div>
        <div class="weatherIcon__text">{{iconText}} </div>
      </div>
      <div class="weather">
        <div class="weather__item">
          <samp>Min</samp>
          <samp class="weather__text--big">{{main.temp_min}}</samp>
        </div>
        <div class="weather__item">
          <samp >Temp</samp>
          <samp class="weather__text--big2">{{main.temp}}</samp>
        </div>
        <div class="weather__item">
          <samp>Max</samp>
          <samp class="weather__text--big">{{main.temp_max}}</samp>
        </div>

      </div>
    </div>


  </div>
</template>


<script lang="ts">
const API_KEY: string = '002d7b9d8a832684dc08c0d738d107bf';
const OPENWEATHERMAP_URL: string = 'https://api.openweathermap.org/data/2.5/weather';

import Loader from '@/components/Loader.vue'

export default {

    props: ['cityId', 'temp'],

    data() {
        return {
            loading: false,
            show: false,
            name: '',
            main: '',
            icon: '',
            iconText: ''
        }
    },

    methods: {
        getWeather(): void {
            if (!(<any>this).cityId) {
                return;
            }
          (<any>this).loading = true;
          (<any>this).show = false;
            fetch(`${OPENWEATHERMAP_URL}?id=${(<any>this).cityId}&appid=${API_KEY}&units=${(<any>this).temp}`)
                .then((response) => {
                    if (response.ok) {
                        return response.json();
                    }
                    throw new Error('Network response was not ok');
                })
                .then((json) => {
                  (<any>this).loading = false;
                  (<any>this).show = true;
                  (<any>this).name = json.name;
                  (<any>this).main = json.main;
                  (<any>this).icon = json.weather[0].icon;
                  (<any>this).iconText = json.weather[0].main;
                })
                .catch((error) => {
                  (<any>this).loading = false;
                    console.log(error);
                });
        }
    },
    components: {
        Loader
    },
    watch: {
        cityId(): void {
          (<any>this).getWeather()
        },
        temp(): void {
          (<any>this).getWeather()
        }
    }
}
</script>

<style scoped lang="sass">

  .weather
    display: flex
    align-items: center
    justify-content: center
    &__item
      display: flex
      flex-direction: column
      flex-wrap: wrap
      padding: 0 5px
    &__text
      &--big
        font-size: 1.5rem
      &--big2
        font-size: 2.2rem

  .weatherIcon
    padding: 0 0 26px 0
    &__image
      height: 160px
      width: 160px
      background-size: cover
      margin: auto
    &__text
      margin-top: -30px
      font-size: 2rem

</style>
