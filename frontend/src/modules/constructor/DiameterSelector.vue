<template>
  <div class="content__diameter">
    <div class="sheet">
      <h2 class="title title--small sheet__title">Выберите размер</h2>
      <div class="sheet__content diameter">
        <label
            v-for="item in sizeItems"
            :key="item.id"
            class="diameter__input"
            :class="'diameter__input--' + item.value"
        >
          <input
              type="radio"
              name="diameter"
              class="visually-hidden"
              :value="item.value"
              :checked="item.value === modelValue"
              @change="emit('update:modelValue', item.value)"
          >
          <span>{{ item.name }}</span>
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  sizeItems: {
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
