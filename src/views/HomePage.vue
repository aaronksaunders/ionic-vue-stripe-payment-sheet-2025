<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Payment</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <ion-item>
        <ion-label position="stacked" style="margin-bottom: 10px"
          >Amount ($)</ion-label
        >
        <ion-input
          v-model="amount"
          type="number"
          :min="0.5"
          step="0.01"
          @ionInput="validateAmount"
        ></ion-input>
      </ion-item>
      <ion-item>
        <ion-label position="stacked" style="margin-bottom: 10px"
          >Email</ion-label
        >
        <ion-input v-model="email" type="email"></ion-input>
      </ion-item>
      <StripeCheckoutButton
        :amount="amountInCents"
        currency="usd"
        :email="email"
      />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonItem,
  IonLabel,
  IonInput,
} from "@ionic/vue";
import StripeCheckoutButton from "@/components/StripeCheckoutButton.vue";

const amount = ref<string>("");
const email = ref<string>("");

const validateAmount = (event: CustomEvent) => {
  const value = parseFloat(event.detail.value);
  if (value < 0.5) {
    amount.value = "0.50";
  }
};

const amountInCents = computed(() =>
  Math.round(parseFloat(amount.value || "0") * 100)
);
</script>

<style scoped></style>
