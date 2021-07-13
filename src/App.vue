<template>
  <div id="app" :class="[dayNight]">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view v-on:timesOfDay="timesOfDay"/>
  </div>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';

  @Component({

    data() {
      return {
        dayNight: 'day',
      }
    },
    methods:{
      timesOfDay(val: string) {
        (<any>this).dayNight = val;
      }
    }
  })
  export default class Home extends Vue {}
</script>

<style lang="scss">
  @import "/assets/colors";

  html,
  body {
    margin: 0;
    padding: 0;
    height: 100%;
  }

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;

}
.day {
  color: $main-color-day;
  background-color: $main-bg-day;
}

.night {
  color: $main-color-night;
  background-color: $main-bg-night;
}
#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: $element-color;
    }
  }
}
</style>
