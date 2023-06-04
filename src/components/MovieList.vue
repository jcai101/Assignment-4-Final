<script setup>
import { ref } from "vue";
import axios from "axios";

const APIkey = `a75766149f058c94cbde1356c3161786`;
const movieData = ref(false);
const trailerData = ref(false);
const movieID = ref(null);
let bActive = ref(false);

const getTMDBData = async () => {
  movieData.value = (
    await axios.get(
      `https://api.themoviedb.org/3/movie/${movieID.value}?api_key=${APIkey}&language=en-US`
    )
  ).data;

  trailerData.value = (
    await axios.get(
      `https://api.themoviedb.org/3//movie/${movieID.value}/videos?api_key=${APIkey}&language=en-US&adult=false`
    )
  ).data;

  bActive = true;
};

</script>

<template>
  <div class="main">
    <section id="container">
      <form action="#" id="navbar">
        <label for="select-movies">Select Movies</label>
        <select v-model="movieID">
          <option value="857">Saving Private Ryan</option>
          <option value="603692">John Wick: Chapter 4</option>
          <option value="39514">RED</option>
          <option value="677179">Creed III</option>
          <option value="19995">Avatar</option>
          <option value="502356">The Super Mario Bros. Movie</option>
          <option value="798286">Beau Is Afraid</option>
          <option value="405775">The Wall</option>
          <option value="4247">Scary Movie</option>
          <option value="315162">Puss in Boots: The Last Wish</option>
          <option value="12162">The Hurt Locker</option>
        </select>
        <button @click="getTMDBData">Get Movie</button>
      </form>

      <section
        id="movie-section"
        v-if="movieData"
      >
        <div id="poster">
          <img
            id="poster"
            :src="`https://image.tmdb.org/t/p/w500${movieData.poster_path}`"
            alt="poster"
          />
        </div>
        <div id="movie-info">
          <h1 id="title">{{ movieData.title }}</h1>
          <p id="production">Production Companies: {{movieData.production_companies[0].name}}</p>
          <p id="tagline">{{ movieData.tagline }}</p>
          <br />
          <p id="overview">Synopsis: {{ movieData.overview }}</p>
          <br />


          <h4 id="genres">Genre: {{ movieData.genres[0].name }}</h4>
          <h4 id="popularity">Popularity: {{ movieData.vote_count }}</h4>
          <h4 id="average-score">Average Score: {{ movieData.vote_average }}</h4>
          
          <h4 id="runtime">Runtime: {{ movieData.runtime }} mins</h4>
          <h4 id="release-date">Release Date: {{ movieData.release_date }}</h4>
          <h4 id="revenue">Revenue: ${{ movieData.revenue }}</h4>
          
          <br />

          <iframe
            id="trailer"
            :src="`https://www.youtube.com/embed/${
              trailerData.results
                .filter((trailer) => trailer.type === 'Trailer')
                .at(0).key
            }`"
            frameborder="0"
          ></iframe>
        </div>
      </section>
    </section>
  </div>
</template>

<style scoped>
  .main {
    background-color: black;
    height: 100vh;
    overflow: hidden;
  }

  h1 {
    font-size: 3ch;
  }

  #container {
    height: 100vh;
    display: flex;
    gap: 0.5em;
    flex-direction: column;
    justify-content: center;
    align-items: left;
    color: whitesmoke;
  }

  #navbar {
    gap: 0.5em;
    display: flex;
    position: absolute;
    top: 0;
    padding: 1.5em;
  }

  #movie-section {
    display: flex;
    flex-direction: row;
    max-width: 48em;
    background-color: black;
  }

  #poster {
    max-height: 36em;
  }

  #title {
    text-decoration: underline;
  }

  #movie-info {
    justify-content: center;
    align-items: center;
    padding: 1em;
  }

  #trailer {
    height: 15em;
    width: 20em;
    margin: 1em 1em 0em 1em;
  }
</style>