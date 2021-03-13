<template>

      <ion-card>
        <img :src="drink.strDrinkThumb" />

        <ion-card-header>
          <ion-card-subtitle>
            {{ drink.strCategory }} | Served In
            {{ drink.strGlass }}
          </ion-card-subtitle>

          <ion-card-title>{{ drink.strDrink }}</ion-card-title>
        </ion-card-header>

        <ion-card-content>
          <p>{{ drink.strInstructions }}</p>
          <ion-list-header>
            <ion-label>Ingredients</ion-label>
          </ion-list-header>

          
          <ion-list>
            <template v-for="index in 15" :key="index">
              <ion-item v-if="drink[`strIngredient${index}`]">
                <ion-label>
                  <span v-if="drink[`strMeasure${index}`]">
                    {{ drink[`strMeasure${index}`] }}
                  </span>
                    {{drink[`strIngredient${index}`]}}
                </ion-label>
              </ion-item>
            </template>
          </ion-list>

        </ion-card-content>
      </ion-card>
  
</template>

<script script="ts">
import {IonCard,IonCardHeader,IonCardSubtitle,IonCardContent,IonCardTitle,IonList,IonListHeader,IonLabel,IonItem
} from '@ionic/vue';

export default {
    name:'DrinkCard',
    props:{
        drink:Object,
    },
    components:{
        IonCard,
        IonCardHeader,
        IonCardSubtitle,
        IonCardContent,
        IonCardTitle,
        IonList,
        IonListHeader,
        IonLabel,
        IonItem
    }
}
</script>

<style>

</style>