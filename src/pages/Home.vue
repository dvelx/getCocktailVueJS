<template>
  <AppLayout imgUrl="/src/assets/images/dawa-cocktail-1.jpg" :back-func="removeIngredient" :is-back-button-visible="!!ingredient">
    <div class="wrapper">
      <div v-if="!ingredient || !cocktails" class="info">
        <div class="title">Choose your drink</div>
        <div class="line"></div>
        <div class="select-wrapper">
          <el-select
              v-model="rootStore.ingredient"
              placeholder="Choose main ingredient"
              size="large"
              filterable
              allow-create
              class="select"
              @change="getCocktails"
          >
            <el-option
                v-for="item in ingredients"
                :key="item.strIngredient1"
                :label="item.strIngredient1"
                :value="item.strIngredient1"
            />
          </el-select>
        </div>
        <div class="text">
          Try our delicious cocktail recipes for every occasion. Find delicious cocktail recipes by ingredient through our cocktail generator.
        </div>
        <img src="/src/assets/images/1665865889_66-podacha-blud-com-p-shot-kokteil-krasivie-foto-71 2.png" alt="Cocktails" class="img" />
      </div>
      <div v-else class="info">
        <div class="title">COCKTAILS WITH Midori {{ ingredient }} </div>
        <div class="line"></div>
        <div class="cocktails">
          <CocktailThumb v-for="cocktail in cocktails" :key="cocktail.idDrink" :cocktail="cocktail"/>
        </div>
      </div>
    </div>
  </AppLayout>
</template>

<script setup>
import AppLayout from "@/components/AppLayout.vue";
import { useRootStore } from "@/stores/root";
import { storeToRefs } from "pinia";
import CocktailThumb from "@/components/CocktailThumb.vue";

const rootStore = useRootStore()
rootStore.getIngredients()

const { ingredients,ingredient, cocktails } = storeToRefs(rootStore)


function getCocktails() {
  rootStore.getCocktails(rootStore.ingredient)
}
function removeIngredient() {
  rootStore.setIngredient(null)
}
</script>

<style lang="sass" scoped>
@import "../assets/styles/main"

.select-wrapper
  padding-top: 50px

.select
  width: 220px

.text
  max-width: 516px
  margin: 0 auto
  padding-top: 50px
  line-height: 36px
  letter-spacing: 0.1em
  color: $textMuted

.img
  margin-top: 60px

.cocktails
  display: flex
  flex-wrap: wrap
  align-items: center
  max-height: 400px
  overflow-y: auto
  margin-top: 60px

</style>
<style>
.cocktails {
  scrollbar-width: thin;
  scrollbar-color: #FF0F82 transparent;
}
.cocktails::-webkit-scrollbar {
  width: 5px;
}

.cocktails::-webkit-scrollbar-track {
  background: #141414;
}

.cocktails::-webkit-scrollbar-thumb {
  background-color: #FF0F82;
  border-radius: 20px;
  border: 3px solid transparent;
}
</style>
