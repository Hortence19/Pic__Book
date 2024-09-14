<script setup lang="ts">
 const { images } = defineProps<{
    images: Array<any>;
  }>();   
</script>

<template>
    <component v-for="(imageComponent, index) in images" :is="imageComponent" :key="index"/>
</template>

<style scoped>
.album {
  width: 1000px;
  display: flex;
}
</style>