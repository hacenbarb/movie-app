<template>
  <div class="home">
    <Hero />
    <div class="movies container">
      <div id="movie-grid" class="movie-grid">
        <div class="movie" v-for="(movie, index) in movies" :key="index">
          <div class="movie_media">
            <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="movie_media">
            <p class="movie_review">{{movie.vote_average}}</p>
            <p class="movie_overview">{{movie.overview}}</p>
          </div>
          <div class="movie_info">
            <p class="movie_title">{{movie.title.slice(0,25)}}<span v-if="movie.title.length > 25">...</span></p>
            <p class="movie_release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-uk', {
                  day: 'numeric',
                  month: 'long',
                  year: 'numeric'
                })
              }}
            </p>
            <NuxtLink class="btn" :to="{name: 'movies-movieid', params: {movieid: movie.id}}">View More</NuxtLink>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'home',
  data() {
    return {
      movies: [],
      API_KEY: `68e2dc67ddfafb643c87e5acdface487`,
      BASE_URL: `https://api.themoviedb.org/3`,
      now_playing_url: `https://api.themoviedb.org/3/movie/now_playing?api_key=68e2dc67ddfafb643c87e5acdface487&language=en-US&page=20`,
      pages: 20
    }
  },
  async fetch() {
    await this.getMovies()
  },
  methods: {
    async getMovies() {
      const data = axios.get(this.now_playing_url)
      const result = await data
      this.movies = result.data.results
      console.log(this.movies)
      return result.data
    }

  }
}
</script>
