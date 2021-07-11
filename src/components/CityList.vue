<template>
  <div class="cityContainer" id="cityContainer">
    <h1 class="cityContainer__title">Select City</h1>
    <vSelect
            class="cityContainer__input"
            :options="label"
            :value="select"
            label="name"
            @search="searchEvent"
            @input="input"
            :filterable="false">
      <li slot="list-footer" class="pagination">
        <button @click="prevPage" :disabled="!showPrev" class="pagination__button">Prev</button>
        <button @click="nextPage" :disabled="!showNext" class="pagination__button">Next</button>
      </li>
    </vSelect>


  </div>
</template>


<script lang="ts">

  import vSelect from 'vue-select'
  import 'vue-select/dist/vue-select.css';
  import cityData from '../assets/city.list.json'

export default {
  components: {
    vSelect
  },
  data() {
    return {
      select: null,
      search: '',
      offset: 0,
      page: 0,
      limit: 10,
      showNext: false,
      showPrev: false,
    }
  },
  mounted() {
    let me = this;
    function showPosition(position:any) {
      let lat = position.coords.latitude.toFixed(6);
      let lon = position.coords.longitude.toFixed(6);

      // lon = 47.159401;
      // lat =  34.330502;

      let result:any = cityData.filter(isPositive);
      function isPositive(element:any) {
        return (element.coord.lon == lon && element.coord.lat == lat)
      }

      if(result.length){
        (<any>me).$data.select = result[0];
        (<any>me).$emit('searchWeather',  result[0]);
      }
    }

    navigator.geolocation.getCurrentPosition(showPosition);
  },
  methods: {
    searchEvent (val:string):void {
      (<any>this).search = val;
      (<any>this).page = 0;
    },

    getCitys ():any {
      const search:string = (<any>this).search;
      function isPositive(element:any) {
        return element.name.includes(search);
      }
      let result:any = cityData.filter(isPositive);
      let pageStart: number = (<any>this).page * (<any>this).limit;
      let pageAnd: number = pageStart + (<any>this).limit;

      result.length > pageAnd ? (<any>this).showNext = true: (<any>this).showNext = false;
      pageStart != 0 ? (<any>this).showPrev = true: (<any>this).showPrev = false;
      return result.slice(pageStart,pageAnd);
    },

    nextPage ():void {
      (<any>this).page++;
    },

    prevPage ():void {
      (<any>this).page--;
    },
    input(val: any): void {
      (<any>this).select = val;
      if (val) {
        (<any>this).$emit('searchWeather', val);
      }
    },
  },
  computed: {
    label():any {
      return (<any>this).getCitys();
    }
  }
}
</script>

<style scoped lang="sass">
  @import "../assets/colors"
  .cityContainer
    &__input
      padding: 5px 0px
      margin: auto
      @media only screen and (min-width: 600px)
        max-width: 360px


  .pagination
    width: 100%
    display: flex
    align-items: center
    justify-content: center
    &__button
      background-color: $element-color
      color: #ffffff
      padding: 2px 10px
      border-radius: 5px
      margin: 0 20px
    &__button:disabled
      background-color: #b3bfd2






</style>
