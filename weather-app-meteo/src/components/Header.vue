<template>
  <ion-header>
    <ion-toolbar color="secondary">
      <ion-buttons slot="start">
        <ion-back-button defaultHref="/"></ion-back-button>
      </ion-buttons>
      <ion-buttons slot="end">
        <ion-menu-button> </ion-menu-button>
      </ion-buttons>
      <ion-title>Weather App</ion-title>
    </ion-toolbar>
  </ion-header>
</template>

<script setup lang="ts">
import { IonHeader, IonToolbar, IonTitle, IonButtons, IonMenuButton, IonBackButton } from "@ionic/vue";
</script>

<style scoped></style>
