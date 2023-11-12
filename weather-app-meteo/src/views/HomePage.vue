//Homepage.vue
<template>
  <ion-page>
    <Header></Header>
    <ion-content>
      <ion-grid class="ion-text-center">
        <ion-row>
          <ion-col>
            <ion-label>{{ date }}</ion-label>
            <div>
              <p>{{ city }}</p>
              <img class="image" :src="icon" />
            </div>
          </ion-col>
        </ion-row>
        <ion-col>
          <p>{{ description }}</p>
          <p class="bold">{{ temp }} °C</p>
          <p>Ressenti: {{ feelsLike }}</p>
        </ion-col>
        <ion-row>
          <ion-col
            ><p>Haut: {{ high }}</p>
          </ion-col>
          <ion-col
            ><p>Bas: {{ low }}</p>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
    <Footer></Footer>
  </ion-page>
</template>

<script setup lang="ts">
import { IonContent, IonPage, IonLabel, IonGrid, IonCol, IonRow } from "@ionic/vue";
import { Header, Footer, getCurrentPosition, fetchCity, getCurrentWeather, getTodayDate } from "../components/barrel";
import { onIonViewDidEnter, onIonViewDidLeave, onIonViewWillEnter, onIonViewWillLeave } from "@ionic/vue";
import { ref, Ref } from "vue";
import { Storage } from "@capacitor/storage";

let lang = "fr";
let date: Ref<string> = ref("");
let city: Ref<string> = ref("");
let description: Ref<string> = ref("");
let feelsLike: Ref<Number> = ref(0);
let temp: Ref<Number> = ref(0);
let high: Ref<Number> = ref(0);
let low: Ref<Number> = ref(0);
let icon: Ref<string> = ref("");
let maVille: string = "";

getTodayDate().then((todayDate) => {
  try {
    date.value = todayDate.toDateString();
  } catch (error) {
    console.log(error);
  }
}); //to-do: translate to french.

//to-do: refactor
const initializeData = async () => {
  maVille = await getChoice("city");
  try {
    if (maVille == "" || maVille == "Ma Position Actuelle") {
      const positionInfo = await getCurrentPosition();
      const weatherInfo = await getCurrentWeather(positionInfo.lat, positionInfo.lon, lang);
      city.value = weatherInfo.name;
      temp.value = Math.round(weatherInfo.temp);
      high.value = Math.round(weatherInfo.temp_max);
      low.value = Math.round(weatherInfo.temp_min);
      feelsLike.value = Math.round(weatherInfo.feels_like);
      description.value = weatherInfo.weather_description;
      // icon.value = "../img/" + weatherInfo.weather_icon + ".svg"; // too lazy to implement img folder when there are many more codes on the website for example ending in '10n'
      icon.value = "https://openweathermap.org/img/wn/" + weatherInfo.weather_icon + "@4x.png";
      console.log(icon.value);
    } else {
      const cityInfo = await fetchCity(maVille);
      const weatherInfo = await getCurrentWeather(cityInfo.lat, cityInfo.lon, lang);
      city.value = weatherInfo.name;
      temp.value = Math.round(weatherInfo.temp);
      high.value = Math.round(weatherInfo.temp_max);
      low.value = Math.round(weatherInfo.temp_min);
      feelsLike.value = Math.round(weatherInfo.feels_like);
      description.value = weatherInfo.weather_description;
      icon.value = "https://openweathermap.org/img/wn/" + weatherInfo.weather_icon + "@4x.png";
      console.log(icon.value);
    }
  } catch (error) {
    console.error(error);
  }
};

const getChoice = async (key: string) => {
  const { value } = await Storage.get({ key: key });
  return value || "";
};

onIonViewWillEnter(() => {
  initializeData();
});
</script>

<style>
.image {
  min-width: min(330px, 60%);
  margin: auto;
}
.bold {
  font-weight: bold;
  font-size: x-large;
}
</style>
