<script setup>
import SearchForm from './SearchForm.vue';
import { ref, watchEffect } from 'vue';

const q = ref('');

watchEffect(() => {});
</script>

<template>
  <nav class="navbar navbar-light bg-white navbar-expand-lg border-bottom fixed-top">
    <div class="container">
      <a class="navbar-brand" href="/"><strong>Vue</strong> Cloud</a>

      <div id="search-form"></div>
    </div>
  </nav>
</template>

<style scoped></style>
