# Claude APK v1.261120.19 - Full Structure Decode Summary

## Package Details
- **Package Name**: `com.anthropic.claude`
- **Version Code**: 26112019
- **Version Name**: 1.261120.19
- **Compile SDK**: 37 (Android 17)
- **Target SDK**: 37
- **Min SDK**: 32
- **File Size**: ~65 MB

## Key Permissions
1. INTERNET
2. ACCESS_NETWORK_STATE
3. VIBRATE
4. RECORD_AUDIO
5. ACCESS_FINE_LOCATION
6. ACCESS_COARSE_LOCATION
7. READ_CALENDAR / WRITE_CALENDAR
8. DETECT_SCREEN_CAPTURE
9. HIDE_OVERLAY_WINDOWS
10. Health API: 24+ health permissions (steps, heart rate, distance, calories, floors, elevation, wheelchair, power, exercise, weight, height, body fat, lean mass, bone mass, water mass, BMR, heart rate, resting HR, HRV, blood pressure, blood glucose, oxygen saturation, respiratory rate, body temperature)
11. POST_NOTIFICATIONS
12. BILLING (Google Play Billing)
13. RECEIVE_BOOT_COMPLETED
14. MODIFY_AUDIO_SETTINGS
15. FOREGROUND_SERVICE / FOREGROUND_SERVICE_MICROPHONE / FOREGROUND_SERVICE_MEDIA_PLAYBACK

## Launchable Activity
- **com.anthropic.claude.mainactivity.MainActivity** (LAUNCHER) - Label: "Claude"

## Key Activity Categories
### Main Activities (LAUNCHER category)
1. MainActivity - Primary user interface
2. AppOnlyMainActivity - Restricted/enterprise mode
3. AssistantOverlayActivity - Floating overlay
4. AppOnlyAssistantOverlay - Restricted overlay

### Deep Link Activities
1. NewWindowActivity - Open in new window
2. CallWindowActivity - Voice call window
3. OpenWithClaude - Share intent handler
4. SecretLinksInClaudeTask - Special task handler
5. PasswordManagerReturnActivity - Auto-fill return
6. SecretLinkActivity - Internal links
7. DeepLinkActivity - General deep links
8. BringToFrontActivity - Bring task to front

### Widget Activities
- ClaudeWidgetConfigActivity - Widget configuration
- Various widget receivers

## Key Services
1. **VoiceModeService** - Voice interaction entry point
2. ClaudeRecognitionService - Speech recognition
3. ClaudeVoiceInteractionService - Voice interaction
4. ClaudeVoiceInteractionSessionService - Voice session management
5. TTSPlaybackService - Text-to-speech playback
6. NotificationAnswerActivity - Notification response

## Key Receivers
- Boot receiver for startup
- Notification receivers
- Permission action receivers
- Update check receivers
- Widget receivers

## Key Providers
- FileProvider (androidx.core.content.FileProvider)
- Androidx Startup InitializationProvider
- Car app connection provider
- Firebase messaging service

## Libraries/Features
- AndroidX Window extensions
- AndroidX Sidecar
- Google Play Billing
- Firebase integration
- Material Design 3 components
- Multi-architecture support (arm64-v8a, armeabi-v7a, x86, x86_64)

The app is well-structured with separation of concerns between main UI, deep links, voice interactions, and widget functionality.
