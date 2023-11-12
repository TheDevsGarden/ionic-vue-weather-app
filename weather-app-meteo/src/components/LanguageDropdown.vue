<template>
  <ion-page>
    <select v-model="selectedLanguage">
      <option v-for="language in languages" :value="language.code">{{ language.language }}</option>
    </select>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { IonPage } from "@ionic/vue";
import languages from "../resources/languages.json";

const languagesList = languages.text;
const selectedLanguage = ref("");
</script>
