<template>
    <div id="content">
        <h1>Coding Challenge</h1>
        <div class="form-group">
          <label for="city">City</label>
          <select v-model="city" name="city">
            <option value="all">All</option>
            <option value="milwaukee">Milwaukee</option>
          </select>
          <label for="type">Type</label>
          <select v-model="type" name="type">
            <option value="all">All</option>
            <option value="micro">Micro</option>
            <option value="nano">Nano</option>
            <option value="regional">Regional</option>
            <option value="brewpub">Brewpub</option>
            <option value="large">Large</option>
            <option value="planning">Planning</option>
            <option value="bar">Bar</option>
            <option value="contract">Contract</option>
            <option value="proprieter">Proprieter</option>
            <option value="closed">Closed</option>
          </select>
          <button @click="changeQuery">Submit</button>
        </div>

        <table class="brewery_table">
          <thead>
            <tr>
              <th>Name</th>
              <th>Type</th>
              <th>Street</th>
              <th>Phone</th>
              <th>Website</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in breweries" :key="item.id">
              <td>{{item.name}}</td>
              <td>{{item.brewery_type}}</td>
              <td>{{item.street}}</td>
              <td>{{item.phone}}</td>
              <td>{{item.website_url}}</td>
            </tr>
          </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data(){
    return{
      breweries: [],
      city: "all",
      type:"all"
    }
  },
  async mounted() {
      this.breweries = (await axios.get("https://api.openbrewerydb.org/breweries")).data
      console.log(this.breweries)
  },
  methods:{
    async changeQuery(){
      let qs = `${this.city !== "all" ? "by_city=" + this.city : '' }${this.type !== "all" && this.city !== "all" ? '&' : ''}${this.type !== "all" ? "by_type=" + this.type : ''}`
      console.log(qs)
      this.breweries = (await axios.get(`https://api.openbrewerydb.org/breweries?${qs}`)).data
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#content{
  margin: 0 5%;
}
.brewery_table{
  width: 100%;
}
.brewery_table tbody tr{
  margin: 10px 0;
}
tbody tr{
  border-bottom: 5px solid grey;
}
tbody:last-child{
  border-bottom: none;

}
thead tr{
  border-bottom: 5px solid darkslategray;
}
</style>
