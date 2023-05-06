<script setup>
import FileItem from './FileItem.vue';

const props = defineProps({
  files: {
    type: Array,
    required: true,
  },
});

</script>

<template>
  <div class="row">
    <FileItem v-for="file in files" :key="`file-${file.id}`" :file="file" />
  </div>
</template>
