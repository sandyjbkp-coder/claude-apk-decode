# Claude APK Structure Analysis (v1.261120.19)

## Package Information
- **Package Name**: com.anthropic.claude
- **Version**: 1.261120.19 (versionCode: 26112019)
- **Compile SDK**: 37 (Android 17)
- **Target SDK**: 37
- **Min SDK**: 32

## Permissions (Key Permissions)
- INTERNET
- ACCESS_NETWORK_STATE
- VIBRATE
- RECORD_AUDIO
- ACCESS_FINE_LOCATION
- ACCESS_COARSE_LOCATION
- READ_CALENDAR
- WRITE_CALENDAR
- DETECT_SCREEN_CAPTURE
- HIDE_OVERLAY_WINDOWS
- Health API permissions (READ_STEPS, READ_DISTANCE, READ_ACTIVE_CALORIES_BURNED, READ_TOTAL_CALORIES_BURNED, READ_FLOORS_CLIMBED, READ_ELEVATION_GAINED, READ_WHEELCHAIR_PUSHES, READ_SPEED, READ_STEPS_CADENCE, READ_CYCLING_PEDALING_CADENCE, READ_POWER, READ_EXERCISE, READ_WEIGHT, READ_HEIGHT, READ_BODY_FAT, READ_LEAN_BODY_MASS, READ_BONE_MASS, READ_BODY_WATER_MASS, READ_BASAL_METABOLIC_RATE, READ_HEART_RATE, READ_RESTING_HEART_RATE, READ_HEART_RATE_VARIABILITY, READ_BLOOD_PRESSURE, READ_BLOOD_GLUCOSE, READ_OXYGEN_SATURATION, READ_RESPIRATORY_RATE, READ_BODY_TEMPERATURE)

## Main Components

### Main Activities
1. **MainActivity** - `com.anthropic.claude.mainactivity.MainActivity` (LAUNCHER)
2. **AppOnlyMainActivity** - `com.anthropic.claude.mainactivity.AppOnlyMainActivity`
3. **AssistantOverlayActivity** - `com.anthropic.claude.mainactivity.AssistantOverlayActivity`
4. **AppOnlyAssistantOverlay** - `com.anthropic.claude.mainactivity.AppOnlyAssistantOverlay`

### Deep Link Activities
- NewWindowActivity - `com.anthropic.claude.deeplink.NewWindowActivity`
- CallWindowActivity - `com.anthropic.claude.deeplink.CallWindowActivity`
- OpenWithClaude - `com.anthropic.claude.deeplink.OpenWithClaude`
- SecretLinksInClaudeTask - `com.anthropic.claude.deeplink.SecretLinksInClaudeTask`
- PasswordManagerReturnActivity - `com.anthropic.claude.deeplink.PasswordManagerReturnActivity`
- SecretLinkActivity - `com.anthropic.claude.deeplink.SecretLinkActivity`
- DeepLinkActivity - `com.anthropic.claude.deeplink.DeepLinkActivity`
- BringToFrontActivity - `com.anthropic.claude.deeplink.BringToFrontActivity`

### Services
- VoiceModeService - `com.anthropic.claude.bell.VoiceModeService`
- VoiceRecognitionService - `com.anthropic.claude.bell.assist.ClaudeRecognitionService`
- VoiceInteractionService - `com.anthropic.claude.bell.assist.ClaudeVoiceInteractionService`
- VoiceInteractionSessionService - `com.anthropic.claude.bell.assist.ClaudeVoiceInteractionSessionService`
- TTSPlaybackService - `com.anthropic.claude.bell.tts.TTSPlaybackService`
- NotificationAnswerActivity - `com.anthropic.claude.notification.NotificationAnswerActivity`

## Key Features
1. **Deep Link Support**: Multiple activities for handling deep links
2. **Notification Handling**: NotificationAnswerActivity for handling notifications
3. **Voice Interaction**: Multiple services for voice recognition and TTS
4. **Widget Support**: WidgetPinnedReceiver, WidgetConfigActivity
5. **Location Services**: ACCESS_FINE_LOCATION and ACCESS_COARSE_LOCATION permissions
6. **Health Tracking**: Extensive health API permissions
7. **Background Services**: Multiple background services for various functions

## Technical Notes
- Uses AndroidX libraries extensively
- Multiple split APKs for different architectures (arm64_v8a, xxhdpi)
- Uses AndroidX WindowExtensions and Sidecar libraries
- Implements modern Android components and architecture

This structure represents the complete architecture of the Claude APK.
