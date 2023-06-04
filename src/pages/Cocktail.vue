<template>
  <div v-if="cocktail" class="wrap">
    <AppLayout :img-url="cocktail.strDrinkThumb">
      <div class="wrapper">
        <div class="info">
          <div class="title">{{ cocktail.strDrink }}</div>
          <div class="line"></div>
          <div class="list">
            <div v-for="(item, key) in ingredients" :key="key" class="list-item">
              {{ item.name }}
              <template v-if="item.messure">
                |
                {{ item.messure }}
              </template>
            </div>
            <div class="instructions">
              {{ cocktail.strInstructions }}
            </div>
          </div>
        </div>
      </div>
    </AppLayout>
  </div>
</template>

<script setup>
import { useRoute } from "vue-router";
import AppLayout from "@/components/AppLayout.vue";
import {computed, ref} from "vue";
import { COCKTAIL_BY_ID_URL } from '@/constans'
import axios from "axios";

const route = useRoute()

const cocktail = ref(null)
const cocktailId = computed(() => {
  return route.path.split('/').pop()
})

const ingredients = computed(() => {
  const ingredients = []

  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) break

    const ingredient = {}
    ingredient.name = cocktail.value[`strIngredient${i}`]
    ingredient.messure = cocktail.value[`strMeasure${i}`]

    ingredients.push(ingredient)
  }

  return ingredients
})

async function getCocktail() {
  const data = await axios.get(`${COCKTAIL_BY_ID_URL}${cocktailId.value}`)
  cocktail.value = data?.data?.drinks[0]
}

getCocktail()
</script>

<style lang="sass" scoped>
@import "../assets/styles/main"
</style>
