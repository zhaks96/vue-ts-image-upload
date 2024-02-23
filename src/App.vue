<template>
  <div id="app">
    <ImageUpload />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ImageUpload from './components/ImageUpload.vue';

export default defineComponent({
  components: {
    ImageUpload,
  },
});
</script>

<style>
/* Add your styles here */
</style>
