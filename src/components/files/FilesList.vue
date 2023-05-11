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

const clearSelected = () => {
  selectedItems.clear();
  emit('select-change', selectedItems);
};
</script>

<template>
  <div class="row" @click="clearSelected">
    <FileItem
      v-for="file in files"
      :key="`file-${file.id}`"
      :file="file"
      @click.exact.stop="selectOne(file)"
      @click.ctrl.exact.stop="selectMultiple(file)"
      :class="{ 'selected-file': isSelected(file) }"
    />
  </div>
</template>
