<template>
  <div class="home">
    <!-- HERO -->
    <Hero />
    <!-- SEARCH -->
    <div class="container">
      <div class="search">
        <input
          type="text"
          class="input no-border-radius"
          placeholder="the hobbit.."
          v-model="search_query"
          @keypress="$fetch()"
        />
        <button
          @click="clearSearch"
          class="btn no-border-radius"
          v-show="search_query.length > 1"
        >
          clear
        </button>
      </div>
    </div>
    <!-- LOADING -->
    <Loading v-if="$fetchState.pending"/>
    <!-- NOW STREAMIN -->
    <div v-else class="container movies">
      <!-- Search Results -->
      <div id="movie-grid" class="movies-grid">
        <div
          class="movie"
          v-for="(movie, index) in search_query === ''
            ? movies
            : searched_movies"
          :key="index"
        >
          <div class="movie-img">
            <div v-if="movie.poster_path === null" class="no-img"></div>
            <img
              v-else
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p
              class="overview"
              v-if="movie.overview.length == 0"
              style="text-align: center"
            >
              -- no overview --
            </p>
            <p class="overview" v-else-if="movie.overview.length < 100">
              {{ movie.overview }}
            </p>
            <p class="overview" v-else>{{ movie.overview.slice(0, 100) }}...</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25)
              }}<span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="btn"
              :to="{ name: 'movies-movieid', params: { id: movie.id } }"
            >
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import axios from 'axios'
const API_KEY = `68e2dc67ddfafb643c87e5acdface487`
const BASE_URL = `https://api.themoviedb.org/3`
const NOW_PLAYING_URL = `${BASE_URL}/movie/now_playing?api_key=${API_KEY}&language=en-US`
const SEARCH_URL = `${BASE_URL}/search/movie?api_key=${API_KEY}`

export default {
  name: 'home',
  head() {
    return {
      title: 'movies'
    }
  },
  data() {
    return {
      movies: [],
      searched_movies: [],
      show: 'movies',
      search_query: '',
      pages: 20,
    }
  },
  async fetch() {
    if (this.search_query === '')  {
      await this.getMovies()
    } else {
      await this.searchMovies()
    }
  },
  fetchDelay:1000,
  methods: {
    async getMovies() {
      const data = axios.get(`${NOW_PLAYING_URL}&page=${this.pages}`)
      const result = await data
      this.movies = result.data.results
    },
    async searchMovies() {
      await axios
        .get(
          `${SEARCH_URL}&query=${this.search_query.trim().replaceAll(' ', '+')}`
        )
        .then((res) => (this.searched_movies = res.data.results))
        .catch((err) =>
          console.error(`there's an error while searching for movies \n${err}`)
        )
    },
    clearSearch() {
      this.search_query = ''
      this.searched_movies = []
    },
  },
}
</script>
<style lang="scss">
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    align-items: center;
    .input {
      margin-bottom: 0;
      max-width: 350px;
    }
    .btn {
      background-color: #c92502;
      color: #fff;
    }
  }
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(3, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        &-img {
          position: relative;
          overflow: hidden;
          height: 100%;
          width: 100%;
          margin-bottom: 0.5rem;
          .no-img {
            width: 100%;
            height: inherit;
            background-color: #c9c9c9;
            display: flex;
            align-items: center;
            justify-content: center;
            &::after {
              content: "this movie don't have a poster";
              color: #222;
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: #c92502;
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }
          .overview {
            width: 100%;
            font-size: 1rem;
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: rgba(201, 38, 2, 0.9);
            padding: 0.75rem;
            color: #fff;
          }
        }
        .info {
          .title {
            color: #fff;
            font-size: 1.2rem;
            margin-bottom: 1rem;
          }
          .release {
            color: #c9c9c9;
            margin-bottom: 0.5rem;
          }
        }
      }
    }
  }
}
</style>
