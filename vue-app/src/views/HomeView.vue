<template>
  <div class="home">
    <!-- 検索フィールド -->
    <input
      type="text"
      v-model="searchTitle"
      class="form-control"
      placeholder="映画を検索"
    />
    <button @click="getMovies" class="btn btn-primary">検索</button>
    <!-- 映画一覧表示 -->
    <div class="flex_test-box">
      <div v-for="movie in movies" :key="movie.imdbId" class="flex_test-item">
        <h3>{{ movie.Title }}</h3>
        <img width="400" :src="movie.Poster" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import { defineComponent, reactive, ref } from "vue";

interface Movie {
  imdbId: string;
  Year: string;
  Type: string;
  Title: string;
  Poster: string;
}

export default defineComponent({
  name: "HomeView",
  setup() {
    const searchTitle = ref("");
    const movies = reactive<Movie[]>([]);

    // http://www.omdbapi.com/
    // ?apikey=f00
    // &s   検索用のタイトル

    const getMovies = () => {
      axios
        .get(`http://www.omdbapi.com/?apikey=f099aa0a&s=${searchTitle.value}`)
        .then((response) => {
          const data = response.data.Search as Movie[];
          movies.length = 0;
          movies.push(...data);
        });
    };

    return {
      movies,
      searchTitle,
      getMovies,
    };
  },
});
</script>

<style scoped>
.flex_test-box {
  background-color: #eee; /* 背景色指定 */
  padding: 10px; /* 余白指定 */
  display: flex; /* フレックスボックスにする */
  justify-content: center;
  flex-wrap: wrap; /* 折り返し指定 */
  align-content: flex-start; /* 折り返し時の配置指定 */
}

.flex_test-item {
  padding: 10px;
  margin: 10px; /* 外側の余白 */
  border-radius: 5px; /* 角丸指定 */
  width: 400px; /* 幅指定 */
  text-align: center;
}
</style>
