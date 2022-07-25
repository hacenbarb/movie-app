<template>
  <div class="home">
    <Hero />
    <!-- <Loading v-if="$fetchState.pending" /> -->
    <!-- Movies -->
    <div class="container movies">
      <!-- Search Results -->
      <div id="movie-grid" class="movies-grid">
        <div class="movie" v-for="(movie, index) in movies" :key="index">
          <div class="movie-img">
            <div v-if="movie.poster_path === null" class="no-img"></div>
            <img
              v-else
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview" v-if="movie.overview.length == 0">-- no overview --</p>
            <p class="overview" v-else-if="movie.overview.length < 200">{{ movie.overview }}</p>
            <p class="overview" v-else>{{ movie.overview.slice(0, 199) }}...</p>
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
              :to="{ name: 'movies-id', params: { id: movie.id } }"
            >
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
    <footer>
      <p>created with ❤ by <a href="https://github.com/hacenbarb" class="link" target="_blank">hacen barboucha</a> check for the project in <a href="https://github.com/hacenbarb/movie-app" class="link" target="_blank">github</a></p>
    </footer>
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
      pages: 20,
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
    padding: 32px 16px;
    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;
      &:focus {
        outline: none;
      }
    }
    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
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
        grid-template-columns: repeat(2, 1fr);
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
          margin-bottom: .5rem;
          .no-img {
            width: 100%;
            height: inherit;
            background-color: #c9c9c9;
            display:flex;
            align-items: center;
            justify-content: center;
            &::after {
              content: 'this move don\'t have a poster';
              color: #222;
            }
          }
          &:hover {
            .overview {
              transform: translateY(0);
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
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: rgba(201, 38, 2, 0.9);
            padding: .75rem;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
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
            margin-bottom: .5rem;
          }
        }
      }
    }
  }
}
footer {
  padding-block: 1rem;
  text-align: center;
  color:#FFF;
}
</style>
