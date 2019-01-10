<template>
  <transition name="localfade">
  <div>
    <ul>
      <li v-for="movie in movies" :key="movie.id">
        <Movie :movie="movie"/>
      </li>
    </ul>
  </div>
  </transition>
</template>

<script>
import Movie from "./Movie.vue";

export default {
  name: "MoviesList",
  data() {
    return {
      movies: []
    };
  },
  created: function() {
    this.fetchData();
  },
  methods: {
    fetchData: async function() {
      try {
        const res = await fetch(
          "https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=0fab6308a97224957ba134eace13450e"
        );
        const movies = await res.json();
        this.movies = movies.results;
      } catch (e) {
        /*eslint no-console: ["error", { allow: ["warn", "error"] }] */
        console.warn('Error fetching data', e);
      }
    }
  },
  components: {
    Movie
  }
};
</script>

<style scoped>
div {
    position: absolute;
    width: 100%;
}

ul {
  display: grid;
  list-style: none;
  padding: 1rem;
  margin: 0;
  grid-row-gap: 1rem;
  grid-template-columns: repeat(6, 1fr);
}
</style>