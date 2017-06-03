<template>
  <div id="app">
    <table class="table table-bordered table-hover"style="width:100%">
    <thead>
      <tr>
        <th v-for="_,key in users[0]" v-bind:class="{'sortingBy': isSortedBy(key), 'ascending': isAscending(key)}"><a href="#" v-on:click="sortBy(key)">{{key}}</a></th>
      </tr>
    </thead>
    <tbody>
    <tr v-for="user of users" data-toggle="collapse" data-target=".more">
      <td v-for="val,key in user"><span v-if="key=='address'">{{val.street}}, {{val.city}}</span>
      <span v-else-if="key=='company'">{{val.name}}</span>
      <span v-else>{{val}}</span>
      </td>
      </tr>
    </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data: function (){
    return {
      users: [],
      sortKey: 'id',
    }
  }
  ,
  methods: {
    loadUsers: function () {
      axios.get('https://jsonplaceholder.typicode.com/users').then((response) => {
        this.users= response.data;
      }, (err) => {
        console.log(err)
      })
    },
    sortBy: function (key) {
              if(key!=this.sortKey)  {
                this.ascending[this.sortKey]=true;
                this.ascending[key]=true;
              }
              if(key!='address' && key!='company'){
              var asc = this.ascending[key] = !this.ascending[key];
              this.users.sort(function (a, b) {
                  var res = a[key] > b[key];
                  if (asc) res = !res
                  return res ? 1 : -1;
              });}
              else if (key=='address') {
                var asc = this.ascending[key] = !this.ascending[key];
                this.users.sort(function (a, b) {
                    var res = a[key].street > b[key].street;
                    if (asc) res = !res
                    return res ? 1 : -1;
                });
              }
              else {
                var asc = this.ascending[key] = !this.ascending[key];
                this.users.sort(function (a, b) {
                    var res = a[key].name > b[key].name;
                    if (asc) res = !res
                    return res ? 1 : -1;
                });
              }
              this.sortKey=key;
          },
          isSortedBy: function (key) {
                        if (key == this.sortKey)
                      { return true}
                      return false
                    },
          isAscending: function(key){
                      if((key != this.sortKey)) return false;
                      if (this.ascending[key] == true) return true;
                      return false;
                }

  },
  created: function(){
    this.loadUsers();
    this.ascending = {};
  }

}
</script>
<style scoped>
tr.collapse.in {
  display:table-row;
}
.sortingBy{
  background-color:#e3f5fc;
	color: #337ab7;
	font-weight: bold;
	padding: 10px;
}
.ascending{
  background-color: #ccddff;
	color: #337ab7;
	font-weight: bold;
	padding: 10px;
}
</style>
