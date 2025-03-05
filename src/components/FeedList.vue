<script setup>
import { onMounted, ref } from "vue";

const feedList = ref([]);

const removeFeed = (feed) => {
  let newFeed = JSON.parse(localStorage.getItem("feedList"));
  newFeed.splice(feed, 1);
  localStorage.setItem('feedList', JSON.stringify(newFeed));
  feedList.value = JSON.parse(localStorage.getItem("feedList")  || "[]");
}

const getFeed = () => {
  feedList.value = JSON.parse(localStorage.getItem("feedList")  || "[]");
}

onMounted(() => {
  getFeed()
  })
</script>

<template>
  <div class="list-container">
    <h2>RSS Feeds</h2>

    <div v-if="feedList.length === 0" class="no-feeds">
      No feeds added yet.
    </div>

    <div v-for="(feed, index) in feedList" :key="feed.feedUrl" class="feed-card">
      <h3>{{ feed.feedTitle }}</h3>
      <p>{{ feed.description ? feed.description : "No description available" }}</p>
      <a href="#" @click.prevent="$emit('viewFeed',feed.feedUrl)">View Feed</a>
      <br/>
      <a href="#" @click.prevent="$emit('editFeed',index)">Edit Feed</a>
      <br/>
      <a href="#" @click.prevent="removeFeed(index)">Delete Feed</a>
    </div>
  </div>
</template>

<style scoped>
.list-container {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 500px;
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

.feed-card {
  background: #f9f9f9;
  padding: 15px;
  border-radius: 5px;
  margin-top: 10px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
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
}

.feed-card a:hover {
  text-decoration: underline;
}
</style>
