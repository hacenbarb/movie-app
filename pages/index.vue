<template>
  <div class="home">
    <Hero />
    <button @click="getMovies()">get movies</button>
  </div>
</template>

<script>
import axios from '@nuxtjs/axios'
export default {
  name: 'home',
  data() {
    return {
      movies: [],
      API_KEY: `68e2dc67ddfafb643c87e5acdface487`,
      BASE_URL: `https://api.themoviedb.org/3`,
      now_playing_url: `${this.BASE_URL}/movie/now_playing?api_key=${this.API_KEY}&language=en-US&page=${this.pages}`,
      pages: 20
    }
  },
  methods: {
    async getMovies() {
      const data = axios.get(this.now_playing_url)
      const result = await data
      console.log(result)
    }

  }
}
</script>
