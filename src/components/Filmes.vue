<template>
    <div class="movies-container">
        <h1>
            Filmes do Studio Ghibli
        </h1>
        <div class="movies-list">
            <div 
                v-for="movie in movies" 
                :key="movie.id" 
                class="movie-card" 
                @click="toggleDetails(movie.id)"
            >
                <div class="card-inner" :class="{flipped: flippedCard === movie.id}">
                    <div class="card-front">
                        <h2>{{ movie.title }}</h2>
                    </div>
                    <div class="card-black">
                        <p><strong>Diretor:</strong> {{ movie.director }}</p>
                        <p><strong>Ano:</strong> {{ movie.release_date }}</p>
                        <p><strong>Descrição:</strong> {{ movie.description || 'Sem descrição' }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
    import axios from "axios";
    export default {
        data (){
            return {
                movies: [],
                flippedcard: null,
            };
        },
        mounted(){
            this.fetchMovies();
        },
        methods: {
            async fetchMovies(){
                try {
                    const response = await axios.get('https://ghibliapi.vercel.app/films');
                    this.movies = response.data;
                } catch (error){
                    console.error("vla", error);
                }
            },
            toggleDetails(id){
                this.flippedcard = this.flippedcard === id ? null: id;
            },
        },
    };
</script>


<style>
.movies-container {
  max-width: 800px;
  margin: auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
}

.movies-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.movie-card {
  width: 200px;
  height: 300px;
  perspective: 1000px;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.card-inner.flipped {
  transform: rotateY(180deg);
}

.card-front{
    background-color: #333;
}
.card-black {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.card-back {
  transform: rotateY(180deg);
  padding: 10px;
}
</style>
