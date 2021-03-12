<template>
  <ion-page>
      <ion-header>
          <ion-toolbar>
              <ion-buttons slot="start">
                  <ion-back-button></ion-back-button>
              </ion-buttons>
              <ion-title>{{state.drink.strDrink}}</ion-title>
          </ion-toolbar>
      </ion-header>

      <ion-content :fullscreen="true" v-if="state.loading">
          <div class="loading-c">
              <ion-spinner color="primary"></ion-spinner>
          </div>
      </ion-content>
      <ion-content :fullscreen="true" v-else>
          <DrinkCard :drink="state.drink"></DrinkCard>
      </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {IonPage,IonHeader,IonToolbar,IonTitle,IonContent,IonButtons,IonBackButton} from "@ionic/vue";
import {reactive} from 'vue';
import{useRoute} from 'vue-router';
import axios from 'axios';
import DrinkCard from '@/components/DrinkCard.vue'

interface Drink {
 idDrink: string;
 strDrink: string;
 strDrinkAlternate: string;
 strDrinkES: string;
 strDrinkDE: string;
 strDrinkFR: string;
 "strDrinkZH-HANS": string;
 "strDrinkZH-HANT": string;
 strTags: string;
 strVideo: string;
 strCategory: string;
 strIBA: string;
 strAlcoholic: string;
 strGlass: string;
 strInstructions: string;
 strInstructionsES: string;
 strInstructionsDE: string;
 strInstructionsFR: string;
 "strInstructionsZH-HANS": string;
 "strInstructionsZH-HANT": string;
 strDrinkThumb: string;
 strIngredient1: string;
 strIngredient2: string;
 strIngredient3: string;
 strIngredient4: string;
 strIngredient5: string;
 strIngredient6: string;
 strIngredient7: string;
 strIngredient8: string;
 strIngredient9: string;
 strIngredient10: string;
 strIngredient11: string;
 strIngredient12: string;
 strIngredient13: string;
 strIngredient14: string;
 strIngredient15: string;
 strMeasure1: string;
 strMeasure2: string;
 strMeasure3: string;
 strMeasure4: string;
 strMeasure5: string;
 strMeasure6: string;
 strMeasure7: string;
 strMeasure8: string;
 strMeasure9: string;
 strMeasure10: string;
 strMeasure11: string;
 strMeasure12: string;
 strMeasure13: string;
 strMeasure14: string;
 strMeasure15: string;
 strCreativeCommonsConfirmed: string;
 dateModified: string;
}

export default {
  name: "Drink",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonButtons,
    IonBackButton,
    DrinkCard
  },
  setup() {
    const route = useRoute();
    const drinkId = route.params.id as string;
    const state = reactive({
      drink: {} as Drink,
      loading: false,
    });
    const fetchDrinkById = async (drinkId: string) => {
      state.loading = true;
      const res = await axios.get(
        `https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${drinkId}`
      );
      if (res.data) {
        state.drink = res.data?.drinks[0];
      }
      state.loading = false;
    };
    fetchDrinkById(drinkId);
    return {
      state,
    };
  },
};
</script>

<style scoped>
.loading-center{
    display:flex;
    align-items:center;
    justify-content:center;
    height:90vh;
}
ion-spinner{
    transform: scale(1.5);
}

</style>