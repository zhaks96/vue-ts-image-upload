<template>
  <div class="vue-uploader">
    <VueDraggableNext
      class="dragArea list-group w-full"
      :list="value"
      animation="600"
      @start="drag = true"
      @end="drag = false"
    >
      <ItemBlock
        v-for="(fileInfo, fileIndex) in value"
        :key="fileIndex"
        :file-info="fileInfo"
        :file-index="fileIndex"
        @delete="doDeleteFile"
      />

      <!-- <ItemUploadingBlock
        v-if="uploadingFile.length > 0"
        v-for="(fileInfo, fileIndex) in uploadingFile"
        :key="fileIndex"
        :file="fileInfo"
      /> -->
    </VueDraggableNext>

    <UploadBlock
      :accepted-extensions="acceptedExtensions"
      :file-rules="fileRules"
      :endpoint="endpoint"
      :limit="limit"
      :total-files="value.length"
      @uploading="handleUploadingFile"
      @uploaded="handleUploadedFile"
      @upload-failed="handleUploadFailed"
    />

    <div v-if="formMode">
      <input
        v-for="fileInfo in value"
        :key="fileInfo.fileId"
        type="hidden"
        name="files[]"
        :value="fileInfo.fileId"
      />

      <input
        v-for="fileInfo in value"
        :key="fileInfo.fileURL"
        type="hidden"
        name="urls[]"
        :value="fileInfo.fileURL"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import ItemBlock from './ItemBlock.vue';
import UploadBlock from './UploadBlock.vue';
import { VueDraggableNext } from 'vue-draggable-next';
// import ItemUploadingBlock from './ItemUploadingBlock.vue';
import { ref, watch } from 'vue';

const props = defineProps({
  limit: {
    type: Number,
    default: 0, // 0 => unlimited
  },
  formMode: {
    type: Boolean,
    default: false,
  },
  fileRules: {
    type: String,
    default: 'image/jpg, image/jpeg, image/png',
  },
  acceptedExtensions: {
    type: Array,
    default: () => ['jpg', 'png', 'jpeg'],
  },
  endpoint: {
    type: Object,
    default: () => ({
      upload: 'http://localhost:3000/api/upload',
      delete: '',
    }),
  },
  value: {
    type: Array<any>,
    default: () => [],
  },
});

const emit = defineEmits(['update:value', 'updateFiles']);

const uploadingFile = ref<any>([]);
const drag = ref(false);

watch(
  () => props.value,
  (val) => {
    console.log(val)
    if (!val || !val.forEach) {
      val = [];
    }
    // setValue(val);
  },
);
const doDeleteFile = (fileIndex: any) => {
  props.value.splice(fileIndex, 1);
  // setValue(props.value);
};

const setValue = (val: any) => {
  emit('update:value', val);
};

const handleUploadingFile = (fileInfo: any) => {
  uploadingFile.value.push(fileInfo);
  emit('updateFiles', uploadingFile.value)
};

const handleUploadedFile = (fileInfo: any) => {
  uploadingFile.value = [];
  props.value.push(fileInfo);
  setValue(props.value);
};

const handleUploadFailed = () => {
  uploadingFile.value = [];
};
</script>

<style>
.dragArea {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
</style>
