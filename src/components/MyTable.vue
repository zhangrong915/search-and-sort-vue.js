<template>
    <table id="table">
        <thead>
            <tr>
                <th v-for="column in columns"
                @click="sortBy(column)"
                :class="{active:sortKey==column}">
                {{column|capitalize}}
                <span class="arrow" :class="flagSort[column]===true?'dsc':'asc'"></span>
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="data in filteredData">
                <td v-for="key in data">{{key}}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
  name: 'table',
  props:{
      datas:Array,
      columns:Array,
      filterKey:String
      },
  data:function () {
    var flagSort={};
    this.columns.forEach(function(value){
        flagSort[value]=false;//true表示降序，false表示升序
    })
    return {
       sortKey:'',
       flagSort:flagSort
    }
  },
  computed:{
    filteredData: function () {
        var data=this.datas;
        var sortKey=this.sortKey;
        var flag=this.flagSort[sortKey];
        var filterKey=this.filterKey&&this.filterKey.toLowerCase();
        if(sortKey){
          data=this.sortList(sortKey,data,flag);
        }
        if(filterKey){
           data=data.filter(function(row){
               return Object.keys(row).some(function(key){
                    return String(row[key]).toLowerCase().indexOf(filterKey)>-1;
               })
           })
        }
        return data;
    }
  },
  methods:{
    sortBy:function(sortKey){
        this.sortKey=sortKey;  
        this.flagSort[sortKey]=!this.flagSort[sortKey];
    },
    sortList: function (sortBy, list, flag) {
        return list.sort(function(a, b) {
            var value= (b[sortBy] - a[sortBy])||(b[sortBy].toLowerCase().localeCompare(a[sortBy].toLowerCase()));
            /**flag等于true时，降序 */
            if(!flag) { return value*(-1); }
            return value;
        });
    }         
  },
  filters:{
    capitalize:function(str){
        return str.charAt(0).toUpperCase()+str.slice(1);
    }
  }
}
</script>



<style>
table {
    margin-top:10px;
    border: 2px solid #42b983;
}
table thead tr{
    background:#42b983;
}
table tr:nth-child(2n){
   background:#eee;
}
table tr th,
table tr td{
    text-align:left;
    padding:5px 20px 5px 5px;
}
th{
    color:rgba(255,255,255,0.7);
}
th.active{
    color:white;
}
.arrow{
    width:0;
    height:0;
    display: inline-block;
    vertical-align: middle;
    margin-left:10px;
    opacity:0.7;
}
.arrow.dsc{
    border-left:4px solid transparent;
    border-right:4px solid transparent;
    border-top:6px solid white;
}
.arrow.asc{
    border-left:4px solid transparent;
    border-right:4px solid transparent;
    border-bottom:6px solid white;
}
th.active .arrow{
    opacity:1;
}
</style>