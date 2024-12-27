<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet } from "@ionic/vue";
import { Capacitor } from "@capacitor/core";
import { Stripe } from "@capacitor-community/stripe";

// Access environment variables through process.env
// const apiUrl = process.env.VITE_API_URL;
const stripeKey = import.meta.env.VITE_STRIPE_KEY;

// Initialize the Stripe plugin
if (Capacitor.isPluginAvailable("Stripe")) {
  debugger;

  if (stripeKey) {
    Stripe.initialize({
      publishableKey: stripeKey,
    });
  } else {
    console.error("Stripe publishable key is not set");
  }
} else {
  console.log("Stripe plugin is not available");
}
</script>
