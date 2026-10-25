# Storefront Masterclass: 10-Module Curriculum

**Build:** a web + mobile storefront where sellers open a store, list items and get paid. Guests can buy without an account.
**Stack:** Vite + React (static build) · Supabase (DB, Auth, Storage, Edge Functions) · Paystack · WhatsApp Cloud API · Resend · Capacitor · Vercel · GitHub Actions

**Owners:** **A** = web, frontend and backend flows (Modules 1–5). **B** = mobile, build pipeline, CI/CD and launch (Modules 6–10).

| # | Module | What gets built | Owner | End state |
|---|--------|-----------------|:-----:|-----------|
| 1 | Architecture, Setup & Data Model | Repo, Vite + React app, Supabase project, tables (`stores`, `products`, `orders`, `order_items`), Row Level Security | A | Empty app talking to the DB, secured |
| 2 | Seller Accounts & Store Creation | Supabase Auth, store profile, add/edit products with image upload | A | A seller can sign up and list an item |
| 3 | Storefront, Cart & Guest Checkout | Public store page, product listing, cart state, checkout with no login, account prompt only at pay | A | A guest can fill a cart and reach checkout |
| 4 | Payments & Orders | Paystack checkout, Edge Function webhook to verify payment, order saved as paid, seller sees orders | A | A real test-mode payment creates a paid order |
| 5 | Notifications & Verification | WhatsApp OTP at checkout, order alerts to seller and buyer, Resend receipt emails | A | Sale triggers WhatsApp + email |
| 6 | Wrap the Web App with Capacitor (Android) | Add Capacitor, build the Android app, run it on a device | B | App runs on a real Android phone |
| 7 | Native Features & iOS | Push notifications (FCM), splash and icons, deep links, iOS build and TestFlight | B | Sale sends a push to the seller's phone |
| 8 | CI/CD with GitHub Actions | Lint, test and build on every push, Android build artifact, Vercel deploy, secrets handling | B | Push to `main` deploys the web app and builds the APK |
| 9 | Vercel Production Launch | Custom domain, env vars, switch Paystack to live, Supabase prod settings | B | Live on the real domain |
| 10 | Onboarding Assessment & Go-Live | Students sign up on the live app and complete the assessment; Terms and Privacy published | B | Students onboarded on the MVP |

## Holes in the Gemini version (and how this fixes them)

1. **Sellers were never built.** The product is about sellers opening stores, but Gemini only covered the buyer side. Module 2 now covers seller sign-up, store creation and product listing.
2. **Auth was missing.** "Guest checkout, login at the end" needs auth built first. It's in Module 2 and used in Module 3.
3. **Split didn't match your plan.** Gemini used four tracks (Backend, Web, Mobile, DevOps) with 4 backend modules. Your plan is 5 and 5: web for one co-founder, mobile and CI for the other.
4. **No place for Supabase security or webhooks.** Row Level Security and Edge Functions (needed to verify Paystack payments safely) are now in Modules 1 and 4. Without them, anyone could mark an order as paid.
5. **Order was wrong.** Push notifications (old Module 5) came before Capacitor (old Module 7), but push only works in the native app. It's now Module 7.
6. **No orders or seller view.** A seller needs to see what was bought. It's in Module 4.
7. **Go-live was thin.** Switching Paystack to live and the student assessment plus Terms/Privacy step had no home. They're now Modules 9 and 10.
8. **Merged thin modules.** Old Modules 5 and 6 (WhatsApp, Resend) became one, and iOS merged into native features. That freed slots for seller flow and orders.

## Decisions and risks to settle before recording

- **Web framework:** Capacitor needs a static build, so use Vite + React, not Next.js with server rendering.
- **WhatsApp API:** Meta's Cloud API needs a business account, verification and approved message templates. This can take days or weeks. Start now, and decide on a fallback (SMS or email OTP) if approval stalls.
- **Resend free tier:** your notes say about 300 emails/month. I believe Resend's free plan is higher (about 3,000/month, 100/day). Please check their current pricing page.
- **"No Android SDK needed":** you still need Android build tools to produce an APK. GitHub Actions can do the build for students (Module 8), but Module 6 needs Android Studio or the SDK locally.
- **iOS needs a Mac and a paid Apple Developer account** (about $99/yr). Android Play Store is a one-time $25. Say this up front so students aren't surprised.
- **Paystack:** test mode is fine for Modules 4–8. Going live needs business verification, so start it early. Decide how sellers get paid (split payments or subaccounts) before Module 4.
- **Seller dashboard:** kept minimal (orders list only) to stay at 10 modules. Anything bigger is a bonus module.
