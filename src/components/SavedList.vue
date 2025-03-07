<script setup>
import { onMounted, ref } from "vue";

const savedList = ref([]);
const savedXMLs = ref([]);
const searchBar = ref('');

const removeSaved = (article) => {
  getSaved();
  savedList.value.splice(article, 1);
  parseSaved();
  localStorage.setItem('savedArticles', JSON.stringify(savedList.value));
}

const getSaved = () => {
  savedList.value = JSON.parse(localStorage.getItem("savedArticles")  || "[]");
}

const parseSaved = () => {
  savedXMLs.value = [];
  const parser = new DOMParser();
  for (const index in savedList.value) {
    savedXMLs.value[index] = parser.parseFromString(savedList.value[index], "application/xml");
  }
}


onMounted(() => {
  getSaved();
  parseSaved();
})

</script>

<template>
  <div class="list-container">
    <h2>Saved Articles</h2>

    <div v-if="savedXMLs.length === 0" class="no-feeds">
      No articles saved yet.
    </div>

    <div v-if="savedXMLs.length !== 0" class="search-bar">
      Search saved feeds:
      <input type="text" v-model="searchBar" placeholder="Search Title or Description" />
    </div>

    <div v-for="(article, index) in savedXMLs" class="feed-list">
      <div v-if="article.querySelector('title').textContent.includes(searchBar) || article.querySelector('description')?.textContent.includes(searchBar)" class="news-item">
        <h3>{{ article.querySelector("title").textContent }}</h3>
        <p>{{ article.querySelector("pubDate")?.textContent }}</p>
        <p>{{ article.querySelector("description")?.textContent }}</p>
        <a :href="article.querySelector('link').textContent" target="_blank">View Article</a>
        <br/>
        <a href="#" @click.prevent="removeSaved(index)">Remove Article</a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.list-container {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  max-width: 80%;
  text-align: center;
  flex-direction: column;
  margin: 100px;
}

h2 {
  text-align: center;
  color: black;
}

.no-feeds {
  text-align: center;
  font-style: italic;
  color: #666;
}

.feed-list {
  width: 100%;
}

.news-item {
  background: #fff;
  padding: 15px;
  margin-top: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.news-image {
  width: 100%;
  max-height: 300px;
  object-fit: cover;
  border-radius: 5px;
  margin-top: 10px;
}

h3 {
  margin: 0;
  color: black;
}

p {
  color: #444;
}

a {
  display: inline-block;
  margin-top: 5px;
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
