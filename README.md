# Claude APK — Full Structure Decode Report

**Package:** `com.anthropic.claude`  
**Version:** 1.261120.19 (versionCode 26112019)  
**Compile SDK:** 37 (Android 17) | **Target SDK:** 37 | **Min SDK:** 32  
**APK Size:** ~65 MB (base.apk)  
**Decoded Components:** 84 (Activities + Services + Receivers + Providers + Aliases)

---

## 📱 Package Details

| Property | Value |
|----------|-------|
| Package | `com.anthropic.claude` |
| Version Code | 26112019 |
| Version Name | 1.261120.19 |
| Compile SDK | 37 (Android 17) |
| Target SDK | 37 |
| Min SDK | 32 |
| APK Size | ~65 MB |

---

## 🔐 Permissions (Major)

- `INTERNET`, `ACCESS_NETWORK_STATE`, `VIBRATE`
- `RECORD_AUDIO`, `ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION`
- `READ_CALENDAR`, `WRITE_CALENDAR`
- `DETECT_SCREEN_CAPTURE`, `HIDE_OVERLAY_WINDOWS`
- Health API: 24+ permissions (steps, heart rate, distance, calories, oxygen, blood pressure, glucose, body temperature, etc.)
- `POST_NOTIFICATIONS`, `FOREGROUND_SERVICE` (microphone, media playback)
- `BILLING` (Google Play Billing)
- `RECEIVE_BOOT_COMPLETED`

---

## 🔑 Main Launchable Activity

```
com.anthropic.claude.mainactivity.MainActivity
```
Label: **Claude** — Yeh main activity hai jo launcher se shuru hoti hai.

---

## 🎯 Activities (Major)

| Category | Activity | Purpose |
|----------|----------|---------|
| **Launcher** | `.mainactivity.MainActivity` | Primary UI |
| | `.mainactivity.AppOnlyMainActivity` | Restricted/enterprise mode |
| | `.mainactivity.AssistantOverlayActivity` | Floating overlay |
| **Deep Links** | `.deeplink.NewWindowActivity` | Open in new window |
| | `.deeplink.CallWindowActivity` | Voice call window |
| | `.deeplink.OpenWithClaude` | Share intent handler |
| | `.deeplink.SecretLinkActivity` | Internal links |
| | `.deeplink.PasswordManagerReturnActivity` | Auto-fill return |
| | `.deeplink.BringToFrontActivity` | Bring task to front |
| **Widgets** | `.widget.ClaudeWidgetConfigActivity` | Widget config |

---

## ⚙️ Services (Major)

- `.bell.VoiceModeService` — Voice interaction entry
- `.bell.assist.ClaudeRecognitionService` — Speech recognition
- `.bell.tts.TTSPlaybackService` — Text-to-speech playback
- `.chat.MessageSseService` — SSE streaming chat

---

## 📡 Receivers

- Boot receiver (startup)
- Notification action receivers
- Permission approve/deny receivers
- Widget pinned receivers
- Update check receiver

---

## 📦 Providers

| # | Provider Name | Authority | Exported | Init Order | Type |
|---|---------------|-----------|----------|------------|------|
| 1 | `androidx.core.content.FileProvider` | `com.anthropic.claude.provider` | ❌ false | — | Main file sharing provider |
| 2 | `androidx.startup.InitializationProvider` | `com.anthropic.claude.androidx-startup` | ❌ false | 200 | AndroidX startup initialization |
| 3 | `com.plaid.internal.webview.PlaidFileProvider` | `com.anthropic.claude.com.plaid.internal.webview.PlaidFileProvider` | ❌ false | — | Plaid file sharing |
| 4 | `org.jetbrains.compose.resources.AndroidContextProvider` | `com.anthropic.claude.resources.AndroidContextProvider` | ❌ false | — | Compose resources |
| 5 | `io.sentry.android.core.SentryInitProvider` | `com.anthropic.claude.SentryInitProvider` | ❌ false | — | Error tracking init |
| 6 | `io.sentry.android.core.SentryPerformanceProvider` | `com.anthropic.claude.SentryPerformanceProvider` | ❌ false | 100 | Performance monitoring |
| 7 | `com.datadog.android.profiling.DdProfilingContentProvider` | `com.anthropic.claude.provider.datadog.profiling` | ❌ false | 1000 | Datadog profiling |
| 8 | `com.datadog.android.rum.DdRumContentProvider` | `com.anthropic.claude.provider.datadog.rum` | ❌ false | 999 | Datadog RUM monitoring |
| 9 | `com.google.mlkit.common.internal.MlKitInitProvider` | `com.anthropic.claude.mlkitinitprovider` | ❌ false | 99 | ML Kit initialization |
| 10 | `androidx.car.app.connection` | — | — | — | Car app connection provider |

All providers use `android:exported=false` (secure by default).

---

## 📂 Extracted / Decoded Files

| File | Description |
|------|-------------|
| `agent/claude_base.apk` | Extracted base APK (~65 MB) |
| `agent/claude_decode/` | Decode output directory |
| `agent/claude_manifest_dump.txt` | Full AndroidManifest.xml tree |
| `agent/claude_structure.md` | Detailed structure doc |
| `agent/claude_summary.md` | Summary report |
| `agent/providers_analysis.md` | Provider-level analysis |
| `agent/claude_components.txt` | Full component list |
| `agent/claude_manual/` | Full decompiled resource tree (res/, assets/, org/, META-INF/) |

---

## 📊 Component Count

Total **84 components** decoded from the manifest.

---

## 🔑 Key Features

- **Deep Link Support**: Multiple activities for handling deep links
- **Notification Handling**: NotificationAnswerActivity for handling notifications
- **Voice Interaction**: Multiple services for voice recognition and TTS
- **Widget Support**: WidgetPinnedReceiver, WidgetConfigActivity
- **Location Services**: ACCESS_FINE_LOCATION and ACCESS_COARSE_LOCATION permissions
- **Health Tracking**: Extensive health API permissions (24+)
- **Background Services**: Multiple background services
- **File Sharing**: 2 FileProviders with grantUriPermissions
- **Analytics/Monitoring**: Sentry, Datadog (RUM + profiling)
- **ML**: Google ML Kit integration
- **Payments**: Plaid integration, Google Play Billing

---

## 🛠 Repository Contents

This repository contains the complete decoded Claude APK project:

- `agent/` — Full decode output including APK, manifest dump, structure docs, component list, extracted resources (`claude_manual/`), logs, memory, skills, and capability-onboarding playbooks.
- Root — This README report.
