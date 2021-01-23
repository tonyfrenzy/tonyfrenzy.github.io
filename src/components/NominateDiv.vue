<template>
  <div class="container">

    <div class="mb-3">
        <!-- @keyup.enter="searchMovie"  -->
      <input
        @keyup="searchMovie" 
        v-model="movieQuery" 
        type="text" class="form-control form-control-lg bg-dark text-white col text-center" placeholder="search movie">
    </div>

    <div class="mb-3 row">
      <div class="col-md-8">
        <section class="list-group pt-2">

          <div class="list-group-item text-center">
            <p>Movies results for <em class="lead">{{ movieQuery }}</em>: <em class="font-weight-bold">{{ movieResultsCount }}</em></p>
          </div>

          <div class="list-group-item d-flex justify-content-between align-items-center"
            v-for="movie in movies.Search" 
            :key="movie.imdbID" >

            <div class="media mr-2">
              <img class="align-self-start mr-3" :src="movie.Poster" alt="movie poster" style="display:block;overflow:hidden;width:150px;height:150px;">
              <div class="media-body text-left">
                <h5 class="mt-0">{{ movie.Title }}</h5>
                <p>{{ movie.Year }}</p>

                <button class="btn btn-primary btn-sm" @click="nominateMovie(movie)" :disabled="deactivateNominationButton(movie)" title="add to nomination lists">Nominate</button>
              </div>
            </div>
          </div>
          <!-- <div class="list-group-item d-flex justify-content-between align-items-center">
            Dapibus ac facilisis in
            <button class="btn btn-primary btn-sm">Nominate</button>
          </div> -->
        </section>
      </div>

      <div class="col-md-4">
        <section class="list-group sticky-top pt-2">
          <div class="list-group-item text-center font-weight-bold">
            <p>Your Nomination Lists: {{ nominatedMovies.length }}</p>

            <div class="alert alert-warning font-weight-normal" role="alert" v-if="nominatedMovies.length > 4">
              <small>Maximum of <strong>5</strong> nominations allowed! Thank you.</small>
            </div>
          </div>

          <div class="list-group-item d-flex justify-content-between align-items-center"
            v-for="(movie, index) in nominatedMovies" 
            :key="movie.id" >
            <span class="mr-2">{{ movie.title }} ({{ movie.releaseDate }})</span>
            <button class="btn btn-danger btn-sm" title="remove from lists" @click="removeNominatedMovie(index)">x</button>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',

  data() {
    return {
      movieQuery: '',
      movies: {},
      nominatedMoviesId: [],
      nominatedMovies: [],
      movieResultsCount : 0,
      movieQueryUrl: 'https://www.omdbapi.com/?apikey=eaaab08a',
    }
  },

  methods: {
    deactivateNominationButton(mv) {
      return this.nominatedMoviesId.includes(mv.imdbID) || this.nominatedMovies.length == 5;
    },

    /** NEW MOVIE SEARCH */
    searchMovie() {
      axios.get(this.movieQueryUrl, {
        params: { s : this.movieQuery }
      })
      .then((response) => (this.movies = response.data))
      .then(() => { 
        this.movieResultsCount = this.movies.totalResults ?? 0;
        // console.log(this.movies.Search | .Response | .totalResults);
      })
      .catch((error) => {
        console.log(error);
      });
    },

    nominateMovie(mv) {
      // Poster | Title | Type | Year | imdbID: (...)
      this.nominatedMovies.push({
        id : mv.imdbID,
        poster : mv.Poster,
        title : mv.Title,
        releaseDate : mv.Year,
      })
      this.nominatedMoviesId.push( mv.imdbID )
    },

    removeNominatedMovie(index) {
      this.nominatedMovies.splice(index, 1);
      this.nominatedMoviesId.splice(index, 1);
    }
  },

  created() {
  },

  computed: {
  }
}
</script>

<style scoped>
</style>
