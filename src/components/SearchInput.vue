<template>
  <div class="input-container">
    <label>{{ label }}</label>
    <div class="input-wrapper">
      <input v-model="innerValue" v-bind="$attrs" />
      <button @click="clearInput">Clear</button>
    </div>
    <p v-if="countSearch > 0">
      Количество символов: {{ countSearch }}
      <span v-if="isLimit" style="color: red;">Количество слов не должно превышать 3</span>
    </p>
  </div>
</template>

<script setup>
import { ref, computed, defineProps, defineEmits, watch } from 'vue';

const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  },
  label: {
    type: String,
    required: true
  }
});

const emit = defineEmits(['clear']);

const wordsLimit = 3;
const isLimit = ref(false);
const innerValue = ref(props.modelValue);

const checkWordsCount = (value) => {
  const wordsCount = value?.trim().split(/\s+/).length;
  isLimit.value = wordsCount > wordsLimit;
};

const countSearch = computed(() => {
  if (innerValue.value) {
    return innerValue.value.length;
  }
  return 0;
});

watch(innerValue, (value) => {
  checkWordsCount(value);
});

const clearInput = () => {
  innerValue.value = '';
  emit('clear');
};
</script>

<style scoped>
.input-wrapper {
  position: relative;
}

input {
  padding: 5px;
}

button {
  position: absolute;
  left: 200px;
  top: 3px;
  padding: 5px;
}
</style>