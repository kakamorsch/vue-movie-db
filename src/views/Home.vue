<template>
  <div class="home">
    <div class="featured-card">
      <router-link to="/movie/tt0409591">
        <img
          src="https://i.pinimg.com/originals/71/73/59/71735901596a554a253359909bb39266.jpg"
          alt="naruto poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Naruto Uzumaki (うずまきナルト, Uzumaki Naruto) is a shinobi of
            Konohagakure's Uzumaki clan. He became the jinchūriki of the
            Nine-Tails on the day of his birth — a fate that caused him to be
            shunned by most of Konoha throughout his childhood. After joining
            Team Kakashi, Naruto worked hard to gain the village's
            acknowledgement all the while chasing his dream to become Hokage. In
            the following years, through many hardships and ordeals, he became a
            capable ninja regarded as a hero both by the villagers, and soon
            after, the rest of the world, becoming known as the Hero of the
            Hidden Leaf (木ノ葉隠れの英雄, Konohagakure no Eiyū, literally
            meaning: Hero of the Hidden Tree Leaves). He soon proved to be one
            of the main factors in winning the Fourth Shinobi World War, leading
            him to achieve his dream and become the village's Seventh Hokage
            (七代目火影, Nanadaime Hokage, literally meaning: Seventh Fire
            Shadow).
          </p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input
        type="text"
        placeholder="What are you looking for?"
        v-model="search"
      />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
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
</template>

<script>
import { ref } from "vue";
import env from "@/env.js";
export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
        .then((res) => res.json())
        .then((data) => {
          movies.value = data.Search;
          search.value = "";
          console.log(movies.value);
        });
    };

    return {
      search,
      movies,
      SearchMovies,
    };
  },
};
</script>
<style lang="scss" scoped>
.home {
  .featured-card {
    position: relative;
    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }
    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba($color: #000000, $alpha: 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }
      p {
        color: #fff;
      }
    }
  }

  .search-box {
    display: flex;

    flex-direction: column;

    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
          text-align: center;
        }
        &::focus {
          box-shadow: 0px 3px 6px rgba($color: #000000, $alpha: 0.2);
        }
      }
      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;
    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }
        }
        .type {
          position: absolute;
          padding: 8px 16px;
          background-color: #42b883;
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
          .year {
            color: #aaa;
            font-size: 14px;
          }

          h3 {
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
