<script setup lang="ts">
import { ref } from 'vue';

import NavigationHeader from './components/NavigationHeader.vue';
import NewFeedForm from './components/NewFeedForm.vue';
// @ts-ignore
import FeedList from './components/FeedList.vue';
// @ts-ignore
import FeedView from './components/FeedView.vue';
// @ts-ignore
import EditFeedForm from './components/EditFeedForm.vue';
// @ts-ignore
import SavedList from './components/SavedList.vue';

const activeTab = ref("AddFeed");
const RSSView = ref('');
const editIndex = ref(0);

const changeTab = (tab: string) => {
  activeTab.value = tab;
}

const viewFeed = (RSS: string) => {
  activeTab.value = "FeedDetail";
  RSSView.value = RSS;
}

const editFeed = (index: number) => {
  activeTab.value = "EditFeed";
  editIndex.value = index;
}

const feedEdited = () => {
  activeTab.value = "ListFeeds";
}
</script>

<template>
  <div class="container">
    <NavigationHeader class="header" @changeTab="changeTab" />
    
    <div v-if="activeTab == 'AddFeed'" class="content">
      <NewFeedForm />
    </div>

    <div v-if="activeTab == 'EditFeed'" class="content">
      <EditFeedForm :index="editIndex" @feedEdited="feedEdited" />
    </div>

    <div v-if="activeTab == 'ListFeeds'" class="content">
      <FeedList @viewFeed="viewFeed" @editFeed="editFeed" />
    </div>

    <div v-if="activeTab == 'FeedDetail'" class="news-container">
      <FeedView :feedUrl="RSSView" />
    </div>
    <div v-if="activeTab == 'Preferences'" class="content">
      <SavedList class="content"/>
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

/* Content area */
.content {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

/* News feed container */
.news-container {
  width: 90%;
  max-width: 1200px;
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
