<template>
  <div class="input-container">
    <label>{{ label }}</label>
    <div class="input-wrapper">
      <input 
        v-model="innerValue"
        v-bind="$attrs"
      />
      <button @click="clearInput">Очистить</button>
    </div>
    <p v-if="countSearch > 0">Количество символов: {{ countSearch }}
      <span v-if="isLimit" style="color: red;">Количество слов не должно превышать 3</span>
    </p>
  </div>
</template>

<script>
import { ref, watch, computed } from 'vue';

export default {
  props: {
    label: String,
    modelValue: String,
  },
  emits: ['update:modelValue', 'clear'],
  setup(props, { emit }) {
    const wordsLimit = 3;
    const isLimit = ref(false);
    const innerValue = ref(props.modelValue);

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

    const checkWordsCount = (value) => {
      const wordsCount = value?.trim().split(/\s+/).length;
      isLimit.value = wordsCount > wordsLimit;
    };

    return {
      innerValue,
      countSearch,
      isLimit,
      clearInput
    };
  }
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