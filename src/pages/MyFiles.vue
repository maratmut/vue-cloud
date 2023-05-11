<script setup>
import ActionBar from '../components/ActionBar.vue';
import filesApi from '../api/files';
import FilesList from '../components/files/FilesList.vue';
import { ref, reactive, watchEffect, toRef } from 'vue';
import SortToggler from '../components/SortToggler.vue';
import SearchForm from '../components/SearchForm.vue';

const files = ref([]);

const query = reactive({
  _sort: 'name',
  _order: 'asc',
  q: '',
});

const q = toRef(query, 'q');

const handleSortChange = (payload) => {
  query._sort = payload.column;
  query._order = payload.order;
};

const selectedItems = ref([]);

const handleSelectChange = (items) => {
  selectedItems.value = Array.from(items);
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
    <ActionBar :selected-count="selectedItems.length" />

    <div class="d-flex justify-content-between align-items-center py-2">
      <h6 class="text-muted mb-0">Файлы</h6>
      <SortToggler @sort-change="handleSortChange($event)" />
    </div>
    <teleport to="#search-form">
      <SearchForm v-model="q" />
    </teleport>
    <FilesList :files="files" @select-change="handleSelectChange($event)" />
  </div>
</template>

<style scoped></style>
