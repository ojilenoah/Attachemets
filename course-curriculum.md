# Storefront Masterclass: Course Curriculum

A web and mobile storefront where sellers open a store, list items and get paid. Guests can buy without an account.

**Stack:** Next.js 15 (static export) · Supabase (DB, Auth, Storage, Edge Functions) · Paystack · Resend · Capacitor · Vercel · GitHub Actions

**Owners:** **N** = Noah (Modules 1, 2, 4, 5, 9) · **C** = Co-founder (Modules 3, 6, 7, 8, 10)

| # | Module | What gets built | Owner | End state |
|---|--------|-----------------|:-----:|-----------|
| 1 | Setup, Supabase & Keeping Secrets Safe | Repo, Next.js app (static export), Supabase project, tables (stores, products, orders, order_items), Row Level Security, .env.local and Vercel env vars, buy the domain and verify it with Resend, set up Claude safely with SQL migrations on a dev project | N | Empty app talking to a secured database; domain and email ready; no secrets in the repo |
| 2 | Seller Accounts & Store Creation | Supabase Auth, store profile, add and edit products with image upload | N | A seller can sign up and list an item |
| 3 | Storefront, Cart & Guest Checkout | Public store page, product listing, cart state, checkout with no login, account prompt only at the pay step | C | A guest can fill a cart and reach checkout |
| 4 | Payments & Orders | Paystack checkout, Edge Function webhook to verify payment, order saved as paid, seller sees orders | N | A test-mode payment creates a paid order |
| 5 | Email Verification & Notifications | Email verification code at checkout, order confirmation and receipt emails to the buyer, new-order emails to the seller (Resend) | N | A sale triggers emails to buyer and seller |
| 6 | Wrap the Web App with Capacitor (Android) | Add Capacitor, build the Android app, run it on a device | C | App runs on a real Android phone |
| 7 | Native Features & iOS | Push notifications (FCM), splash screen and icons, deep links, iOS build and TestFlight | C | A sale sends a push to the seller's phone |
| 8 | CI/CD with GitHub Actions | Lint, test and build on every push, Android build artifact, Vercel deploy, secrets handling | C | Push to main deploys the web app and builds the APK |
| 9 | Vercel Production Launch | Attach the domain to Vercel, production env vars, switch Paystack to live, Supabase production settings | N | Live on the real domain |
| 10 | Onboarding Assessment & Go-Live | Students sign up on the live app and complete the assessment; Terms and Privacy published | C | Students onboarded on the MVP |

## Key handling rules (taught in Module 1)

- **Assume the app bundle is public.** Anything prefixed NEXT_PUBLIC_ is bundled into the website and the APK. Only three values ship in the app: the Supabase URL, the Supabase anon key and the API URL.
- **The anon key is safe because of Row Level Security.** Plain reads and writes (products, cart, store profile) go straight to Supabase.
- **Sensitive actions go through Edge Functions.** Create order, verify payment and send email run in Supabase Edge Functions that check the user's login token. Allow the Capacitor origins (https://localhost, capacitor://localhost) in CORS.
- **Secrets live only in Edge Function secrets.** The service-role key, Paystack secret key and Resend key are set with supabase secrets set. They never go in the app, the repo or Vercel.
- **Local and Vercel setup.** Public values go in .env.local (gitignored) with a blank .env.example committed, and the same public values go in Vercel environment variables.
- **Claude and the database.** Claude writes SQL migrations applied with the Supabase CLI against a separate dev project. It never gets the service-role key, and a deny rule stops it reading .env files.
- **Safety nets.** GitHub push protection and secret scanning. If a key leaks, rotate it in the dashboard immediately.

## Decisions and risks to settle before recording

- **Resend free tier:** Notes say about 300 emails a month. I believe the free plan is higher (about 3,000 a month, 100 a day). Confirm on their pricing page.
- **Android build tools:** An APK still needs the Android SDK. GitHub Actions can build it for students (Module 8), but Module 6 needs Android Studio or the SDK locally.
- **iOS:** Needs a Mac and a paid Apple Developer account (about $99 a year). Say so up front.
- **Paystack:** Test mode covers Modules 4 to 8. Going live needs business verification, so start early. Decide how sellers get paid (split payments or subaccounts) before Module 4.
- **Static export:** No server-rendered pages. Use client-side routing for stores and products, and put webhooks in Edge Functions.
- **Seller dashboard:** Kept to an orders list to stay at 10 modules. Anything bigger is a bonus module.
