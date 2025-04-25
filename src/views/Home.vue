<script setup lang="ts">
import { ref } from 'vue'
import { ipcRenderer } from 'electron'

const selectedFile = ref<File | null>(null)
const filePath = ref<string>('')

const openFileDialog = async () => {
  const result = await ipcRenderer.invoke('show-open-dialog', {
    properties: ['openFile']
  })
  if (!result.canceled && result.filePaths.length > 0) {
    filePath.value = result.filePaths[0]
    // 这里可以添加文件读取逻辑
  }
}

const removeFile = () => {
  selectedFile.value = null
  filePath.value = ''
}
</script>

<template>
  <div class="home-container">
    <h1>文件上传</h1>
    <button 
      class="upload-btn" 
      @click="openFileDialog"
    >
      点击上传
    </button>
    
    <div v-if="filePath" class="file-path-container">
      <span>{{ filePath }}</span>
      <button class="delete-btn" @click="removeFile">
        ×
      </button>
    </div>
  </div>
</template>

<style scoped>
.home-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  padding: 2rem;
}

.upload-btn {
  padding: 0.5rem 1rem;
  background-color: #646cff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.file-path-container {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.delete-btn {
  background: none;
  border: none;
  color: red;
  cursor: pointer;
  font-size: 1.2rem;
}
</style>