<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button></ion-back-button>
        </ion-buttons>
        <ion-title>{{ ingredient }} Drinks</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item
          v-for="drink in state.lstDrinks"
          :key="drink.idDrink"
          @click="
            () => router.push(`/drink/${drink.idDrink}`)
          "
        >
          <ion-avatar slot="start">
            <img :src="drink.strDrinkThumb" />
          </ion-avatar>
          <ion-label>
            <h2>{{ drink.strDrink }}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { reactive } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";

interface Drink {
    strDrink: string;
    strDrinkThumb: string;
    idDrink: string;
}

export default {
    setup(){
        const router = useRouter();
        const route = useRoute();
        const ingredient = route.params.ingredients as string;

        const state = reactive({
            lstDrinks:[] as Drink [],
            loading: false,
        });

        const fetchDrinksByIngredients = async(ingredient: string)=>{
            state.loading = true;
            const res = await axios.get(
                `https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=${encodeURI(ingredient)}`
            );
            if(res.data){
                state.lstDrinks=res.data?.drinks;
            }
            state.loading = false;
        }

        fetchDrinksByIngredients(ingredient);

        return{
            fetchDrinksByIngredients,
            router,
            state,
        }
    }

}
</script>

<style>

</style>