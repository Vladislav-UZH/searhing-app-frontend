<script setup>
import { ref, reactive } from "vue";
import axios from "axios";
axios.defaults.baseURL = "https://searhing-app-api.onrender.com/api";
axios.defaults.headers["Content-Type"] = "application/json; charset=utf-8";
const state = reactive({
  searchValue: "",
  searchedData: [],
});

async function fetchData({ data = {}, query = {} }) {
  try {
    const { data } = await axios.get("/questions/search", {
      params: query,
    });
    return data;
  } catch (e) {
    console.log(e.response);
  }
}

async function search(evt) {
  const searchValue = evt.target.value.trim();
  if (!searchValue) {
    return;
  }
  const { data } = await fetchData({
    query: {
      question: searchValue,
    },
  });
  state.searchedData = data.data;
}

function getSearchedResults() {
  return [
    {
      question: "Some text",
      imageUrl:
        "https://res.cloudinary.com/deoplx5oc/image/upload/v1711982695/questions/discret-tasks-28_ivmzqm.jpg",
      origPosition: 1,
      positionOnPage: 1,
      fromPage: 1,
    },
  ];
}
</script>

<template>
  <div>
    <div v-if="state.searchValue.trim()">
      <p>We will search!</p>
    </div>
    <form>
      <label>
        <p>Your question:{{ state.searchValue }}</p>
        <input
          class="field"
          @input="search"
          type="text"
          v-model="state.searchValue"
          placeholder="Put your text in me"
        />
      </label>
      <button type="reset">x</button>
    </form>
    <ul class="list">
      <li v-for="item in state.searchedData" :key="item._id">
        <span class="text"
          >Умова:
          <h3 class="question">{{ item.question }}</h3></span
        >
        <span class="text">
          Позиція на фото:
          <h2>{{ item.positionOnPage }}</h2>
        </span>
        <div class="thumb">
          <img class="image" :src="item.imageUrl" :alt="item.question" />
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
li {
  list-style: none;
}
.text {
  display: inline-block;
}
.read-the-docs {
  color: #888;
}
.field {
  width: 300px;
  height: 40px;
}
.list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.question {
}
.thumb {
  width: 600px;
  height: 800px;
}
.image {
  object-position: center;
  object-fit: cover;
  width: 100%;
  height: 100%;
}
</style>
