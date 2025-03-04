
<script setup>
import { reactive } from "vue";

const formData = reactive({
  feedUrl: "",
  feedTitle: "",
  description: ""
});

// Submit function (prevents refresh)
const submitForm = () => {
    let feedList = JSON.parse(localStorage.getItem("feedList")  || "[]");
    feedList.push(formData);
    localStorage.setItem("feedList", JSON.stringify(feedList));
  alert("Feed Added! (Data is saved in Local Storage)");
  clearForm();
};

// Clear form & Local Storage
const clearForm = () => {
  formData.feedUrl = "";
  formData.feedTitle = "";
  formData.description = "";
};
</script>

<template>
  <div class="form-container">
    <h2>Add a New RSS Feed</h2>
    <form @submit.prevent="submitForm">
      <label for="feed-url">RSS Feed URL:</label>
      <input v-model="formData.feedUrl" type="url" id="feed-url" required />

      <label for="feed-title">Title:</label>
      <input v-model="formData.feedTitle" type="text" id="feed-title" required />

      <label for="description">Description:</label>
      <textarea v-model="formData.description" id="description" rows="4"></textarea>

      <button type="submit">Add Feed</button>
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