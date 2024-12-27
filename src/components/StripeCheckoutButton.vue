<template>
  <ion-button @click="handleCheckout" :disabled="loading">
    {{ loading ? "Processing..." : `Pay $${(amount / 100).toFixed(2)}` }}
  </ion-button>
</template>
<script setup lang="ts">
import { Stripe, PaymentSheetEventsEnum } from "@capacitor-community/stripe";
import { ref, computed } from "vue";
import { IonButton, toastController } from "@ionic/vue";

const props = defineProps<{
  amount: number;
  currency: string;
  email: string;
}>();

const loading = ref<boolean>(false);

const showToast = (message: string, isError = false) => {
  toastController
    .create({
      message,
      duration: 3000,
      position: "bottom",
      color: isError ? "danger" : "success",
    })
    .then((t) => t.present());
};

const isValidEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

const validateInput = (): boolean => {
  if (!props.email || !isValidEmail(props.email)) {
    showToast("Please enter a valid email address", true);
    return false;
  }

  if (!props.amount || props.amount < 50) {
    showToast("Minimum amount is $0.50", true);
    return false;
  }

  return true;
};

const handleCheckout = async () => {
  if (!validateInput()) {
    return;
  }

  loading.value = true;

  try {
    // Step 1: Create PaymentIntent on the server
    const response = await fetch(
      "http://127.0.0.1:3000/create-payment-intent",
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          amount: props.amount,
          currency: props.currency,
          email: props.email,
        }),
      }
    );

    const { paymentIntent, ephemeralKey, customer } = await response.json();
    console.log("paymentIntent", paymentIntent);
    console.log("ephemeralKey", ephemeralKey);
    console.log("customer", customer);

    // Step 2: Initialize the PaymentSheet
    try {
      console.log("Starting createPaymentSheet...");
      await Stripe.createPaymentSheet({
        paymentIntentClientSecret: paymentIntent,
        customerId: customer,
        customerEphemeralKeySecret: ephemeralKey,
        merchantDisplayName: "Personal",
      });

      // Step 3: Present the PaymentSheet
      const { paymentResult } = await Stripe.presentPaymentSheet();
      console.log("Payment result:", paymentResult);

      if (paymentResult === PaymentSheetEventsEnum.Completed) {
        showToast("Payment successful!");
        return true;
      } else {
        throw new Error("Payment failed");
      }
    } catch (error) {
      debugger;
      console.error("Payment sheet error:", error);
      throw error;
    }
  } catch (error) {
    console.error("Payment failed:", error);
    showToast("Payment failed. Please try again.", true);
    return false;
  } finally {
    loading.value = false;
  }
};
</script>
