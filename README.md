# Stayyo — recovered from APKs

Material recovered from the three shipped Stayyo APKs (built by the former vendor "sieora"),
after the contract was terminated. Owner: Sameer Ahamed.

| Folder | App | Package |
|---|---|---|
| `owner-vendor/`   | Stayyo PG Management (owner) | `com.stayyoVendor` |
| `tenant-user/`    | Stayyo Tenant                | `com.stayyoUser`   |
| `manager-warden/` | Stayyo Manager / Warden      | `com.stayyoWarden` |

## Read this first — what you have and what you don't

**You do NOT have the original editable source here, and it cannot be recovered from an APK.**
A built APK ships the app as compiled **Hermes bytecode**, not the original React Native
`.tsx`/`.jsx` files. These were decompiled back to pseudo-JavaScript (`decompiled.bundle.REFERENCE-ONLY.js`
in each folder), but that output:
- has no components, JSX, hooks, real names, or project structure,
- will **not** rebuild into an app,
- is for **reading the logic and endpoints only**, so you can re-implement correctly.

Editing the decompiled file and recompiling is not possible. Treat it like documentation.

## What IS directly reusable
- **`API-MAP.md`** (root) and `recovered/api-endpoints.md` (per app): the complete backend API
  surface — every `/api/...` route each app calls. This is the real spec of your backend.
- **`assets/images/`** and **`assets/fonts/`** per app: the actual icons/images/fonts shipped,
  reusable as-is in a new project.
- **`recovered/AndroidManifest.summary.txt`**: permissions, SDK levels, components, package name.

## Known issues already found (see the security report)
- Base URL is hardcoded to a dev tunnel `https://6zgbmww9-5012.inc1.devtunnels.ms` — must become `https://stayyo.in` via env config.
- Hardcoded Mongo IDs baked into calls.
- Google API key embedded (restrict it).
- `http://localhost` left in the build.

## The realistic path forward
Since the editable source is gone with the vendor, the way to "fix bugs and update by yourselves"
is to **rebuild the three apps as a fresh, clean React Native project you own** — using this repo
as the exact spec (endpoints, screens list, assets). See `REBUILD.md`.

The backend (the `stayyo.in` API and its database) is separate and may still be intact and yours —
confirm you have its source/hosting access too, because the apps are useless without it.
