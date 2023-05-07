<script setup>
import ActionBar from '../components/ActionBar.vue';
import filesApi from '../api/files';
import FilesList from '../components/files/FilesList.vue';
import {ref, reactive, watchEffect } from 'vue';
import SortToggler from '../components/SortToggler.vue';

const files = ref([]);

const query = reactive({
  _sort: 'name',
  _order: 'asc',
});

const handleSortChange = (payload) => {
  query._sort = payload.column;
  query._order = payload.order;
};

const fetchFiles = async (query) => {
  try {
    const { data } = await filesApi.index(query);
    return data;
  } catch (error) {
    console.error(error);
  }
};

watchEffect(async () => {
  files.value = await fetchFiles(query);
});
</script>
<template>
  <div class="container py-3">
    <ActionBar />

    <div class="d-flex justify-content-between align-items-center py-2">
      <h6 class="text-muted mb-0">Files</h6>
      <SortToggler @sort-change="handleSortChange($event)" />
    </div>
    <FilesList :files="files" />
  </div>
</template>

<style scoped></style>
