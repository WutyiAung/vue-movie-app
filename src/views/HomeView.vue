<template>
  <div class="home">
    <div class="featured-card">
      <router-link to="/movie/tt0409591">
        <img src="../assets/op.jpg" alt="one piece poster" />
        <div class="detail">
          <h3>One Piece</h3>
          <p>
            It is the Great Age of Piracy. The legendary King of the Pirates,
            Gold Roger left his treasure known as the “ONE PIECE” somewhere in
            the world for it to be found. This lured men to the great seas,
            causing fierce battles around the world. As a young boy, Luffy
            admired pirates.
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="searchMovies" class="search-box">
      <input type="text" placeholder="what are you looking for ?" v-model="search" />
      <input type="submit" value="search" />
    </form>
    <div v-if="movies">
      <div class="movie-list">
        <div class="movie" v-for="movie in movies" :key="movie.imdbID">
          <router-link :to="{ name: 'detail', params: { id: movie.imdbID } }" class="movie-link">
            <div class="product-img">
              <img :src="movie.Poster" alt="movie poster" />
              <div class="type">{{ movie.Type }}</div>
            </div>
            <div class="detail">
              <p class="year">{{ movie.Year }}</p>
              <h3>{{ movie.Title }}</h3>
            </div>
          </router-link>
        </div>
      </div>
    </div>
    <div v-if="error" class="error">
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import { ref } from "vue";
import env from "@/env.js";
export default {
  components: {},
  setup() {
    let search = ref("");
    let movies = ref("");
    let error = ref(null);
    let data = ref("");
    let searchMovies = async () => {
      // if (search.value !== "") {
      //   fetch(`http://www.omdbapi.com/?i=tt3896198&apikey=${env.apikey}&s=${search.value}`)
      //     .then(response => response.json())
      //     .then(data => {
      //       movies.value = data.Search;
      //       search.value = "";
      //     })
      // }
      try {
        let response = await fetch(
          `http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`
        );
        data.value = await response.json();
        if (data.value.Response === "False") {
          search.value = "";
          throw new Error("Movie not found");
        }
        movies.value = data.value.Search;
        search.value = "";

      } catch (err) {
        error.value = err.message;
      }
    };
    return { search, movies, searchMovies, error };
  },
};
</script>
<style>
.home {
  padding: 0 5rem;
  margin-bottom: 5rem;
}

.home .featured-card {
  position: relative;
}

.featured-card img {
  display: block;
  object-fit: cover;
  position: relative;
  width: 100%;
  height: 300px;
  z-index: 0;
}

.featured-card .detail {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 16px;
  color: #fff;
}

.detail h3 {
  margin-bottom: 16px;
  font-size: 1.5rem;
}

.search-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 16px;
  align-items: center;
}

input {
  display: block;
  appearance: none;
  border: none;
  outline: none;
  background: none;
}

input[type="text"] {
  width: 100%;
  color: #fff;
  background-color: #496583;
  font-size: 20px;
  padding: 10px 16px;
  border-radius: 8px;
  margin-bottom: 15px;
  transition: 0.4s;
}

input[type="text"]::placeholder {
  color: #f3f3f3;
}

input[type="text"]:focus {
  box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
}

input[type="submit"] {
  width: 100%;
  max-width: 300px;
  background-color: #42b883;
  padding: 16px;
  color: #fff;
  font-size: 20px;
  border-radius: 8px;
  text-transform: uppercase;
  transition: 0.4s;
}

input[type="submit"]:active {
  background-color: #249361;
}

/* .movie-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0px 8px;
}
.movie {
  max-width: 50%;
  flex: 1 1 50%;
  padding: 16px 8px;
}
.movie-link {
  display: flex;
  flex-direction: column;
  height: 100%;
}
.product-img {
  position: relative;
  display: block;
}
.product-img img {
  display: block;
  width: 100%;
  height: 275px;
  object-fit: cover;
}
.type {
  position: absolute;
  padding: 8px 16px;
  background: #42b883;
  color: #fff;
  bottom: 16px;
  left: 0px;
  text-transform: capitalize;
}
.detail {
  background-color: #496583;
  padding: 16px 8px;
  flex: 1 1 100%;
  border-radius: 0px 0px 8px 8px;
}
.year {
  color: #fff;
  font-size: 14px;
}
h3 {
  color: #fff;
  font-weight: 600;
  font-size: 18px;
}
.error{
  font-size: 1.5rem;
  color: #fff;
  text-align:  center;
} */
.movie-list {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 2rem;
}

.movie-list .movie {
  max-width: 50%;
  flex: 1 1 50%;
  padding: 8px 16px;
}

.product-img {
  position: relative;
}

.product-img img {
  width: 100%;
  height: 275px;
  object-fit: cover;
}

.product-img .type {
  position: absolute;
  bottom: 16px;
  left: 0px;
  padding: 8px 16px;
  font-size: 14px;
  color: #fff;
  background-color: #249361;
  text-transform: capitalize;
}

.movie-link {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.movie-link .detail {
  flex: 1 1 100%;
  height: 100%;
  background: #496583;
  color: #fff;
  padding: 16px 8px;
  border-radius: 0px 0px 8px 8px;
}

.year {
  color: #fff;
  font-size: 14px;
}

h3 {
  color: #fff;
  font-weight: 600;
  font-size: 18px;
}

.error {
  font-size: 1.5rem;
  color: #fff;
  text-align: center;
}

@media (max-width: 600px) {
  .movie-list {
    flex-direction: column;
  }

  .movie-list .movie {
    max-width: 100%;
    flex: 1 1 100%;
  }
}
</style>
