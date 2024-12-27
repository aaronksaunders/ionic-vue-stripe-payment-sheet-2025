# Ionic Vue Stripe Payment Demo

A simple payment demo using Ionic Vue and Stripe Payment Sheet. This project demonstrates how to integrate Stripe payments in an Ionic Vue application.

## Features

- Simple payment form with amount and email input
- Stripe Payment Sheet integration
- Real-time amount validation (minimum $0.50)
- Email validation
- Toast notifications for feedback

## Setup

1. Clone the repository

2. Install dependencies:

```
npm install
cd server
npm install
```

3. Configure Stripe keys:

copy .env.example to .env and add your Stripe keys in the server/.env file

copy .env.example to .env and add your Stripe keys in the src/.env file

4. Run the server:

```
npm run start
```

5. Run the app:

```
npm run dev
```

The application will be available at `http://localhost:5173`

## Development

- Frontend: Ionic Vue with TypeScript
- Backend: Express server for Stripe integration
- Payment Processing: Stripe Payment Sheet

## Project Structure

- `src/`: Frontend code
- `server/`: Backend code
- `capacitor.config.json`: Capacitor configuration
- `package.json`: Project dependencies
- `readme.md`: This file

## Environment Variables

- `VITE_STRIPE_KEY`: Stripe publishable key (frontend)
- `STRIPE_SECRET_KEY`: Stripe secret key (backend)

## Related Documentation

- [Capacitor Stripe Plugin Documentation](https://github.com/capacitor-community/stripe)
- [Stripe Payment Sheet Documentation](https://stripe.com/docs/payments/payment-sheet)
- [Ionic Vue Documentation](https://ionicframework.com/docs/vue/overview)
