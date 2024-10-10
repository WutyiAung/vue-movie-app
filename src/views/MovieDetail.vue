<template>
    <div class="movie-detail">
      <h1>{{ movie.Title }}</h1>
      <p><strong>Year:</strong> {{ movie.Year }}</p>
      <img :src="movie.Poster" alt="movie poster" class="featured-img">
      <p><strong>Plot:</strong> {{ movie.Plot }}</p>
      <p><strong>Director:</strong> {{ movie.Director }}</p>
      <p><strong>Actors:</strong> {{ movie.Actors }}</p>
      <p><strong>Genre:</strong> {{ movie.Genre }}</p>
      <p><strong>IMDb Rating:</strong> {{ movie.imdbRating }}</p>
      <p><strong>Released:</strong> {{ movie.Released }}</p>
      <p><strong>Language:</strong> {{ movie.Language }}</p>
      <p><strong>Country:</strong> {{ movie.Country }}</p>
      <p><strong>Awards:</strong> {{ movie.Awards }}</p>
      <p><strong>Writer:</strong> {{ movie.Writer }}</p>
    </div>
  </template>
  
<script>
import { onMounted, ref } from 'vue';
import env from '@/env';
export default{
 props:['id'],
 setup(props){
    let movie = ref("");
    let data = ref("");
    onMounted( async() => {
        try{
            let response = await fetch(`http://www.omdbapi.com/?i=${props.id}&apikey=${env.apikey}&plot=full`);
            data.value = await response.json();
            movie.value = data.value;
            console.log(movie.value);
        }catch{

        }
    });
    return { movie }
 }
}
</script>
<style>
  .movie-detail{
    display: flex;
    flex-direction: column;
    padding: 20px;
    max-width: 600px;
    margin: 1rem  auto 0;
    gap: 1rem;
  }
  .movie-detail img{
    width: 100%;
    height: 300px;
    object-fit: cover;
    display: block;
  }
  p{
    color: #fff;
    font-size: 16px;
  }
</style>