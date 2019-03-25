<template>
  <div class="search">
    <h2> Type a Term to Search NASA Image Database </h2>
    <h3> (Must Enter a Valid Search Term): </h3>
    <form v-on:submit.prevent="getResult(encodeURIComponent(term.trim()))">
      <input type = "text" placeholder = "Type in Search Term" v-model="term" />
    </form>
    <h3> Refine by Year (Enter a Valid Range): </h3>
    <form v-on:submit.prevent="getResultRefine(encodeURIComponent(term.trim()),encodeURIComponent(ystart.trim()),encodeURIComponent(yend.trim()),encodeURIComponent(location.trim()))">
      <input type = "text" placeholder = "Year Start (YYYY)" v-model="ystart" />
    </form>
    <form v-on:submit.prevent="getResultRefine(encodeURIComponent(term.trim()),encodeURIComponent(ystart.trim()),encodeURIComponent(yend.trim()),encodeURIComponent(location.trim()))">
      <input type = "text" placeholder = "Year End (YYYY)" v-model="yend" />
    </form>
    <h3> Refine by Location (e.g. Kennedy Space Center or National Air and Space Museum): </h3>
    <form v-on:submit.prevent="getResultRefine(encodeURIComponent(term.trim()),encodeURIComponent(ystart.trim()),encodeURIComponent(yend.trim()),encodeURIComponent(location.trim()))">
      <input type = "text" placeholder = "Enter a Location" v-model="location" />
    </form>
    <div class = "results" v-if="results">
      <div v-for="result in results">
        <img v-bind:src="result.links[0].href" />
        <h3> {{"Description: " + result.data[0].description}} </h3>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'search',
  data () {
    return {
      msg: 'Search',
      term: '',
      ystart: '',
      yend: '',
      location: '',
      results: ''
    }
  },
  methods: {
    getResult(term) {
      axios.get('https://images-api.nasa.gov/search?q=' + term + '&media_type=image').then( response => {
        console.log(response.data.collection.items);
        this.results = response.data.collection.items;
      });
    },
    getResultRefine(term,ystart,yend,location) {
      if((ystart == null || ystart == "") && (yend == null || yend == "") && (location == null || location == "")) {
        getResult(term);
      } else if((ystart == null || ystart == "") && (yend == null || yend == "") && !(location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term +'&media_type=image' + '&location=' + location).then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      } else if(!(ystart == null || ystart == "") && !(yend == null || yend == "") && (location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term + '&year_start='+ ystart + '&year_end=' + yend +'&media_type=image').then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      } else if(!(ystart == null || ystart == "") && (yend == null || yend == "") && (location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term + '&year_start='+ ystart + '&media_type=image').then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      } else if((ystart == null || ystart == "") && !(yend == null || yend == "") && (location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term + '&year_end='+ yend + '&media_type=image').then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      } else if((ystart == null || ystart == "") && !(yend == null || yend == "") && !(location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term + '&year_end='+ yend + '&location=' + location + '&media_type=image').then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      } else if(!(ystart == null || ystart == "") && !(yend == null || yend == "") && !(location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term + '&year_start='+ ystart + '&year_end=' + yend +'&media_type=image' + '&location=' + location).then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      } else if(!(ystart == null || ystart == "") && !(yend == null || yend == "") && (location == null || location == "")) {
        axios.get('https://images-api.nasa.gov/search?q=' + term + '&year_start='+ ystart + '&year_end=' + yend +'&media_type=image').then( response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
        });
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.results img{
  height: 300px;
  margin: 10px;
}
h1, h2 {
  font-weight: normal;
}
h3 {
  font-size: small;
  text-align: center;
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
