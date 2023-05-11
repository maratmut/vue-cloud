<script setup>
import ActionBar from '../components/ActionBar.vue';
import filesApi from '../api/files';
import FilesList from '../components/files/FilesList.vue';
import { ref, reactive, watchEffect, toRef } from 'vue';
import SortToggler from '../components/SortToggler.vue';
import SearchForm from '../components/SearchForm.vue';

const files = ref([]);
const toast = reactive({
  show: false,
  message: '',
});

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

const removeItem = async (item, files) => {
  try {
    const response = await filesApi.delete(item.id);
    if (response.status === 200 || response.status === 204) {
      const index = files.value.findIndex((file) => file.id === item.id);
      files.value.splice(index, 1);
    }
  } catch (error) {
    console.error(error);
  }
};

const handleRemove = () => {
  if (confirm('Вы уверены что хотите удалить?')) {
    selectedItems.value.forEach((item) => removeItem(item, files));
    selectedItems.value.splice(0);
    toast.show = true;
    toast.message = 'Успешно удалено';
  }
};

watchEffect(async () => {
  files.value = await fetchFiles(query);
});
</script>
<template>
  <div class="container py-3">
    <ActionBar :selected-count="selectedItems.length" @remove="handleRemove" />

    <div class="d-flex justify-content-between align-items-center py-2">
      <h6 class="text-muted mb-0">Файлы</h6>
      <SortToggler @sort-change="handleSortChange($event)" />
    </div>
    <teleport to="#search-form">
      <SearchForm v-model="q" />
    </teleport>
    <FilesList :files="files" @select-change="handleSelectChange($event)" />
    <app-toast
      :show="toast.show"
      :message="toast.message"
      type="Отлично"
      position="bottom-left"
      @hide="toast.show = false"
    ></app-toast>
  </div>
</template>

<style scoped></style>
