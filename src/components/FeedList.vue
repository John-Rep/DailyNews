<script setup>
import { onMounted, ref } from "vue";

const feedList = ref([]);

const removeFeed = (index) => {
  let storedFeeds = JSON.parse(localStorage.getItem("feedList")) || [];
  
  if (index >= 0 && index < storedFeeds.length) {
    storedFeeds.splice(index, 1);
    localStorage.setItem("feedList", JSON.stringify(storedFeeds));
    feedList.value = storedFeeds;
  }
};

const getFeed = () => {
  feedList.value = JSON.parse(localStorage.getItem("feedList")) || [];
};

onMounted(getFeed);
</script>

<template>
  <div class="list-container">
    <h2>RSS Feeds</h2>

    <div v-if="feedList.length === 0" class="no-feeds">
      No feeds added yet.
    </div>

    <div class="feed-grid">
      <div v-for="(feed, index) in feedList" :key="feed.feedUrl" class="feed-card">
        <h3>{{ feed.feedTitle }}</h3>
        <p>{{ feed.description || "No description available" }}</p>
        <a href="#" @click.prevent="$emit('viewFeed', feed.feedUrl)">View Feed</a>
        <a href="#" @click.prevent="$emit('editFeed', index)">Edit Feed</a>
        <a href="#" class="delete" @click.prevent="removeFeed(index)">Delete Feed</a>
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
  width: 90%;
  max-width: 800px;
  margin: 50px auto;
  text-align: center;
}

h2 {
  color: black;
}

.no-feeds {
  text-align: center;
  font-style: italic;
  color: #666;
}

.feed-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 15px;
  margin-top: 20px;
}

.feed-card {
  background: #f9f9f9;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  text-align: left;
}

.feed-card h3 {
  margin: 0;
  color: black;
}

.feed-card p {
  color: #444;
}

.feed-card a {
  display: inline-block;
  margin-top: 5px;
  color: #007bff;
  text-decoration: none;
  margin-right: 10px;
}

.feed-card a.delete {
  color: red;
}

.feed-card a:hover {
  text-decoration: underline;
}
</style>
