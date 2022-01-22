<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search By Ingredient</ion-title>
      </ion-toolbar>
    </ion-header>


    <ion-content v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>  
      </ion-content>

      <ion-content :fullscreen="true" v-else>
        <ion-list>
          <ion-item 
            v-for="ingredient in state.lstIngredients"
            :key="ingredient.strIngredient1"
            @click="
              ()=>
              router.push(`/drinks-by-ingredient/${ingredient.strIngredient1}`)
            "
          >
          <ion-avatar slot="start">
            <img :src="ingredientImage(ingredient.strIngredient1)"/>
          </ion-avatar>

          <ion-label>
            <h2>{{ingredient.strIngredient1}}</h2>
          </ion-label>
          </ion-item>
          
        </ion-list>

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonSpinner, IonAvatar, IonLabel, IonItem, IonList } from '@ionic/vue';
import { reactive } from 'vue'

import axios from 'axios';
import {useRouter} from 'vue-router';
import IIngredient from '../interfaces/IIngredient'



export default  {
  name: 'Tab2',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonSpinner, IonAvatar, IonLabel, IonItem, IonList
  },
  setup(){
    const router = useRouter();
    const state = reactive({
      lstIngredients:[] as IIngredient[],
      loading:false
    });

    const fetchIngredients= async()=>{
      state.loading = true;
      const res = await axios.get(
        "https://www.thecocktaildb.com/api/json/v1/1/list.php?i=list"
      );


      if(res.data){
        state.lstIngredients = res.data?.drinks;
        state.lstIngredients.sort(function(a,b){
          return a.strIngredient1.localeCompare(b.strIngredient1);
        });
      }
      state.loading=false;
    };
    
    const ingredientImage =(ingredient: string)=>{
      return `https://www.thecocktaildb.com/images/ingredients/${encodeURI(ingredient)}-Small.png`;
    };

    fetchIngredients();
    return{
      router,
      state,
      ingredientImage,
    }

  }
}
</script>