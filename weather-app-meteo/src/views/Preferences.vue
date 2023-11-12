//Preferences.vue
<template>
  <IonPage>
    <Header></Header>
    <ion-content>
      <ion-grid class="ion-text-center">
        <ion-col>
          <ion-row>
            <ion-col><p>Ville</p></ion-col>
            <ion-col size="7"
              ><ion-select label="Choisir Ville" label-placement="floating" fill="solid" v-model="villeChoisie">
                <ion-select-option v-for="value in Ville" :key="value.id" :value="value.nom">
                  {{ value.nom }}
                </ion-select-option>
              </ion-select>
            </ion-col>
          </ion-row>
          <ion-row>
            <ion-col><p>Langue</p></ion-col>
            <ion-col size="7"
              ><ion-select label="Choisir Langue" label-placement="floating" fill="solid" v-model="langueChoisie">
                <ion-select-option v-for="value in Langue" :key="value.id" :value="value.nom">
                  {{ value.nom }}
                </ion-select-option>
              </ion-select>
            </ion-col>
          </ion-row>
        </ion-col>
      </ion-grid>
    </ion-content>
    <Footer></Footer>
  </IonPage>
</template>

<script setup lang="ts">
import { IonCol, IonContent, IonGrid, IonPage, IonRow, IonSelect, IonSelectOption } from "@ionic/vue";
import Header from "../components/Header.vue";
import Footer from "../components/Footer.vue";
import { Ref, ref, reactive, watch } from "vue";
import { Storage } from "@capacitor/storage";

const villeChoisie = ref<string>("");
const langueChoisie = ref<string>("");

interface Item {
  id: number;
  nom: string;
}

const Ville = ref<Item[]>([
  { id: 1, nom: "Montreal" },
  { id: 2, nom: "Laval" },
  { id: 3, nom: "Quebec" },
  { id: 3, nom: "Ma Position Actuelle" },
]);

const Langue = ref<Item[]>([
  { id: 1, nom: "en" },
  { id: 2, nom: "fr" },
  { id: 3, nom: "es" },
]);

//methodes de capacitor storage
const storeChoice = async (key: string, value: string) => {
  await Storage.set({
    key: key,
    value: value,
  });
};

const getChoice = async (key: string) => {
  const { value } = await Storage.get({ key: key });
  return value;
};

//repere la ville choisie
getChoice("city").then((value) => {
  villeChoisie.value = value || "";
});
getChoice("language").then((value) => {
  langueChoisie.value = value || "";
});

//sauvegarde la ville choisie
watch(villeChoisie, (value) => {
  storeChoice("city", value);
});
watch(langueChoisie, (value) => {
  storeChoice("language", value);
});
</script>
