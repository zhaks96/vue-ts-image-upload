<template>
  <div class="upload-block" @click="doChooseFile" title="Click here to upload">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24px"
      height="24px"
      fill="#000"
      viewBox="0 0 20 20"
    >
      <path d="M13 10v6H7v-6H2l8-8 8 8h-5zM0 18h20v2H0v-2z" />
    </svg>

    <p class="text-center">Click here to upload</p>

    <input
      type="file"
      v-show="false"
      id="vueUploadField"
      :accept="fileRules"
      @change="uploadSelectedFile"
    />
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue';
// import { UploadedFile } from '../classes/uploaded-file.class';
// import { AjaxService } from './AjaxService.js';

const props = defineProps({
  fileRules: String,
  acceptedExtensions: { type: Array, default: [] },
  endpoint: { type: Object, default: {} },
  limit: { type: Number, default: 5 },
  totalFiles: { type: Number, default: 0 },
});

const emit = defineEmits(['uploading', 'uploaded', 'upload-failed']);

const uploaderDOM = ref<any>(null);
const isUpload = ref(false);

onMounted(() => {
  uploaderDOM.value = document.getElementById('vueUploadField');
});

const doChooseFile = () => {
  if (props.limit > 0 && props.limit <= props.totalFiles) {
    alert("Limit reached. Can't upload more.");
    return;
  }

  if (isUpload.value) {
    alert("There's an upload process running. Please wait...");
    return;
  }

  uploaderDOM.value.click();
};

const uploadSelectedFile = (e: any) => {
  const file = e.target.files.item(0);
  const fileExtension = file.name.substr(file.name.lastIndexOf('.') + 1);

  if (props.acceptedExtensions.indexOf(fileExtension) < 0) {
    return false
  }

  emit('uploading', file);
  // isUpload.value = true

//   const uploadData = new FormData();
//   uploadData.append('file', file);
//   console.log(uploadData);
};

// const clearFile = () => {
//   uploaderDOM.value.value = null;
// };
</script>
