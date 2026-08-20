# Barcode app — steps and learning notes

A running log of **what we did**, **what we plan**, and **what each library/class is for**.  
After every slice, append a new step plus notes under **Libraries** and **Classes / APIs**.

**How to use this file:** add a dated subsection under the slice you just finished. Do not rewrite old notes; append.

---

## Goal (short)

Beginner-friendly Expo + TypeScript app: live camera → scan any barcode the library supports → second screen showing **all fields** the scan returns.

**Out of scope unless asked later:** LabelScanner’s custom Kotlin CameraX/ML Kit pipeline, ROI overlay, JSON template, lot/expiry OCR, duplicate suppressor.

---

## Constraints

- Expo unless there is a strong reason for bare React Native.
- No custom Kotlin / Java / Swift in **this** repo. Native code inside `node_modules` (autolink) is OK.
- Android phone is the main target; camera permission when we add the camera.
- Simple UI: scan screen, then details.

---

## Planned approach

| Slice | What | Status |
| ----- | ---- | ------ |
| **0** | Empty Expo TypeScript project (this folder) | **Done** |
| **1** | Two screens + navigation + **fake** scan result (no camera) | Not started |
| **2** | Camera + real barcode scans (`expo-camera`) | Wait until you say go |
| **3** | Polish details UI (show every field the library exposes) | After slice 2 |

**Library choice (for slice 2, not installed yet):** [`expo-camera`](https://docs.expo.dev/versions/latest/sdk/camera/) barcode scanning. Maintained by Expo, TypeScript-friendly, no custom native code in this repo.

**Intended folder layout after slice 1** (not created yet):

```
app/                 # Expo Router (file-based screens)
  _layout.tsx        # stack: Scan → Details
  index.tsx          # Scan screen
  details.tsx        # Details screen
```

Until slice 1 we still have the blank template: `App.tsx` + `index.ts`.

---

## Steps taken

### Slice 0 — empty Expo app (2026-08-20)

1. Confirmed this folder was empty except `instructions.txt`. Did **not** touch LabelScanner.
2. Temporarily moved `instructions.txt` so `create-expo-app` would accept a non-empty directory.
3. Ran `npx create-expo-app@latest . --template blank-typescript --yes`.
4. Restored `instructions.txt`.
5. Stopped. No navigation, no camera, no barcode library.

**Resulting stack:** Expo SDK ~57, React Native 0.86, React 19, TypeScript.

**Main files from the template:**

| File | Role |
| ---- | ---- |
| `package.json` | Scripts (`expo start`, `android`) and dependencies |
| `app.json` | Expo app name, icons, Android/iOS/web config |
| `index.ts` | Registers `App` as the root component |
| `App.tsx` | The only screen: a centered “hello” `View` |
| `tsconfig.json` | TypeScript; extends Expo’s base config, `strict: true` |
| `assets/` | Default icon / splash images |

**Run later:** `npx expo start`, then open on Android with Expo Go.

---

## Libraries

Notes for **packages we actually use**. Add a new heading when we install something.

### `expo` (slice 0)

The Expo runtime and CLI. Lets us run a React Native app in **Expo Go** without writing native Android/iOS project files ourselves. Scripts in `package.json` call `expo start`.

- **`registerRootComponent`** (used in `index.ts`): tells React Native which component is the app root. Works the same in Expo Go and in a later native build.

### `expo-status-bar` (slice 0)

Thin wrapper around the phone status bar (time, battery icons). `StatusBar` in `App.tsx` is from this package, not from `react-native`.

### `react` (slice 0)

UI library. We write **components** (functions that return JSX). `App` is a default-export function component.

### `react-native` (slice 0)

The primitives that map to native Android/iOS views. We do not use HTML (`div`, `span`).

### `typescript` + `@types/react` (slice 0)

Type-check `.ts` / `.tsx` files. `strict: true` means TypeScript complains about missing types and nulls. Helps catch mistakes before the phone runs the app.

### Planned (not installed)

- **`expo-router`** — file-based navigation for slice 1.
- **`expo-camera`** — live camera + barcode scanning for slice 2.

---

## Classes / APIs

React Native mostly uses **components and functions**, not Java-style classes. These are the building blocks from slice 0.

### `App` (`App.tsx`)

Root component. Returns one tree of UI. Default export so `index.ts` can `import App from './App'`.

### `View` (`react-native`)

A box / container. Closest analog to a `div`. Layout is flexbox (`flex: 1`, `alignItems`, `justifyContent`).

### `Text` (`react-native`)

All visible strings must live inside `Text`. You cannot put raw text as a child of `View`.

### `StyleSheet.create` (`react-native`)

Defines styles once as a JS object (similar to CSS, different names: `backgroundColor` not `background-color`). `styles.container` is applied with `style={styles.container}`.

### `StatusBar` (`expo-status-bar`)

Controls status-bar appearance. `style="auto"` follows light/dark. Does not draw the main screen content.

---

## Next (wait for go)

**Slice 1:** Expo Router (or equivalent stack), Scan screen with a button that navigates using a **hardcoded** barcode object, Details screen that lists those fake fields. Still no camera.
