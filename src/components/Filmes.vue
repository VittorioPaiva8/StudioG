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
                        <img :src="movie.image" alt="Poster do filme" class="movie-poster">
                        <h2>{{ movie.title }}</h2>
                    </div>
                    <div class="card-back">
                        <p><strong>Diretor:</strong> {{ movie.director }}</p>
                        <p><strong>Ano:</strong> {{ movie.release_date }}</p>
                        <p><strong>Assistir mais tarde</strong> {{ movie.description || 'Sem descrição' }}</p>
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
                flippedCard: null,
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
                    console.error("Erro ao buscar filmes:", error);
                }
            },
            toggleDetails(id){
                this.flippedCard = this.flippedCard === id ? null : id;
            },
        },
    };
</script>

<style>
.movies-container {
 display: grid;
  
}

h1 {
  text-align: center;
  color: #333;
}

.movies-list {
  gap: 30px;
  max-width: 1200px;
  width: 100%; 
}
.movie-card {
  width: 100%; 
  height: 350px;
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

.card-front, .card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border: 1px solid #ddd;
  border-radius: 10px;
}

.card-front {
  background-color: #333;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.movie-poster {
  width: 100%;
  height: 80%;
  object-fit: cover;
  border-bottom: 1px solid #ddd;
}

.card-back {
  background-color: #333;
  transform: rotateY(180deg);
  padding: 10px;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>





<!-- MUDAR O GRID DO APP  -->
