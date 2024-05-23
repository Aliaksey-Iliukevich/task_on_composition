<template>
  <div>
    <SearchInput 
      label="Input value:" 
      v-model="search"
      class="class-1 class-2"
      placeholder="Найти..."
      @clearInput="handleClearInput"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import SearchInput from './components/SearchInput.vue';

const search = ref('');
function handleClearInput() {
  console.log('Инпут очищен!');
}
</script>
