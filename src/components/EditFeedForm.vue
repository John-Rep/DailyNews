
<script setup>
import { onMounted, reactive } from "vue";

const emit = defineEmits(['feedEdited']);
const props = defineProps({
  index: Number
})

const formData = reactive({
  feedUrl: "",
  feedTitle: "",
  description: ""
});

// Submit function (prevents refresh)
const submitForm = () => {
    let feedList = JSON.parse(localStorage.getItem("feedList")  || "[]");
    feedList.splice(props.index, 1, formData);
    localStorage.setItem("feedList", JSON.stringify(feedList));
  clearForm();
  emit('feedEdited');
};

// Clear form & Local Storage
const clearForm = () => {
  formData.feedUrl = "";
  formData.feedTitle = "";
  formData.description = "";
};

onMounted(() => {
  let data = JSON.parse(localStorage.getItem("feedList"))[props.index];
  formData.feedTitle = data.feedTitle;
  formData.feedUrl = data.feedUrl;
  formData.description = data.description;
})


</script>

<template>
  <div class="form-container">
    <h2>Edit RSS Feed</h2>
    <form @submit.prevent="submitForm">
      <label for="feed-url">RSS Feed URL:</label>
      <input v-model="formData.feedUrl" type="url" id="feed-url" required />

      <label for="feed-title">Title:</label>
      <input v-model="formData.feedTitle" type="text" id="feed-title" required />

      <label for="description">Description:</label>
      <textarea v-model="formData.description" id="description" rows="4"></textarea>

      <button type="submit">Confirm Changes</button>
      <button type="button" @click="clearForm" class="clear-btn">Clear</button>
    </form>
  </div>
</template>

<style scoped>
    .form-container {
        background: white;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        max-width: 400px;
    }
    h2 {
        text-align: center;
        color: black;
    }
    label {
        color: black;
        font-weight: bold;
        display: block;
        margin-top: 10px;
    }
    input, textarea, select {
        width: 100%;
        padding: 8px;
        margin-top: 5px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }
    button {
        background: #28a745;
        color: white;
        padding: 10px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        margin-top: 15px;
        width: 100%;
    }
    button:hover {
        background: #218838;
    }
</style>