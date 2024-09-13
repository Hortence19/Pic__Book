<script setup lang="ts">
    const props = defineProps({
        images: {
                 type: Array,
                required: true
            }
    });
</script>

<template>
    <component
      v-for="(imageComponent, index) in images"
      :is="imageComponent"
      :key="index"
    />
</template>

<style scoped>
.album {
  width: 1000px;
  display: flex;
}
</style>