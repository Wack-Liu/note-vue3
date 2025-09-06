<script setup>
import NoteList from './components/NoteList.vue'
</script>

<template>
  <div class="app">
    <NoteList />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #fafafa;
  min-height: 100vh;
}

.app {
  min-height: 100vh;
  padding: 2rem 0;
}
</style>
