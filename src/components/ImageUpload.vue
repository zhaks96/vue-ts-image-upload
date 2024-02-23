<template>
  <div>
    <vue-uploader :value="myFiles" @updateFiles="updateFiles"></vue-uploader>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import VueUploader from '../VueUploader/components/VueUploader.vue';

const myFiles = ref<any>([]);

const updateFiles = (files: any) => {
  myFiles.value = files
}

// const handleFileChange = async (event: any) => {
//       const files = event.target.files;
//       console.log(files)
//       const images2 = [] as Array<{ url: string }>
//       for (let i = 0; i < files.length; i++) {
//         const reader = new FileReader();
//         reader.onload = (e) => {
//           images2.push({ url: e.target?.result as string });
//         };
//         await reader.readAsDataURL(files[i]);
//       }
//       images.value = images2
//     }

// const handleDragEnd = () => {
//       // Handle drag end event
//     }
</script>

<style scoped>
/* Add your styles here */
.image-item {
  margin: 5px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
