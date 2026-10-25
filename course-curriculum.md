# Storefront Masterclass: Course Curriculum

A web and mobile storefront where sellers open a store, list items and get paid. Guests can buy without an account.

**Stack:** Next.js 15 (static export) · Supabase (DB, Auth, Storage, Edge Functions) · Paystack · Resend · Capacitor · Vercel · GitHub Actions

**Owners:** **N** = Noah (Modules 1, 3, 5, 6, 9) · **C** = Co-founder (Modules 2, 4, 7, 8, 10)

| # | Module | What gets built | Owner | End state |
|---|--------|-----------------|:-----:|-----------|
| 1 | Setup Everything | Repo, Next.js app (static export), Supabase project, tables (stores, products, orders, order_items), Row Level Security, buy the domain, deploy to Vercel on the domain, verify the domain with Resend, .env.local and Vercel env vars, set up Claude safely with SQL migrations on a dev project | N | Blank app live on the domain, database secured, email ready; no more setup |
| 2 | Capacitor & Build Pipeline | Wrap the app in Capacitor for Android, GitHub Actions builds a signed APK on every push to main using one release keystore stored in GitHub secrets, version number goes up with each build, web deploys to Vercel | C | Each push gives an APK that updates the old one |
| 3 | Seller Accounts & Store Creation | Supabase Auth, store profile, add and edit products with image upload | N | A seller can sign up and list an item |
| 4 | Storefront, Cart & Guest Checkout | Public store page, product listing, cart state, checkout with no login, account prompt only at the pay step | C | A guest can fill a cart and reach checkout |
| 5 | Payments & Orders | Paystack checkout, Edge Function webhook to verify payment, order saved as paid | N | A test-mode payment creates a paid order |
| 6 | Email Verification & Notifications | Email verification code at checkout, order confirmation and receipt emails to the buyer, new-order emails to the seller (Resend) | N | A sale triggers emails to buyer and seller |
| 7 | Seller Dashboard | Orders list and order detail, mark as fulfilled, simple sales summary | C | A seller can see and manage their orders |
| 8 | Native Features & iOS | Push notifications (FCM), splash screen and icons, deep links, iOS build and TestFlight | C | A sale sends a push to the seller's phone |
| 9 | Go Live | Switch Paystack to live, Supabase production settings, production env vars, Play Store release | N | Real payments on the real domain and in the store |
| 10 | Onboarding Assessment | Students sign up on the live app and complete the assessment; Terms and Privacy published | C | Students onboarded on the MVP |

## Key handling rules (taught in Module 1)

- **Assume the app bundle is public.** Anything prefixed NEXT_PUBLIC_ is bundled into the website and the APK. Only three values ship in the app: the Supabase URL, the Supabase anon key and the API URL.
- **The anon key is safe because of Row Level Security.** Plain reads and writes (products, cart, store profile) go straight to Supabase.
- **Sensitive actions go through Edge Functions.** Create order, verify payment and send email run in Supabase Edge Functions that check the user's login token. Allow the Capacitor origins (https://localhost, capacitor://localhost) in CORS.
- **Secrets live only in Edge Function secrets.** The service-role key, Paystack secret key and Resend key are set with supabase secrets set. They never go in the app, the repo or Vercel.
- **Local and Vercel setup.** Public values go in .env.local (gitignored) with a blank .env.example committed, and the same public values go in Vercel environment variables.
- **Claude and the database.** Claude writes SQL migrations applied with the Supabase CLI against a separate dev project. It never gets the service-role key, and a deny rule stops it reading .env files.
- **Safety nets.** GitHub push protection and secret scanning. If a key leaks, rotate it in the dashboard immediately.

## Decisions and risks to settle before recording

- **Updates that install over the old app:** Android only accepts an update if it is signed with the same key and has a higher version number. Use one release keystore in GitHub secrets and set the version from the build number. CI debug builds use a random key each run and will not upgrade. Firebase App Distribution can notify testers of each new build.
- **Resend free tier:** Notes say about 300 emails a month. I believe the free plan is higher (about 3,000 a month, 100 a day). Confirm on their pricing page.
- **Android build tools:** An APK still needs the Android SDK. GitHub Actions builds it for students, but Module 2 needs Android Studio or the SDK locally to test.
- **iOS:** Needs a Mac and a paid Apple Developer account (about $99 a year). Say so up front.
- **Paystack:** Test mode until Module 9. Going live needs business verification, so start early. Decide how sellers get paid (split payments or subaccounts) before Module 5.
- **Static export:** No server-rendered pages. Use client-side routing for stores and products, and put webhooks in Edge Functions.
