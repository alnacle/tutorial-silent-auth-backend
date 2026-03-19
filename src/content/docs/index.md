---
title: "What We're Building"
description: "A high-level look at the 2FA with Silent Auth — what it does, how it works, and what you'll build across this tutorial."
---

# What We're Building

In this tutorial you'll build a complete **two-factor authentication (2FA) system** using [Vonage Verify v2](https://developer.vonage.com/en/verify/overview). It uses **Silent Authentication** as the primary channel — no OTP required when it works — and falls back to **SMS** when it can't.

By the end you'll have:

- A **Node.js backend** that manages the entire verification flow
- An **Android app** (Kotlin + Jetpack Compose) that drives the user experience
- A working end-to-end test confirming both paths

---

### Why two channels?

| Channel | How it works | User experience |
|--------|-------------|-----------------|
| **Silent Authentication** | Vonage checks the phone's mobile network connection — no action from the user | Invisible |
| **SMS OTP** | Vonage sends a one-time code by SMS — user types it in | Familiar fallback |

---

## Design principle: the backend owns everything

The Android app never calls Vonage directly. It never stores API keys. It only talks to **your** backend, which in turn talks to Vonage. This keeps secrets off the device and puts all business logic in one place.

```
Android App  ──→  Your Backend  ──→  Vonage Verify API
                       ↑
                  Vonage Callback (webhook)
```

---

## What you'll need

### Backend (GitHub Codespaces)

This tutorial runs in **GitHub Codespaces**. The backend terminal, nodemon, Node.js, and npm are all pre-configured — no local installation needed.

> **nodemon** is already installed in the Codespaces environment. It watches `server.js` and restarts the server automatically every time you save the file. You'll never need to manually stop and restart the server during this tutorial.

You will need:

- A [Vonage developer account](https://developer.vonage.com/sign-up) (free tier is fine)
- A phone number in **E.164 format** (e.g. `+34600111222`) to perform the verification

### Android app (local machine)

- [Android Studio](https://developer.android.com/studio) (stable channel)
- A real Android device running **Android 7.0+** (API 24 or higher) with a SIM and mobile data

> **Why a real device?** Silent Authentication relies on the mobile carrier network context. An emulator cannot replicate this. You can still use an emulator to test the SMS path, but Silent Auth will always fall back to SMS on an emulator.

---


## Tutorial structure

| # | Page | What you'll do |
|---|------|----------------|
| 01 | Backend: Project Init | Create `server/`, install deps |
| 02 | Backend: Minimal Server | First working `server.js`, first cURL test |
| 03 | Vonage Setup | Create Vonage Application, configure credentials |
| 04 | Backend: Vonage SDK | Add auth + Verify client |
| 05 | Backend: Verification Store | Add in-memory state map |
| 06 | Backend: `/verification` | Start a verification request |
| 07 | Backend: `/check-code` | Validate a code |
| 08 | Backend: `/callback` + `/next` | Webhook + fallback trigger |
| 09 | Android: New Project | Create project in Android Studio |
| 10 | Android: Dependencies | Add all required libraries |
| 11 | Android: Configuration | Permissions, backend URL |
| 12 | Android: Verify Setup | Minimal counter app smoke test |
| 13 | Android: UI States | State machine for the UI |
| 14 | Android: UI Layout | Full verification screen |
| 15 | Android: Networking | Backend API calls |
| 16 | Android: Silent Auth | Vonage Client SDK integration |
| 17 | End-to-End Testing | Full flow, common issues |
