<template>
  <div class="note-container">
    <div class="note-header">
      <h1>我的备忘录 v1.0</h1>
      <div class="add-note-btn" @click="showAddModal = true">
        <span>+</span>
      </div>
    </div>

    <div class="notes-grid">
      <div v-for="note in notes" 
           :key="note.id" 
           class="note-card"
           :class="{ 'note-completed': note.completed }"
           @click="toggleComplete(note.id)">
        <div class="note-content">
          <h3 v-if="note.title && note.title !== '无标题'" class="note-title">{{ note.title }}</h3>
          <p>{{ note.text }}</p>
          <span class="note-date">{{ note.date }}</span>
        </div>
        <button class="delete-btn" @click.stop="deleteNote(note.id)">×</button>
      </div>
    </div>
  </div>
  
  <!-- 添加备忘录模态框 -->
  <div v-if="showAddModal" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <h2>添加新备忘录</h2>
      <div class="form-group">
        <label for="note-title">标题</label>
        <input 
          type="text" 
          id="note-title"
          v-model="newNote.title"
          placeholder="输入标题（可选）"
        />
      </div>
      <div class="form-group">
        <label for="note-content">内容</label>
        <textarea 
          id="note-content" 
          v-model="newNote.content"
          placeholder="输入备忘录内容..."
        ></textarea>
      </div>
      <div class="modal-actions">
        <button class="btn-cancel" @click="closeModal">取消</button>
        <button class="btn-save" @click="saveNote" :disabled="!newNote.content.trim()">保存</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const showAddModal = ref(false)
const notes = ref([])
const newNote = ref({
  title: '',
  content: ''
})

const saveNote = () => {
  if (newNote.value.content.trim()) {
    notes.value.unshift({
      id: Date.now(),
      title: newNote.value.title || '无标题',
      text: newNote.value.content,
      date: new Date().toLocaleDateString(),
      completed: false
    })
    closeModal()
  }
}

const closeModal = () => {
  showAddModal.value = false
  newNote.value = {
    title: '',
    content: ''
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
  width: 390px;
  height: 624px;
  margin: 0 auto;
  padding: 1rem;
  background-color: #ffffff;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow-y: auto;
  border-radius: 20px;
}

/* 使用固定宽度的滚动条，避免占用布局空间 */
.note-container::-webkit-scrollbar {
  width: 0px;
  background: transparent;
}

.note-container {
  scrollbar-width: none;
}

.notes-grid {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1.5rem;
  padding-bottom: 1rem;
  height: 450px;
  overflow-y: auto;
  overflow-x: hidden;
  padding-right: 0;
  /* 添加自定义滚动条 */
  scrollbar-gutter: stable;
}

/* 自定义滚动条样式 */
.notes-grid::-webkit-scrollbar {
  width: 8px;
}

.notes-grid::-webkit-scrollbar-track {
  background: transparent;
  border-radius: 4px;
}

.notes-grid::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  transition: all 0.3s ease;
}

.notes-grid::-webkit-scrollbar-thumb:hover {
  background-color: rgba(0, 0, 0, 0.4);
}

/* Firefox scrollbar */
.notes-grid {
  scrollbar-width: thin;
  scrollbar-color: rgba(0, 0, 0, 0.2) transparent;
}

.note-header {
  text-align: center;
  margin-bottom: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.note-header h1 {
  color: #ff9800;
  font-size: 1.8rem;
  margin: 0;
  flex: 1;
  text-align: center;
}

.add-note-btn {
  width: 40px;
  height: 40px;
  background-color: #ff9800;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1.8rem;
  color: white;
  flex-shrink: 0;
  margin-left: auto;
}

.add-note-btn:hover {
  background-color: #f57c00;
  transform: scale(1.1);
}

.note-card {
  background-color: white;
  border-radius: 16px;
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

.note-title {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
  color: #333;
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

/* 模态框样式 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 2rem;
  border-radius: 16px;
  width: 90%;
  max-width: 400px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  position: relative;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #555;
}

input, textarea {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  resize: vertical;
  min-height: 40px;
  box-sizing: border-box;
}

textarea {
  min-height: 100px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 2rem;
}

.btn-cancel {
  background-color: #e0e0e0;
  color: #333;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-cancel:hover {
  background-color: #d0d0d0;
}

.btn-save {
  background-color: #ff9800;
  color: white;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-save:hover:not(:disabled) {
  background-color: #f57c00;
}

.btn-save:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>