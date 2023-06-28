<template>
  <div class="ingredients__sauce">
    <p>Основной соус:</p>
    <label class="radio ingredients__input" v-for="item in sauceItems" :key="item.id">
      <input type="radio"
             name="sauce"
             :value="item.value"
             :checked="item.value === modelValue"
             @change="emit('update:modelValue', item.value)"
      >
      <span>{{ item.name }}</span>
    </label>
  </div>
</template>

<script setup>
const props = defineProps({
  sauceItems: {
    type: Array,
    default: () => [],
  },
  modelValue: {
    type: String,
    default: "",
  }
})
const emit = defineEmits(['update:modelValue'])
</script>
