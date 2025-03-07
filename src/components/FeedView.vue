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
    feedItems.value = Array.from(items).map(item => {
      return {
        title: item.querySelector("title")?.textContent || "Sans titre",
        pubDate: item.querySelector("pubDate")?.textContent || "",
        description: item.querySelector("description")?.textContent || "",
        link: item.querySelector("link")?.textContent || "#",
        image: item.querySelector("image")?.textContent || 
               item.querySelector("enclosure")?.getAttribute("url") || ""
      };
    });

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

// Fetch the feed when the component mounts or if the URL changes
onMounted(fetchRSSFeed);
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
      <div v-for="(item, index) in feedItems" :key="index" class="news-item">
        <div v-if="feedCount == 'All' || index < feedCount">
          <h3>{{ item.title }}</h3>
          <p>{{ item.pubDate }}</p>
          <p>{{ item.description }}</p>
          <a :href="item.link" target="_blank">View Article</a>
          <img v-if="item.image" :src="item.image" alt="News Image" class="news-image" />
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

.news-item {
  background: #fff;
  padding: 15px;
  margin-bottom: 15px;
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
</style>
