<template>
  <div class="home">
    <CityList @searchWeather="searchWeather"></CityList >
    <SettingsContainer @setTemp="setTemp" @timesOfDay="timesOfDay"></SettingsContainer>
    <WeatherContainer v-bind:cityId="cityId" v-bind:temp="temp"></WeatherContainer>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src
import CityList from '@/components/CityList.vue'; // @ is an alias to /src
import WeatherContainer from '@/components/WeatherContainer.vue'; // @ is an alias to /src
import SettingsContainer from '@/components/SettingsContainer.vue'; // @ is an alias to /src

@Component({
  data() {
    return {
      cityId:'',
      temp:'metric'
    }
  },
  components: {
    CityList, WeatherContainer,SettingsContainer
  },
  methods:{
    setTemp(val) {
      this.temp = val;
    },
    timesOfDay(val) {
       this.$emit('timesOfDay', val)
    },
    searchWeather(val) {
      this.cityId = val.id;
    }
  }
})
export default class Home extends Vue {}
</script>
