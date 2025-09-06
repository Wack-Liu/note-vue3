<template>
  <div class="add-note-overlay" @click="closeModal">
    <div class="add-note-modal" @click.stop>
      <div class="modal-header">
        <h2>添加备忘录</h2>
        <button class="close-btn" @click="closeModal">×</button>
      </div>
      
      <div class="modal-body">
        <div class="input-group">
          <label for="noteTitle">标题（可选）</label>
          <input 
            id="noteTitle"
            v-model="noteTitle" 
            placeholder="输入备忘录标题..." 
            type="text"
            class="title-input"
          />
        </div>
        
        <div class="input-group">
          <label for="noteContent">正文内容 *</label>
          <textarea
            id="noteContent"
            v-model="noteContent"
            placeholder="输入备忘录内容..."
            class="content-textarea"
            rows="6"
          ></textarea>
        </div>
      </div>
      
      <div class="modal-footer">
        <button class="cancel-btn" @click="closeModal">取消</button>
        <button 
          class="save-btn" 
          @click="saveNote" 
          :disabled="!noteContent.trim()"
        >
          保存
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const noteTitle = ref('')
const noteContent = ref('')

const emit = defineEmits(['save', 'close'])

const saveNote = () => {
  if (noteContent.value.trim()) {
    emit('save', {
      title: noteTitle.value.trim() || '无标题',
      content: noteContent.value.trim()
    })
    closeModal()
  }
}

const closeModal = () => {
  noteTitle.value = ''
  noteContent.value = ''
  emit('close')
}
</script>

<style scoped>
.add-note-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.add-note-modal {
  width: 90%;
  max-width: 500px;
  background-color: white;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  overflow: hidden;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  border-bottom: 1px solid #eee;
}

.modal-header h2 {
  margin: 0;
  color: #ff9800;
  font-size: 1.5rem;
}

.close-btn {
  background: none;
  border: none;
  font-size: 1.8rem;
  cursor: pointer;
  color: #888;
  padding: 0;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.close-btn:hover {
  background-color: #f1f1f1;
  color: #333;
}

.modal-body {
  padding: 1.5rem;
}

.input-group {
  margin-bottom: 1.5rem;
}

.input-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: #333;
}

.title-input,
.content-textarea {
  width: 100%;
  padding: 0.8rem;
  border: 2px solid #ddd;
  border-radius: 12px;
  font-size: 1rem;
  font-family: inherit;
  outline: none;
  transition: all 0.3s ease;
  box-sizing: border-box;
}

.title-input:focus,
.content-textarea:focus {
  border-color: #ff9800;
  box-shadow: 0 0 0 2px rgba(255, 152, 0, 0.2);
}

.content-textarea {
  resize: vertical;
  min-height: 120px;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  padding: 1.5rem;
  border-top: 1px solid #eee;
}

.cancel-btn,
.save-btn {
  padding: 0.8rem 1.5rem;
  border-radius: 12px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  border: none;
}

.cancel-btn {
  background-color: #f1f1f1;
  color: #333;
}

.cancel-btn:hover {
  background-color: #e0e0e0;
}

.save-btn {
  background-color: #ff9800;
  color: white;
}

.save-btn:hover:not(:disabled) {
  background-color: #f57c00;
  transform: translateY(-2px);
}

.save-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
  transform: none;
}
</style>