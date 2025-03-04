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
    console.log(items[0]);
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

// Fetch the feed when the component mounts or if the URL changes
onMounted(fetchRSSFeed);
watch(() => props.feedUrl, fetchRSSFeed);
</script>

<template>
  <div class="feed-container">
    <h2>RSS Feed Information</h2>
    <div>
      <div v-if="errorMessage" class="error">{{ errorMessage }}</div>

      <div v-else>
        <h3>{{ feedTitle }}</h3>
        <p>{{ feedDescription }}</p>
        <a :href="feedLink" target="_blank">Visit Source</a>
      </div>
      <div v-for="item in feedItems">
        <h3>{{ item.querySelector("title").textContent }}</h3>
        <p>{{ item.querySelector("pubDate")?.textContent }}</p>
        <p>{{ item.querySelector("description")?.textContent }}</p>
        <a :href="item.querySelector('link').textContent" target="_blank">Visit Source</a>
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
</style>
