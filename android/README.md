# FocusAI Android Wrapper

This Android project wraps the FocusAI web app in a native WebView. It does not replace the Vercel frontend or Railway backend.

## Open in Android Studio

1. Open Android Studio.
2. Choose `File > Open`.
3. Select:

```text
C:\Users\prane\COHORT-HARKIRAT\FocusAI\android
```

4. Let Gradle sync finish.
5. Connect your Android phone with USB debugging enabled, or start an emulator.

## Run Deployed App

Use this variant for the live Vercel/Railway app:

```text
Build Variant: deployedDebug
Run target: your phone or emulator
```

It loads:

```text
https://focusai-nine.vercel.app
```

## Run Local App on Emulator

Start the web app locally:

```bash
cd C:\Users\prane\COHORT-HARKIRAT\FocusAI\backend
npm run dev
```

```bash
cd C:\Users\prane\COHORT-HARKIRAT\FocusAI\frontend
npm run dev -- --host 0.0.0.0 --port 5174
```

In Android Studio:

```text
Build Variant: localEmulatorDebug
```

The emulator loads:

```text
http://10.0.2.2:5174
```

When the web app is opened from `10.0.2.2`, FocusAI automatically sends API requests to:

```text
http://10.0.2.2:8001
```

## Run Local App on Physical Phone

1. Make sure your laptop and phone are on the same Wi-Fi.
2. Find your laptop IPv4 address:

```powershell
ipconfig
```

3. Edit `android/app/build.gradle` and replace this value:

```gradle
buildConfigField "String", "WEB_APP_URL", "\"http://192.168.1.10:5174\""
```

with your laptop IP, for example:

```gradle
buildConfigField "String", "WEB_APP_URL", "\"http://192.168.0.105:5174\""
```

4. Start frontend with host enabled:

```bash
cd C:\Users\prane\COHORT-HARKIRAT\FocusAI\frontend
npm run dev -- --host 0.0.0.0 --port 5174
```

5. In Android Studio:

```text
Build Variant: localDeviceDebug
Run target: connected phone
```

When the web app is opened from your laptop IP, FocusAI automatically sends API requests to the same IP on port `8001`.

## Notes

- Login/session storage works through WebView DOM storage and cookies.
- Back button navigates inside the web app first.
- The deployed build loads the HTTPS Vercel app.
- Local builds allow HTTP so Android Studio can run against your laptop development servers.
