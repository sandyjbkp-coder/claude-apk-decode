# Claude APK Providers Analysis (v1.261120.19)

## Provider List Analysis

### 1. Core Android Providers
| Name | Authority | Exported | Grant URI Permissions | Key Details |
|------|-----------|----------|------------------------|-------------|
| `androidx.core.content.FileProvider` | `com.anthropic.claude.provider` | false | true | File sharing provider with `android.support.FILE_PROVIDER_PATHS` (resource @0x7f170008) |
| `androidx.startup.InitializationProvider` | `com.anthropic.claude.androidx-startup` | false | false | Initialization provider with initOrder=200 |

### 2. Third-Party Service Providers
| Name | Authority | Exported | Key Details |
|------|-----------|----------|-------------|
| `com.plaid.internal.webview.PlaidFileProvider` | `com.anthropic.claude.com.plaid.internal.webview.PlaidFileProvider` | false | File sharing for Plaid integration with `android.support.FILE_PROVIDER_PATHS` (resource @0x7f17000c) |
| `org.jetbrains.compose.resources.AndroidContextProvider` | `com.anthropic.claude.resources.AndroidContextProvider` | false | Compose resources provider, enabled=true |
| `io.sentry.android.core.SentryInitProvider` | `com.anthropic.claude.SentryInitProvider` | false | Sentry error tracking initialization |
| `io.sentry.android.core.SentryPerformanceProvider` | `com.anthropic.claude.SentryPerformanceProvider` | false | Sentry performance monitoring |
| `com.datadog.android.profiling.DdProfilingContentProvider` | `com.anthropic.claude.provider.datadog.profiling` | false | Datadog performance profiling |
| `com.datadog.android.rum.DdRumContentProvider` | `com.anthropic.claude.provider.datadog.rum` | false | Datadog RUM (Real User Monitoring) |
| `com.google.mlkit.common.internal.MlKitInitProvider` | `com.anthropic.claude.mlkitinitprovider` | false | ML Kit initialization provider |

### 3. Key Provider Characteristics
- **File Sharing**: 2 providers (FileProvider, PlaidFileProvider) with `grantUriPermissions=true`
- **Security**: All providers have `exported=false` (secure by default)
- **Init Order**: SentryPerformanceProvider (100), DatadogRum (999), DatadogProfiling (1000)
- **Compose Integration**: AndroidContextProvider enabled for Compose resources
- **Third-Party Integrations**: 
  - Plaid (payment/financial services)
  - Sentry (error tracking)
  - Datadog (monitoring)
  - Google ML Kit (ML features)
  - Google ML Kit + AndroidX Startup integration

### 4. Security & Compliance
- All providers use `android:exported=false` (secure default)
- File providers grant URI permissions for controlled sharing
- No exported providers with sensitive permissions
- Proper initOrder values for startup sequence

This analysis shows Claude uses standard Android providers with third-party integrations for analytics, error tracking, and file sharing.
