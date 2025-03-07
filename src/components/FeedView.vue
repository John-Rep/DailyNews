<script setup>
import { ref, watch, onMounted } from "vue";

const props = defineProps({
  feedUrl: String, // Prop to receive the RSS feed URL
});

const feedTitle = ref("");
const feedDescription = ref("");
const feedLink = ref("");
const errorMessage = ref("");
const feedItems = ref([]);

const feedCount = ref(10);

const savedArticles = ref([]);

const fetchRSSFeed = async () => {
  if (!props.feedUrl) return;

  try {
    errorMessage.value = "";
    
    // Using a free proxy to fetch RSS as many sites block direct CORS requests
    const response = await fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(props.feedUrl)}`);

    if (!response.ok) throw new Error("Failed to fetch the RSS feed.");

    const data = await response.json();
    const parser = new DOMParser();
    const xml = parser.parseFromString(data.contents, "application/xml");
    const items = xml.querySelectorAll("item"); // Get all <item> elements
    feedItems.value = items;

    // Extract RSS feed information
    const titleElement = xml.querySelector("channel > title");
    const descriptionElement = xml.querySelector("channel > description");
    const linkElement = xml.querySelector("channel > link");

    feedTitle.value = titleElement ? titleElement.textContent : "No title available";
    feedDescription.value = descriptionElement ? descriptionElement.textContent : "No description available";
    feedLink.value = linkElement ? linkElement.textContent : "#";
    
  } catch (error) {
    errorMessage.value = "Error fetching feed. Please check the URL.";
  }
};

const getSavedArticles = () => {
  savedArticles.value = JSON.parse(localStorage.getItem("savedArticles")  || "[]");
}

const saveArticle = (item) => {
  getSavedArticles();
  const serializer = new XMLSerializer();
  savedArticles.value.push(serializer.serializeToString(item));
  localStorage.setItem('savedArticles', JSON.stringify(savedArticles.value));
}

const serializeItem = (item) => {
  const serializer = new XMLSerializer();
  return serializer.serializeToString(item);
}

const removeArticle = (article) => {
  getSavedArticles();
  savedArticles.value.splice(article, 1);
  localStorage.setItem('savedArticles', JSON.stringify(savedArticles.value));
}


// Fetch the feed when the component mounts or if the URL changes
onMounted(() => {fetchRSSFeed(); getSavedArticles();});
watch(() => props.feedUrl, fetchRSSFeed);

</script>

<template>
  <div class="feed-container">
    <h2>RSS Feed Information</h2>

    <label for="feedCount">Select Feed Limit</label>
    <select id="feedCount" v-model="feedCount">
      <option>10</option>
      <option>50</option>
      <option>100</option>
      <option>All</option>
    </select>

    <div>
      <div v-if="errorMessage" class="error">{{ errorMessage }}</div>

      <div v-else>
        <h2>{{ feedTitle }}</h2>
        <p>{{ feedDescription }}</p>
        <a :href="feedLink" target="_blank">Visit Site</a>
      </div>
      <div v-for="(item, index) in feedItems">
        <div v-if="feedCount == 'All' || index < feedCount" class="feed-card">
          <h3>{{ item.querySelector("title").textContent }}</h3>
          <p>{{ item.querySelector("pubDate")?.textContent }}</p>
          <p>{{ item.querySelector("description")?.textContent }}</p>
          <a :href="item.querySelector('link').textContent" target="_blank">View Article</a>
          <br/>
          <!-- <p>{{ savedArticles }}</p> -->
          <a v-if="!savedArticles.includes(serializeItem(item))" href="#" @click.prevent="saveArticle(item)">Save Article</a>
          <a v-if="savedArticles.includes(serializeItem(item))" href="#" @click.prevent="removeArticle(savedArticles.indexOf(serializeItem(item)))">Unsave Article</a>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.feed-container {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  max-width: 80%;
  text-align: center;
  flex-direction: column;
  margin: 100px;
}

.feed-card {
  background: #f9f9f9;
  padding: 15px;
  border-radius: 5px;
  margin-top: 20px;
  width: 100%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

h2 {
  color: black;
}

h3 {
  margin-top: 10px;
  color: black;
}

p {
  color: #444;
}

a {
  display: inline-block;
  margin-top: 10px;
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

.error {
  color: red;
  font-weight: bold;
}
</style>
