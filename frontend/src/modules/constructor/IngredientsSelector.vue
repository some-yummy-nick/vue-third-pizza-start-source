<template>
  <div class="ingredients__filling">
    <p>Начинка:</p>

    <ul class="ingredients__list">
      <template v-for="item in ingredientItems" :key="item.id">
        <app-drag
            :transfer-data="item"
            :draggable="getValue(item.value) < MAX_INGREDIENT_COUNT"
        >
          <li class="ingredients__item">
            <span :class="'filling filling--' + item.value">{{ item.name }}</span>
            <app-counter
                :value="getValue(item.value)"
                :min="0"
                :max="MAX_INGREDIENT_COUNT"
                @input="inputValue(item.value, $event)"
            />
          </li>
        </app-drag>
      </template>
    </ul>
  </div>
</template>

<script setup>
import {toRef} from "vue";

import AppDrag from '@/common/components/AppDrag.vue'
import AppCounter from '@/common/components/AppCounter.vue'

import {MAX_INGREDIENT_COUNT} from "@/common/constants";

const props = defineProps({
  ingredientItems: {
    type: Array,
    default: () => [],
  },
  values: {
    type: Object,
    default: () => ({}),
  },
})

const values = toRef(props, "values");

const getValue = (ingredient) => {
  return values.value[ingredient] ?? 0;
};

const setValue = (ingredient, count) => {
  emit("update", ingredient, Number(count));
};

const inputValue = (ingredient, count) => {
  return setValue(ingredient, Math.min(MAX_INGREDIENT_COUNT, Number(count)));
};

const update = (ingredient, count) => {
  emit("update", ingredient, Number(count));
}

const emit = defineEmits(["update"]);
</script>
