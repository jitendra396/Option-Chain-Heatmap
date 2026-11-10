# Option Chain Pro — Android app (v13)

NSE option chain, OI charts, Live OI ± and Sector Heatmap as an installable Android app.
Everything (screens, charts, logic) is bundled inside the APK, so it installs and opens with
no internet. **Data refreshes automatically whenever the phone is online**; offline it keeps
showing the last saved data and refreshes by itself as soon as the connection returns.

## What is inside
| Screen | Contents |
|---|---|
| **Chain** | Spot + change, Prev Day (breakout/breakdown + H/L/C), PCR, Call/Put OI & change, OI bias with Support/Resistance, USD/INR, Dollar Index, India VIX, Nifty 50 / 500 Adv-Dec, full option chain (Compact / Full), CE/PE **Volume** (K/L/Cr) with **Top-1 volume** badge, Top-3 OI & Amount badges, ATM highlight, pinned **TOTAL** row |
| **OI Charts** | OI + Change from Opening (N / O / C labels, current-level tick, CE/PE under bars, ATM) · OI Change per Strike · Cumulative OI (values at dots, S / R) · OI Change Trend |
| **Live OI ±** | 5-minute history of combined CE/PE OI for ATM ±N (default ±2, adjustable 1–15) |
| **Heatmap** | Sector tiles coloured by % change with adv/dec, broad indices, India VIX |

Auto-refresh: 15 s – 5 min, pauses while the app is in the background, refreshes on reconnect
and when you return to the app. Light / Dark theme. Symbols: NIFTY, BANKNIFTY, FINNIFTY,
MIDCPNIFTY or any F&O stock (CUSTOM).

## Get the APK — easiest way (no Android Studio, nothing to install)
1. Create a **private** GitHub repository and upload this whole folder (keep the folder
   structure, including `.github/`).
2. Open the repo → **Actions** → **Build Android APK** → **Run workflow**
   (it also runs on every push).
3. After ~5 minutes open the finished run → **Artifacts** → download
   `OptionChainPro-v13-apk`, unzip, copy `app-release.apk` to the phone and tap it
   (allow "Install unknown apps" for your file manager / browser when asked).

## Build on your own PC instead
Requirements: Node 22+, JDK 21, Android SDK (Android Studio installs it).
```bash
npm install
npm test                 # logic tests
npx cap sync android     # copies www/ into the Android project
cd android
./gradlew assembleRelease        # Windows: gradlew.bat assembleRelease
# APK: android/app/build/outputs/apk/release/app-release.apk
```
Or `npx cap open android` and use **Build → Build APK(s)** in Android Studio.

## Notes
* **Signing key:** `android/app/ocp-release.jks` (password `optionchain`) is a personal key so
  updates install over older versions. Keep the repo private. For Play Store publishing create
  your own keystore and set `OCP_KS_PASS`.
* **Data sources:** NSE India public endpoints (chain, indices) and Yahoo Finance (USD/INR,
  DXY, previous-day range). NSE can block or throttle automated requests; the app then keeps the
  last data on screen, shows the reason and retries. Data can be delayed. Not investment advice.
* **Live OI ± history** is stored on the phone and only builds while the app is open.
* The app uses Capacitor's native HTTP (needed for NSE cookies and to avoid browser CORS).
  Opening `www/index.html` directly in a desktop browser will therefore not reach NSE.
* Not included yet: IV Analysis and the PCR-trend window from the desktop version.

## Project layout
```
www/                 the app (index.html, css/, js/core.js · nse.js · charts.js · app.js)
android/             generated Android project (icon, splash, signing already set)
tests/               core.test.js (logic) · ui.test.js (phone-size UI test with mocked NSE)
.github/workflows/   one-click cloud APK build
capacitor.config.json
```
