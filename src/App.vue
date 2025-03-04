<script setup lang="ts">
import { ref } from 'vue';

import NavigationHeader from './components/NavigationHeader.vue';
import NewFeedForm from './components/NewFeedForm.vue';
// @ts-ignore
import FeedList from './components/FeedList.vue';
// @ts-ignore
import FeedView from './components/FeedView.vue';

const activeTab = ref("AddFeed");
const RSSView = ref('');

const changeTab = (tab: string) => {
  activeTab.value = tab;
}

const viewFeed = (RSS: string) => {
  activeTab.value = "FeedDetail";
  RSSView.value = RSS;
}
</script>

<template>
  <div class="container">
    <NavigationHeader class="header" @changeTab = "changeTab"/>
    <div v-if="activeTab == 'AddFeed'" class="content">
      <NewFeedForm />
    </div>
    <div v-if="activeTab == 'ListFeeds'" class="content">
      <FeedList @viewFeed = "viewFeed" />
    </div>
    <div v-if="activeTab == 'FeedDetail'" class="content">
      <FeedView :feedUrl=RSSView class="content"/>
    </div>
  </div>
</template>

<style scoped>
/* Main container takes full height */
.container {
  background: #f4f4f4;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh; /* Full viewport height */
  width: 100%; /* Stretches across the screen */
  margin: 0;
  padding: 0;
}

/* Header stays at the top */
.header {
  width: 100%;
  background: #333;
  padding: 15px 0;
  text-align: center;
}

/* Content area: centers the form */
.content {
  flex-grow: 1; /* Pushes form to the center */
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
  width: 100%;
}
</style>
