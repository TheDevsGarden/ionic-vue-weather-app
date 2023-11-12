<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay">
        <ion-content>
          <ion-list>
            <ion-list-header>Mon menu</ion-list-header>
            <ion-menu-toggle auto-hide="false">
              <ion-item router-link="/">
                <ion-icon slot="start" :ios="logoTux" :md="logoTux"></ion-icon>
                <ion-label>Meteo</ion-label>
              </ion-item>

              <ion-item router-link="/preferences">
                <ion-icon slot="start" :ios="logoTux" :md="logoTux"></ion-icon>
                <ion-label>Preferences</ion-label>
              </ion-item>
            </ion-menu-toggle>
          </ion-list>
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet, IonSplitPane, IonMenu, IonContent, IonList, IonMenuToggle, IonItem, IonIcon, IonLabel, IonListHeader } from "@ionic/vue";

import { logoTux } from "ionicons/icons";

const setup = () => {
  return {
    logoTux,
  };
};
</script>
