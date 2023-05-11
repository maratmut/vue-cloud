<script setup>
import FileItem from './FileItem.vue';
import { reactive } from 'vue';

const props = defineProps({
  files: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['select-change']);

const selectedItems = reactive(new Set());

const selectOne = (item) => {
  selectedItems.clear();
  selectedItems.add(item);
  emit('select-change', selectedItems);
};

const selectMultiple = (item) => {
  if (selectedItems.has(item)) {
    selectedItems.delete(item);
  } else {
    selectedItems.add(item);
  }
  emit('select-change', selectedItems);
};

const isSelected = (item) => selectedItems.has(item);
</script>

<template>
  <div class="row">
    <FileItem
      v-for="file in files"
      :key="`file-${file.id}`"
      :file="file"
      @click.exact="selectOne(file)"
      @click.ctrl.exact="selectMultiple(file)"
      :class="{ 'selected-file': isSelected(file) }"
    />
  </div>
</template>
