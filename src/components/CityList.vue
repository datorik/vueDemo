<template>
  <div class="cityContainer" id="cityContainer">
    <h1>Vue Select</h1>
    <vSelect
            :options="label" label="name"
            @search="searchEvent"
            @input="input"
            :filterable="false">
      <li slot="list-footer" class="pagination">
        <button @click="prevPage" :disabled="!showPrev">Prev</button>
        <button @click="nextPage" :disabled="!showNext">Next</button>
      </li>
    </vSelect>


  </div>
</template>


<script lang="ts">

  import vSelect from 'vue-select'
  import 'vue-select/dist/vue-select.css';
  import cityData from '@/assets/city.list.json'

  export default {
    components: {
      vSelect
    },
    data() {
      return {
        select: {state: 'Florida', abbr: 'FL'},
        search: '',
        offset: 0,
        page: 0,
        limit: 10,
        showNext: false,
        showPrev: false
      }
    },
    methods: {
      searchEvent (val:string):void {
        this.search = val;
        this.page = 0;
      },

      getCitys ():any {
        const search:string = this.search;
        function isPositive(element:any) {
          return element.name.includes(search);
        }
        let result:any = cityData.filter(isPositive);
        let pageStart: number = this.page * this.limit;
        let pageAnd: number = pageStart + this.limit;

        result.length > pageAnd ? this.showNext = true: this.showNext = false;
        pageStart != 0 ? this.showPrev = true: this.showPrev = false;
        return result.slice(pageStart,pageAnd);
      },

      nextPage ():void {
        this.page++;
      },

      prevPage ():void {
        this.page--;
      },
      input(val: any): void {
        if (val) {
          this.$emit('searchWeather', val)
        }
      },
    },
    computed: {
      label():any {
        return this.getCitys();
      }
    }
  }
</script>




<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
