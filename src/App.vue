<script setup lang="ts">
import NavigationHeader from './components/NavigationHeader.vue';
import NewFeedForm from './components/NewFeedForm.vue';
</script>

<template>
  <div class="container">
    <NavigationHeader class="header" />

    <div class="content">
      <NewFeedForm />
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
  height: 100vh; /* Full viewport height */
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
