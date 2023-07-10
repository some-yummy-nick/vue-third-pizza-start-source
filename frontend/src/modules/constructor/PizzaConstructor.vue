<template>
  <div class="content__constructor">
    <app-drop @drop="$emit('drop', $event.value)">
      <div class="pizza" :class="'pizza--foundation--' + diameter + '-' + sauce">
        <div class="pizza__wrapper">
          <div class="pizza__filling"
               v-for="(value, key) in pizzaIngredients"
               :key="key"
               :class="[
             `pizza__filling--${key}`,
              value === TWO_INGREDIENTS && 'pizza__filling--second',
              value === THREE_INGREDIENTS && 'pizza__filling--third',
            ]"
          ></div>
        </div>
      </div>
    </app-drop>
  </div>
</template>

<script setup>
import { computed } from "vue";

import AppDrop from '@/common/components/AppDrop.vue'

const TWO_INGREDIENTS = 2;
const THREE_INGREDIENTS = 3;

const props = defineProps({
  ingredients: {
    type: Object,
    default: () => ({}),
  },
  diameter: {
    type: String,
    required: true
  } ,
  sauce: {
    type: String,
    required: true
  }
})

const pizzaIngredients = computed(() => {
  return Object.entries(props.ingredients).reduce((result, entry) => {
    const [key, value] = entry;

    if (value > 0) {
      result[key] = value;
    }

    return result;
  }, {});
});

defineEmits(['drop'])
</script>
