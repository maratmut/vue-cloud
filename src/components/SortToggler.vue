<script setup>
import { reactive } from 'vue';

const emit = defineEmits(['sort-change']);

const sort = reactive({
  column: 'name',
  order: 'asc',
});

const toggle = () => {
  sort.order = sort.order === 'asc' ? 'desc' : 'asc';
  emit('sort-change', sort);
};

</script>
<template>
  <button @click="toggle" class="rounded-button">
    <icon-arrow-up v-if="sort.order === 'asc'" />
    <icon-arrow-down v-else />
  </button>
</template>
