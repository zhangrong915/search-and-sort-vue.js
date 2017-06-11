<template>
  <div id="app">
    <form>
      <input type="search" name="search" id="search" placeholder="search..." v-model="searchValue" />
    </form>
    <MyTable
    :datas="gridData"
    :columns="gridColumns"
    :filterKey="searchValue">
    </MyTable>
  </div>
</template>

<script>
import MyTable from './components/MyTable.vue';
export default {
  name: 'app',
  data () {
    return {
      searchValue:'',
      gridColumns:['name','power'],
      gridData:[
        { name: 'aet Li', power: 100 },
        { name: 'Chuck Norris', power: Infinity },
        { name: 'Bruce Lee', power: 9000 },
        { name: 'Zackie Chan', power: 7000 },
        { name: 'Jet Li', power: 8000 },
        { name: 'bet Li', power: 900 }
      ]
    }
  },
  components:{
    MyTable
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  text-align:left;
  margin-top: 20px;
}
form #search {
  text-indent:5px;
  height:30px;
  width:200px;
}

</style>
