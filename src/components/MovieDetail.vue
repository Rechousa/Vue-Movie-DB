<template>
  <transition name="fade">
    <div class="movie-wrapper" :style="styles">
      <div class="movie-info">
        <h1>{{ movie.title }}</h1>
        <h3>Release date: {{ movie.release_date}}</h3>
        <p>{{ movie.overview }}</p>
      </div>
    </div>
  </transition>
</template>

<script>
const BACKDROP_PATH = "http://image.tmdb.org/t/p/w1280";

export default {
  data() {
    return {
      movie: {}
    };
  },
  created: function() {
    this.fetchData();
  },
  computed: {
    styles() {
      if(this.movie.backdrop_path === undefined) {
        return '';
      }
      return {
        background: `url(${BACKDROP_PATH}/${
          this.movie.backdrop_path
        }) no-repeat`
      };
    }
  },
  methods: {
    fetchData: async function() {
      try {
        const res = await fetch(
          `https://api.themoviedb.org/3/movie/${
            this.$route.params.id
          }?api_key=0fab6308a97224957ba134eace13450e`
        );
        const movie = await res.json();
        this.movie = movie;
      } catch (e) {
        /*eslint no-console: ["error", { allow: ["warn", "error"] }] */
        console.warn('Error fetching data', e);
      }
    }
  }
};
</script>

<style scoped>
.movie-wrapper {
  position: absolute;
  padding-top: 50vh;
  background-size: cover;
  width: 80%;
  margin: 0 10%;
  text-align: center;
}

.movie-info {
  background: #fff;
  color: #222;
  padding: 2rem 10%;
}
</style>