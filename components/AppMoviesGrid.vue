<template>
  <div class="container movies">
    <!-- searched movies -->
    <div v-if="searchInput.length" id="movie-grid">
      <div v-if="searchedMovies.length" class="movies-grid">
        <div v-for="movie in searchedMovies" :key="movie.id" class="movie">
          <div class="movie-img">
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              onerror="this.onerror=null;this.src='https://kaverisias.com/wp-content/uploads/2018/01/catalog-default-img.gif';"
              :alt="movie.title"
            />

            <p class="review">
              {{ movie.vote_average }}
            </p>
            <p class="overview">
              {{ movie.overview.slice(0, 150) }}
              <span v-if="movie.overview.length > 150">...</span>
            </p>
          </div>

          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>

            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-gb', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>

            <nuxt-link class="button button-light" :to="`movies/${movie.id}`">
              More Details
            </nuxt-link>
          </div>
        </div>
      </div>

      <p v-else class="no-results">No results for "{{ searchInput }}"</p>
    </div>
    <!-- end of searched movies -->

    <!-- streaming movies -->
    <div v-else id="movie-grid" class="movies-grid">
      <div v-for="movie in movies" :key="movie.id" class="movie">
        <div class="movie-img">
          <img
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            :alt="movie.title"
          />

          <p class="review">
            {{ movie.vote_average }}
          </p>
          <p class="overview">
            {{ movie.overview.slice(0, 150) }}
            <span v-if="movie.overview.length > 150">...</span>
          </p>
        </div>

        <div class="info">
          <p class="title">
            {{ movie.title.slice(0, 25) }}
            <span v-if="movie.title.length > 25">...</span>
          </p>

          <p class="release">
            Released:
            {{
              new Date(movie.release_date).toLocaleString('en-gb', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>

          <nuxt-link class="button button-light" :to="`movies/${movie.id}`">
            More Details
          </nuxt-link>
        </div>
      </div>
    </div>
    <!-- end of streaming movies -->
  </div>
</template>

<script>
export default {
  name: 'MoviesGrid',
  props: {
    movies: {
      type: Array,
      default: () => [],
    },
    searchedMovies: {
      type: Array,
      default: () => [],
    },
    searchInput: {
      type: String,
      default: '',
    },
  },
};
</script>

<style lang="scss" scoped>
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

      .movie-img {
        position: relative;
        overflow: hidden;
        height: 100%;

        img {
          display: block;
          object-fit: cover;
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
          padding: 12px;
          color: #fff;
          transform: translateY(100%);
          transition: 0.3s ease-in-out all;
        }

        &:hover {
          .overview {
            transform: translateY(0);
          }
        }
      }

      .info {
        margin-top: auto;

        .title {
          margin-top: 8px;
          color: #fff;
          font-size: 20px;
        }

        .release {
          margin-top: 8px;
          color: #c9c9c9;
        }

        .button {
          margin-top: 8px;
        }
      }
    }
  }

  .no-results {
    color: #fff;
    line-height: 1.5;
    font-size: 20px;
  }
}
</style>
