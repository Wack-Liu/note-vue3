<template>
  <div class="note-container">
    <div class="note-header">
      <h1>我的备忘录 v1.0</h1>
      <div class="add-note">
        <input 
          v-model="newNote" 
          @keyup.enter="addNote"
          placeholder="输入新的备忘录..."
          type="text"
        />
        <button @click="addNote" :disabled="!newNote.trim()">
          <span>+</span>
        </button>
      </div>
    </div>

    <div class="notes-grid">
      <div v-for="note in notes" 
           :key="note.id" 
           class="note-card"
           :class="{ 'note-completed': note.completed }"
           @click="toggleComplete(note.id)">
        <div class="note-content">
          <p>{{ note.text }}</p>
          <span class="note-date">{{ note.date }}</span>
        </div>
        <button class="delete-btn" @click.stop="deleteNote(note.id)">×</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newNote = ref('')
const notes = ref([])

const addNote = () => {
  if (newNote.value.trim()) {
    notes.value.unshift({
      id: Date.now(),
      text: newNote.value,
      date: new Date().toLocaleDateString(),
      completed: false
    })
    newNote.value = ''
  }
}

const deleteNote = (id) => {
  notes.value = notes.value.filter(note => note.id !== id)
}

const toggleComplete = (id) => {
  const note = notes.value.find(note => note.id === id)
  if (note) {
    note.completed = !note.completed
  }
}
</script>

<style scoped>
.note-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.note-header {
  text-align: center;
  margin-bottom: 3rem;
}

.note-header h1 {
  color: #ff9800;
  font-size: 2.5rem;
  margin-bottom: 2rem;
}

.add-note {
  display: flex;
  gap: 1rem;
  max-width: 600px;
  margin: 0 auto;
}

.add-note input {
  flex: 1;
  padding: 1rem;
  border: 2px solid #ff9800;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: all 0.3s ease;
}

.add-note input:focus {
  box-shadow: 0 0 0 2px rgba(255, 152, 0, 0.2);
}

.add-note button {
  padding: 0.5rem 1.5rem;
  background-color: #ff9800;
  border: none;
  border-radius: 8px;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.add-note button:hover {
  background-color: #f57c00;
  transform: scale(1.05);
}

.add-note button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
  transform: none;
}

.notes-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-top: 2rem;
}

.note-card {
  background-color: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  position: relative;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  justify-content: space-between;
}

.note-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.note-completed {
  background-color: #fff3e0;
}

.note-completed .note-content p {
  text-decoration: line-through;
  color: #888;
}

.note-content {
  flex: 1;
}

.note-content p {
  margin: 0;
  font-size: 1.1rem;
  line-height: 1.5;
  color: #333;
}

.note-date {
  display: block;
  margin-top: 0.5rem;
  font-size: 0.85rem;
  color: #888;
}

.delete-btn {
  background: none;
  border: none;
  color: #ff9800;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
  transition: all 0.2s ease;
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
}

.delete-btn:hover {
  background-color: #ff98001a;
  color: #f57c00;
}
</style>
