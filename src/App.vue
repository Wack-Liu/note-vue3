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
  background-color: #f5f5f5;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.app {
  min-height: 100vh;
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  width: 100%;
}
</style>
