<template>
  <div class="content__dough">
    <div class="sheet">
      <h2 class="title title--small sheet__title">Выберите тесто</h2>
      <div class="sheet__content dough">
        <label
            v-for="item in doughItems"
            :key="item.id"
            :class="'dough__input dough__input--'+ item.value"
        >
          <input type="radio"
                 name="dough"
                 class="visually-hidden"
                 :value="item.value"
                 :checked="item.value === modelValue"
                 @change="emit('update:modelValue', item.value)"
          >
          <b>{{ item.name }}</b>
          <span>{{ item.description }}</span>
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  doughItems: {
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
