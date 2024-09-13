<template>
  <header>
    <h1>Pic__Book</h1>
  </header>
  
  <main>
    <div class="album-container">
      <album :images="imageComponents" />
    </div>
    
    <div class="file-upload">
      <label for="fileInput" class="file-upload-btn">Choisir des images</label>
      <input ref="fileInput" id="fileInput" type="file" multiple @change="onFileChange" accept="image/*" />
    </div>

    <div v-if="imageGroups.length > 0">
      <h3>Images sélectionnées :</h3>
      <div class="album-container"> 
        <div v-for="(group, groupIndex) in imageGroups" :key="groupIndex" class="image-row">
          <div v-for="(image, index) in group" :key="index" class="image-item">
            <img :src="image" alt="Image sélectionnée" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue';

import album from './components/album.vue';
import img1 from './components/img1.vue';
import img2 from './components/img2.vue';
import img3 from './components/img3.vue';
import img4 from './components/img4.vue';
import img5 from './components/img5.vue';
import img6 from './components/img6.vue';

const imageComponents = [img1, img2, img3, img4, img5, img6];

const maxImagesPerGroup = 6;  
const imageGroups = ref<string[][]>([]);  
const fileInput = ref<HTMLInputElement | null>(null);  

const onFileChange = (event: Event) => {
  const target = event.target as HTMLInputElement;
  const files = target.files;

  if (files) {
    const newImages: string[] = [];

    Array.from(files).forEach(file => {
      if (newImages.length < maxImagesPerGroup) {
        const reader = new FileReader();
        reader.onload = (e: ProgressEvent<FileReader>) => {
          if (newImages.length < maxImagesPerGroup) {
            newImages.push(e.target?.result as string);
          }
        };
        reader.readAsDataURL(file);
      }
    });

    setTimeout(() => {
      if (newImages.length === maxImagesPerGroup) {
        imageGroups.value.push(newImages);
      } else {
        alert(`Veuillez sélectionner exactement ${maxImagesPerGroup} images.`);
        fileInput.value?.click();  
      }
    }, 500);  
  }
};
</script>

<style scoped>
header {
  background-color: #f8b400;
  padding: 20px;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

header h1 {
  font-size: 2.5rem;
  color: white;
  margin: 0;
  font-family: 'Arial', sans-serif;
}

.album-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 15px;
  padding: 20px;
}

.selected-images-container {
  display: flex;
  justify-content: center;
  padding: 20px;
}

.file-upload {
  margin: 20px auto;
  text-align: center;
}

.file-upload-btn {
  background-color: #4CAF50;
  color: white;
  padding: 12px 25px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
  text-transform: uppercase;
}

.file-upload-btn:hover {
  background-color: #45a049;
}

input[type="file"] {
  display: none;
}

.image-row {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;
}

.image-item {
  flex: 0 0 150px;
  overflow: hidden;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  height: 150px;
}

.image-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.image-item img:hover {
  transform: scale(1.05);
}

@media (max-width: 768px) {
  .image-item {
    flex: 0 0 100px;
    height: 100px;
  }

  .file-upload-btn {
    padding: 10px 20px;
    font-size: 14px;
  }
}
</style>
