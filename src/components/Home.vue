<template>
  <v-container>
  <v-layout row wrap>
    <v-flex xs12 sm6 lg4 xl3 v-for="movie in movies" :key="movie._id">
      <v-card max-width="400" class="mx-auto">
        <v-card-title class="justify-center">
          <v-btn
            text v-bind:to="`/movies/${movie._id}`">
            {{ movie.name }}
          </v-btn>
        </v-card-title>
        <v-card-subtitle class="justify-center">
          {{ movie.release_year }} . {{ movie.genre }}
        </v-card-subtitle>
        <v-card-text
          class="text-justify">
          {{ movie.description.length >= 200 ? movie.description.slice(0, 190)+ '...' : movie.description   }}
          <v-btn text v-bind:to="`/movies/${movie._id}`" >Read More</v-btn>
        </v-card-text>

        <div class="text-center">
          <v-btn
            text v-bind:to="`/movies/${movie._id}`"
            color="primary">
              Rate this movie
          </v-btn>
          <v-spacer></v-spacer>
        </div>
        <v-spacer></v-spacer>
      </v-card>
      <br>
    </v-flex>
    <br>
  </v-layout>
  <br>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Movies',
  data () {
    return {
      movies: [],
      mostRatedMovies: [],
      otherMovies: [],
    };
  },
  mounted(){
    this.fetchMovies();
  },
  methods: {
    async fetchMovies(){
     // const token = window.localStorage.getItem('auth')
      return axios({
        method: 'get',
        url: '/movies',
      })
      .then((response)=>{
        this.movies = response.data.movies;
        
       // this.current_user = response.data.current_user
      })
      .catch(()=>{});
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
