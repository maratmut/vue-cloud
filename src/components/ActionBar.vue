<script setup>
const props = defineProps({
  selectedCount: {
    type: Number,
    default: 0,
  },
});

const emit = defineEmits(['rename, remove']);

</script>

<template>
  <div class="d-flex justify-content-between align-items-center border-bottom pt-2 pb-3">
    <button class="btn btn-outline-primary"><icon-upload /> Загрузить</button>

    <div class="action-buttons" v-show="selectedCount">
      <button
        type="button"
        class="rounded-button"
        title="Rename"
        v-show="selectedCount === 1"
        @click="$emit('rename')"
      >
        <icon-pencil />
      </button>
      <button type="button" class="rounded-button" title="Remove selected" @click="$emit('remove')">
        <icon-trash />
      </button>
    </div>
  </div>
</template>

<style scoped></style>
