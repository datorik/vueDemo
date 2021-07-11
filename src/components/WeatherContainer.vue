<template>
  <div class="cityContainer" id="weatherContainer">
    <Loader v-if="loading" />

    <div v-if="show">
      <h1>{{name}}</h1>
      <div>
        <div>
          <samp>Feels Like</samp>
          <samp>{{main.feels_like}}</samp>
        </div>
        <div>
          <samp>Humidity</samp>
          <samp>{{main.humidity}}</samp>
        </div>
        <div>
          <samp>Pressure</samp>
          <samp>{{main.pressure}}</samp>
        </div>
      </div>
      <div class="weatherIcon">
<!--        <div class="weatherIcon__image" style=" url(http://openweathermap.org/img/wn/{{icon}})"></div>-->
        <div class="weatherIcon__image" v-bind:style="{backgroundImage: 'url(http://openweathermap.org/img/wn/'+ icon + '@2x.png'}"></div>
      </div>
      <div>
        <div>
          <samp>Min</samp>
          <samp>{{main.temp_min}}</samp>
        </div>
        <div>
          <samp>Temp</samp>
          <samp>{{main.temp}}</samp>
        </div>
        <div>
          <samp>Max</samp>
          <samp>{{main.temp_max}}</samp>
        </div>

      </div>
    </div>


  </div>
</template>


<script lang="ts">
const API_KEY: string = '002d7b9d8a832684dc08c0d738d107bf';

  import {Prop, Vue} from "vue-property-decorator";
  import Loader from '@/components/Loader.vue'
  export default {

    props:['cityId', 'temp'],

    data() {
      return {
        loading: false,
        show: false,
        name:'',
        main:'',
      }
    },

    methods: {
      getWeather: function(){
        if(!this.cityId){
          return;
        }
        console.log(this.temp);
        this.loading = true;
        this.show = false;
        fetch(`http://api.openweathermap.org/data/2.5/weather?id=${this.cityId}&appid=${API_KEY}&units=${this.temp}`)

                .then((response) => {
                  if(response.ok) {
                    return response.json();
                  }

                  throw new Error('Network response was not ok');
                })
                .then((json) => {
                  this.loading = false;
                  this.show = true;
                  this.name = json.name;
                  this.main = json.main;
                  this.icon = json.weather[0].icon;
                  console.log(this.icon);
                })
                .catch((error) => {
                  this.loading = false;
                  console.log(error);
                });

      }
    },

    components: {
      Loader
    },
    watch: {
      cityId: function(){
        this.getWeather()
      },
      temp: function(){
        this.getWeather()
      }

    }

  }
</script>




<style scoped lang="scss">

  .weatherIcon__image{
    height: 160px;
    width: 160px;
    background-size: cover;
    margin: auto
  }

</style>
