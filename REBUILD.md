# Rebuild plan — Stayyo (three apps, owned by you)

Goal: a clean React Native project you control, reproducing the three apps, so you can fix bugs
and ship updates without any vendor.

## What we already know (from the APKs)
- Stack: React Native (New Architecture, Hermes), Firebase (auth/messaging/crashlytics),
  Razorpay payments, Google Maps, ML Kit (OCR/face/barcode — likely Aadhaar KYC), VisionCamera,
  Reanimated, MMKV storage, notifee notifications.
- One shared backend. Full route list in `API-MAP.md`.
- Three roles: Vendor (owner), User (tenant), Warden (manager).

## Recommended structure
A monorepo with a shared core and three app targets:
```
stayyo/
  packages/core/        # api client, auth, models, shared UI, theme
  apps/owner/           # com.stayyoVendor
  apps/tenant/          # com.stayyoUser
  apps/warden/          # com.stayyoWarden
```
Share the API layer so an endpoint fix is made once.

## Phase 1 — foundation (do first)
1. New Expo/bare RN project, TypeScript, New Architecture.
2. API client with `baseURL` from env (`stayyo.in`), auth-token interceptor. Paste routes from `API-MAP.md`.
3. OTP auth: `/api/auth/send-otp`, `/api/auth/verify-otp`; store token in MMKV; session context.
4. Drop in `assets/` images and fonts.

## Phase 2 — screens, per app (rebuild from the endpoint list)
- Tenant: PG search/list (`/api/pg`, `/api/favourite-pgs`, `/api/city`), bookings (`/api/bookings`),
  payments (`/api/payment` + Razorpay), issues (`/api/issues`), notices, reviews, plans.
- Owner: PGs & rooms (`/api/pg/vendor`, `/api/vendor`), tenants (`/api/tenant`), expenses
  (`/api/pg-expense`), wardens (`/api/warden`), attendance (`/api/wardan-attendance`), amenities, banners.
- Warden: attendance (`/api/wardan-attendance`), tenants, issues, notices.

## Phase 3 — integrations
Firebase project (push/crashlytics), Razorpay key_id from backend, Google Maps key (restricted),
KYC via ML Kit. You need the Firebase and Razorpay account credentials — get these transferred to you.

## Phase 4 — release (you own the keys)
Generate NEW signing keystores for all three packages, keep them safe (or use Play App Signing),
set up the GitHub Actions APK build (same as the Stayyo starter repo), publish.

## Hand-off you must still collect (not in these APKs)
- Backend source + database + hosting access (the `stayyo.in` API).
- Firebase project ownership, Razorpay account, Google Cloud project/key ownership.
- Any Play Store / App Store listing ownership.
