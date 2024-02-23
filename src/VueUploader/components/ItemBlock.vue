<template>
  <div class="col-md-3">
    <div class="item-block">
      <div class="toolbox">
        <span class="drag-icon" title="Reposition file">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="24px" height="24px">
            <path
              d="M1 4h2v2H1V4zm4 0h14v2H5V4zM1 9h2v2H1V9zm4 0h14v2H5V9zm-4 5h2v2H1v-2zm4 0h14v2H5v-2z"
            />
          </svg>
        </span>

        <span class="delete-icon" title="Remove file" @click="doRemoveFile">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="24px" height="24px">
            <path
              d="M10 8.586L2.929 1.515 1.515 2.929 8.586 10l-7.071 7.071 1.414 1.414L10 11.414l7.071 7.071 1.414-1.414L11.414 10l7.071-7.071-1.414-1.414L10 8.586z"
            />
          </svg>
        </span>
      </div>

      <div v-if="canBeShowInPreviewMode">
        <img class="img-responsive" :src="imageURL" draggable="false" />
      </div>
      <div v-else style="padding-top: 10px">
        <div class="file-icon">
          <svg
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            x="0px"
            y="0px"
            viewBox="0 0 317.001 317.001"
            style="enable-background: new 0 0 317.001 317.001"
            xml:space="preserve"
            width="48px"
            height="48px"
          >
            <path
              d="M270.825,70.55L212.17,3.66C210.13,1.334,207.187,0,204.093,0H55.941C49.076,0,43.51,5.566,43.51,12.431V304.57
                            c0,6.866,5.566,12.431,12.431,12.431h205.118c6.866,0,12.432-5.566,12.432-12.432V77.633
                            C273.491,75.027,272.544,72.51,270.825,70.55z M55.941,305.073V12.432H199.94v63.601c0,3.431,2.78,6.216,6.216,6.216h54.903
                            l0.006,222.824H55.941z"
            />
          </svg>
        </div>
        <p class="text-file-name">Filename: {{ fileName }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
// import {UploadedFile} from "../classes/uploaded-file.class";
//     import {AjaxService} from './AjaxService.js';

const emit = defineEmits(['delete']);

const imageExtensions = ['png', 'jpg', 'jpeg', 'gif', 'svg'];
const props = defineProps({
  fileInfo: {
    type: Object,
    required: true,
  },
  fileIndex: Number,
  endpoint: { type: Object, default: {} },
});

const canBeShowInPreviewMode = computed(() => {
    console.log(props.fileInfo)
  const extension = props.fileInfo.name.substr(props.fileInfo.name.lastIndexOf('.') + 1);
  return imageExtensions.indexOf(extension) >= 0;
});

const fileName = computed(() => {
  return props.fileInfo.name.substr(props.fileInfo.name.lastIndexOf('/') + 1);
});
const imageURL = computed(() => {
  return URL.createObjectURL(props.fileInfo);
});

const doRemoveFile = () => {
  if (!confirm('Are you sure you want to delete this file?')) {
    return;
  }

  // Delete-Enpoint not empty => Run it
  // if (props.endpoint.delete !== "") {
  //     AjaxService.delete(
  //         `${props.endpoint.delete}/${props.fileInfo.fileId}`,
  //         removeFile,
  //         null
  //     )
  //     return
  // }

  // DELETE
  removeFile();
};

const removeFile = () => {
  emit('delete', props.fileIndex);
};
</script>

<style scoped></style>
