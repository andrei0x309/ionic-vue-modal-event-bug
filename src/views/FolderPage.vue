<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>Bug reproduction app</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Bug reproduction app</ion-title>
        </ion-toolbar>
      </ion-header>
      
      <div style="padding: 2rem;">
      <p>Demo app to reproduce bug in Ionic Vue 8.7.2, where ionModalWillPresent is not executed, but didPresent( is a ionModalWillPresent shorthand) is executing</p>

      <ion-button id="open-modal" >Open modal that has ionModalWillPresent attached</ion-button>
      <ion-button id="open-modal2">Open modal that has didPresent attached</ion-button>
      
      <div v-if="executionHistory.length">
        <p>Execution history:</p>
        <p style="margin-left: 0.5rem;" v-for="(item, index) in executionHistory" :key="index"> {{ item }} </p>
      </div>

      
      </div>
    <ion-modal ref="modal" trigger="open-modal" @ionModalDidPresent="ionModalDidPresentFn">
      <ion-header>
        <ion-toolbar>
          <ion-title>Welcome</ion-title>
          <ion-buttons slot="end">
            <ion-button :strong="true" @click="close()">Close</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <p>Modal with ionModalDidPresentFn attached, check console for output</p>
      </ion-content>
    </ion-modal>

      <ion-modal ref="modal2" trigger="open-modal2" @didPresent="didPresent">
      <ion-header>
        <ion-toolbar>
          <ion-title>Welcome</ion-title>
          <ion-buttons slot="end">
            <ion-button :strong="true" @click="close2()">Close</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <p>Modal with ionModalWillPresent attached, check console for output</p>
      </ion-content>
    </ion-modal>
       
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonModal, IonButton } from '@ionic/vue';
import { ref } from 'vue';

 
const modal = ref();
const modal2 = ref();
const executionHistory = ref([] as string[]);

const close = () => modal.value.$el.dismiss(null, 'cancel');
const close2 = () => modal2.value.$el.dismiss(null, 'cancel');

 
const ionModalDidPresentFn = () => {
  console.log('ionModalDidPresentFn executed');
  executionHistory.value.push('ionModalDidPresent executed at ' + new Date().toLocaleTimeString());
}


const didPresent = () => {
  console.log('didPresent executed');
  executionHistory.value.push('didPresent executed at '  + new Date().toLocaleTimeString());
}


</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
