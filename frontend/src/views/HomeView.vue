<template>
  <main class="content">
    <form action="#" method="post">

      <div class="content__wrapper">
        <h1 class="title title--big">Конструктор пиццы</h1>

        <dough-selector
            :doughItems="doughItems"
            v-model="data.dough"
        />

        <diameter-selector
            :sizeItems="sizeItems"
            v-model="data.diameter"
        />

        <div class="content__ingredients">
          <div class="sheet">
            <h2 class="title title--small sheet__title">Выберите ингредиенты</h2>

            <div class="sheet__content ingredients">
              <sauce-selector
                  :sauceItems="sauceItems"
                  v-model="data.sauce"
              />
              <ingredients-selector
                  :values="data.ingredients"
                  :ingredientItems="ingredientItems"
                  @update="updateIngredientAmount"
              />
            </div>
          </div>
        </div>

        <div class="content__pizza">
          <label class="input">
            <span class="visually-hidden">Название пиццы</span>
            <input type="text" name="pizza_name" placeholder="Введите название пиццы" v-model="data.name">
          </label>
          <pizza-constructor
              :ingredients="data.ingredients"
              :diameter="data.diameter"
              :sauce="data.sauce"
              @drop="addIngredient"
          />
          <div class="content__result">
            <p>Итого: {{ price }} ₽</p>
            <button type="button" class="button" :disabled="disableSubmit">Готовьте!</button>
          </div>
        </div>
      </div>
    </form>
  </main>
</template>

<script setup>
import {computed, reactive} from 'vue'

import {
  normalizeDough,
  normalizeIngredients,
  normalizeSauces,
  normalizeSize,
} from "@/common/helpers/normalize";
import doughJSON from "@/mocks/dough.json";
import ingredientsJSON from "@/mocks/ingredients.json";
import saucesJSON from "@/mocks/sauces.json";
import sizesJSON from "@/mocks/sizes.json";
import DoughSelector from '@/modules/constructor/DoughSelector.vue'
import DiameterSelector from '@/modules/constructor/DiameterSelector.vue'
import SauceSelector from '@/modules/constructor/SauceSelector.vue'
import IngredientsSelector from '@/modules/constructor/IngredientsSelector.vue'
import PizzaConstructor from '@/modules/constructor/PizzaConstructor.vue'

const doughItems = doughJSON.map(normalizeDough);
const ingredientItems = ingredientsJSON.map(normalizeIngredients);
const sauceItems = saucesJSON.map(normalizeSauces);
const sizeItems = sizesJSON.map(normalizeSize);

const data = reactive({
  name: "",
  ingredients: ingredientItems.reduce((acc, item) => {
    acc[item.value] = 0;
    return acc;
  }, {}),
  diameter: sizeItems[0].value,
  sauce: sauceItems[0].value,
  dough: doughItems[0].value,
})

const price = computed(() => {
  const {dough, diameter, sauce, ingredients} = data;

  const sizeMultiplier =
      sizeItems.find((item) => item.value === diameter)?.multiplier ?? 1;

  const doughPrice =
      doughItems.find((item) => item.value === dough)?.price ?? 0;

  const saucePrice =
      sauceItems.find((item) => item.value === sauce)?.price ?? 0;

  const ingredientsPrice = ingredientItems
      .map((item) => ingredients[item.value] * item.price)
      .reduce((acc, item) => acc + item, 0);

  return (doughPrice + saucePrice + ingredientsPrice) * sizeMultiplier;
});

const addIngredient = (ingredient) => {
  data.ingredients[ingredient]++;
};

const disableSubmit = computed(() => {
  return data.name.length === 0 || price.value === 0;
});

const updateIngredientAmount = (ingredient, count) => {
  data.ingredients[ingredient] = count;
};
</script>