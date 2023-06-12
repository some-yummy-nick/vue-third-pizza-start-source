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
            <div class="counter counter--orange ingredients__counter">
              <button type="button"
                      class="counter__button counter__button--minus"
                      :disabled="getValue(item.value) === 0"
                      @click="decrementValue(item.value)"
              >
                <span class="visually-hidden">Меньше</span>
              </button>
              <input type="text" name="counter"
                     class="counter__input"
                     :value="getValue(item.value)"
                     @input="inputValue(item.value, $event.target.value)"
              >
              <button type="button"
                      class="counter__button counter__button--plus"
                      :disabled="getValue(item.value) === MAX_INGREDIENT_COUNT"
                      @click="incrementValue(item.value)"
              >
                <span class="visually-hidden">Больше</span>
              </button>
            </div>
          </li>
        </app-drag>
      </template>
    </ul>
  </div>
</template>

<script setup>
import {toRef} from "vue";

import AppDrag from '@/common/components/AppDrag.vue'
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

const decrementValue = (ingredient) => {
  setValue(ingredient, getValue(ingredient) - 1);
};

const incrementValue = (ingredient) => {
  setValue(ingredient, getValue(ingredient) + 1);
};

const inputValue = (ingredient, count) => {
  return setValue(ingredient, Math.min(MAX_INGREDIENT_COUNT, Number(count)));
};

const emit = defineEmits(["update"]);
</script>
