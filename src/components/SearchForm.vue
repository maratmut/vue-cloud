<script setup>
const props = defineProps({
  modelValue: {
    type: String,
    required: true,
  },
});

const emit = defineEmits('update:modelValue');

const handleEnterKey = (event) => {
  emit('update:modelValue', event.target.value);
};

const handleEscapeKey = () => {
  emit('update:modelValue', '');
};
</script>

<template>
  <form class="form-inline mt-2 mt-md-0">
    <input
      class="form-control"
      type="text"
      :value="modelValue"
      @keydown.enter.prevent="handleEnterKey"
      @keydown.esc.prevent="handleEscapeKey"
      placeholder="поиск файлов"
      aria-label="Search"
    />
  </form>
</template>
