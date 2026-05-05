# Section 10: Best Practices & Checklists

## 10.1 Pre-Launch Checklist

### Functionality
- [ ] All MVP features working end-to-end
- [ ] All API endpoints tested in production environment
- [ ] Deep links tested on fresh install
- [ ] Push notifications delivered and deep link correctly
- [ ] Onboarding flow tested by 5 non-team users
- [ ] In-app purchases / subscriptions tested (use test accounts)
- [ ] Logout / account deletion flow tested
- [ ] All empty states and error states display correctly
- [ ] App works on poor / no network connection
- [ ] App works across target Android versions (API 26–34)

### Performance
- [ ] Cold start < 2 seconds on mid-range device
- [ ] No ANRs in 30 minutes of QA testing
- [ ] Memory usage stable over 10 minutes of use (no leaks)
- [ ] Battery usage not flagged in Android Vitals testing
- [ ] APK/AAB size reviewed (target < 20MB for initial install)
- [ ] Images compressed and using WebP where possible

### Security
- [ ] No API keys / secrets in codebase (use BuildConfig from CI secrets)
- [ ] Auth tokens in EncryptedSharedPreferences
- [ ] Network calls over HTTPS only (enforce with Network Security Config)
- [ ] Root detection implemented (for sensitive apps)
- [ ] Certificate pinning implemented (for financial / health apps)
- [ ] No sensitive data in logs in release builds
- [ ] Input validation on all forms (client + server)
- [ ] SQL injection prevented (use parameterized queries / Room)
- [ ] WebView: JavaScript disabled unless required, no arbitrary URL loading

### Compliance
- [ ] Privacy policy live at public URL
- [ ] Terms of service live at public URL
- [ ] All permissions justified and documented
- [ ] Data safety section in Play Console accurate and complete
- [ ] GDPR: data deletion flow implemented (if EU users targeted)
- [ ] App rating (content rating questionnaire) completed
- [ ] Play Store listing reviewed against policies

### Store Listing
- [ ] App icon 512×512px uploaded
- [ ] Feature graphic 1024×500px uploaded
- [ ] Minimum 4 phone screenshots uploaded
- [ ] Short description: 80 chars, primary keyword included
- [ ] Long description: structured, no keyword stuffing
- [ ] App category correct
- [ ] Contact email live and monitored

---

## 10.2 Security Best Practices

### Android Security Checklist
```kotlin
// Network Security Config (res/xml/network_security_config.xml)
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">api.yourapp.com</domain>
    </domain-config>
</network-security-config>

// AndroidManifest.xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    android:allowBackup="false"  // Disable backup for sensitive apps
    ...>
```

| Area | Practice |
|------|---------|
| Storage | EncryptedSharedPreferences, never plain SharedPrefs for tokens |
| Deep links | Validate and sanitize all incoming URL parameters |
| WebViews | `setJavaScriptEnabled(false)` unless necessary |
| Exports | Mark non-exported components explicitly in Manifest |
| Proguard | Enable for release — obfuscates class names |
| Secrets | Never in version control — use CI environment variables |

---

## 10.3 Performance Optimization Tips

### Code Efficiency
```
✅ Use suspend functions for all I/O operations
✅ Collect flows in lifecycle-aware scope (repeatOnLifecycle)
✅ Use LazyColumn for lists > 20 items
✅ Use remember{} and derivedStateOf{} to reduce recompositions
✅ Hoist state to the highest necessary level only
✅ Cancel coroutines on ViewModel cleared (viewModelScope auto-cancels)

❌ Never call .collect() in ViewModel — use StateFlow
❌ Never create objects inside Composable function bodies
❌ Never load images from disk on main thread
❌ Never use blocking .execute() on Retrofit calls
```

### App Size Optimization
- Enable R8 / ProGuard (shrinks + obfuscates)
- Use `android:extractNativeLibs="false"` in Manifest
- Use WebP for all images (30% smaller than PNG)
- Use vector drawables instead of PNG where possible
- Use `splits { abi { enable true } }` in Gradle for native libs
- Use Play Asset Delivery for large assets (games, AR)

### Startup Optimization
```kotlin
// Use App Startup library for lazy initialization
class MyInitializer : Initializer<MyLibrary> {
    override fun create(context: Context): MyLibrary {
        return MyLibrary.initialize(context)
    }
    override fun dependencies() = emptyList<Class<out Initializer<*>>>()
}

// Avoid: Heavy initialization in Application.onCreate()
// Use: Lazy initialization — init only when first needed
```

---

## 10.4 Common Pitfalls to Avoid

### Architecture Pitfalls
| Pitfall | Consequence | Fix |
|---------|------------|-----|
| API calls in Composables | Untestable, lifecycle issues | Move to ViewModel |
| Context leaks in ViewModel | Memory leaks, crashes | Use ApplicationContext or avoid Context |
| Missing loading/error states | Bad UX, crashes | Always handle all 4 states: Loading/Success/Error/Empty |
| Repository calling other repositories | Circular dependencies | Use UseCases as orchestrators |
| Shared ViewModel between unrelated screens | Tight coupling | One ViewModel per screen/feature |

### Launch Pitfalls
| Pitfall | Consequence | Fix |
|---------|------------|-----|
| Launching without analytics | Flying blind, no retention data | Instrument before launch |
| Requesting all permissions on launch | Users deny, uninstall | Request at point of need |
| No staged rollout | Crash affects 100% of users | Always start at 10–20% |
| No response to Play reviews | Poor rating, no recovery | Set daily review check reminder |
| Hardcoded server URLs | Impossible to change without update | Use BuildConfig fields |
| Missing privacy policy | App rejection, legal risk | Required before submission |
| Ignoring Android Vitals | Play Store demotion | Fix before each major release |

### Team / Process Pitfalls
| Pitfall | Fix |
|---------|-----|
| No code review process | PR template + required reviewer |
| Committing secrets to git | git-secrets pre-commit hook |
| No error monitoring | Firebase Crashlytics from day 1 |
| Feature creep in MVP | Strict MoSCoW + changelog discipline |
| Skipping unit tests "to move fast" | Write tests for every UseCase minimum |
