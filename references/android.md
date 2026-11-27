# Android — the official way, and nothing past it

Read only when the gate below passes: this is a hub, not a tutorial. It names the official skill that owns each job and the decision that job forces; the skills themselves, and `developer.android.com`, are the depth. Google's own framing is that they cover the work models get wrong rather than the basics — so do not paraphrase them into vaguer advice.

## The gate

Three answers, one line each, before any Android act.

1. **Is this Android?** A marker in the project — `settings.gradle[.kts]`, `AndroidManifest.xml`, `gradlew`, `app/src/main/…`, `gradle/libs.versions.toml` — or the user asking for an Android app. Quote the marker. A web page that mentions Android is not a trigger; build that page as web work.
2. **Which surface?** The module, the form factor (phone and tablet by default; wear, tv, cars, xr only when the module says so), and the stack as it is: Compose or Views, DI, navigation, `minSdk`, `targetSdk`, AGP. Read them, never assume them.
3. **What proves it?** The Gradle task that builds, the test task that runs, and a render path — emulator, device, or Robolectric. With no render path the visual case is UNVERIFIED, and saying so is the correct answer.

## Read the project once

Four files answer the stack, and guessing it wastes the rounds that follow.

- `gradle/libs.versions.toml`, or the build files: Compose BOM, AGP, Kotlin, `minSdk`, `targetSdk`, DI, navigation, test libraries.
- `AndroidManifest.xml`: exported components, permissions, deeplink filters.
- The sourceset shape: one app module or several, Compose or Views, where the theme lives.
- The test setup: JUnit version, mock or fake, Robolectric, a screenshot tool, Espresso or Compose test.

## The official routes

| Task | Official skill | The decision it enforces |
| --- | --- | --- |
| Scaffold a project, or run the Android tooling | `android-cli` | the official template, not a hand-built Gradle tree |
| Gradle, AGP, build configuration | `agp` | the project's AGP and catalog decide, no side upgrades |
| Compose UI, theme, state | `jetpack-compose/*` | Compose and Material 3 before anything custom |
| Compose performance | `jetpack-compose-performance` | recomposition is measured, not guessed |
| Views to Compose | `jetpack-compose/migration` | migrate per screen, never as one rewrite |
| Adaptive layouts, phone to tablet | `adaptive` | window size classes, not a single width |
| Navigation | `navigation-3` | one graph, typed routes |
| Test strategy and harnesses | `testing-setup` | fakes before mocks, the ladder below |
| App size, R8, keep rules | `r8-analyzer` | R8 decides optimisation; a keep rule needs a reason |
| ANR, jank, startup | `anr-audit-and-fix` | find the stall before changing code |
| Profiling a running app | `android-profiler` | a trace, not a guess |
| Edge-to-edge, insets | `edge-to-edge` | draw under the bars, pad with insets |
| App widgets | `glance-*` | Glance and its own layout rules |
| Media playback and casting | `media3-cast-integration` | Media3, one player |
| Camera | `camerax` | CameraX lifecycle, not a raw camera |
| Permissions | `android-permissions-security` | ask at use, never at launch |
| Intents and deeplinks | `android-intent-security` | explicit where possible, exported false by default |
| Native code | `android-jni-security` | validate at the JNI boundary |
| Sign-in and credentials | `identity/*` | the system credential flow |
| In-app purchases and policy | `play/*` | Play Billing, and the policy page read |
| Wear | `wear-*` | the Wear surface is not a shrunk phone |
| TV | `leanback-to-compose-tv-migration` | Compose for TV, focus first |
| Cars | `android-for-cars-parked-apps` | the parked-app template |
| XR and glasses | `xr/*` | that surface and its own hardware |
| On-device AI | `litert`, `appfunctions`, `mlkit-*` | the on-device path, not a server call |
| iOS to Android | `ios-to-android` | a port keeps behaviour, not the framework |

If a row's skill is installed, read it before the work: it is the depth this file summarises. If it is not, read the page it points at on `developer.android.com`, and say in the report which one you used. Never install a skill unasked — the command belongs to the user.

## The floors

Eight lines that hold in every Android task, whatever the route.

- Kotlin, and Compose for anything new. Views only because the project already has them, or because the task is the migration.
- Material 3 (`androidx.compose.material3`) before a custom theme, with the system's dynamic colour where it is on.
- Edge-to-edge with insets (`enableEdgeToEdge`, `WindowInsets`), never padding that fakes a bar.
- Window size classes instead of a phone width; one layout that adapts beats two that fork.
- One navigation library, one DI library, one async library — the ones in the catalog.
- State in the ViewModel, unidirectional: a composable reads state and emits events, it does not fetch.
- Every dependency through the version catalog; a new one is a decision the user makes.
- R8, baseline profiles and `targetSdk` left as the project has them unless the task is exactly that.

## The fence

Android moves faster than any file, so the fence is where the guidance stops.

- No API, annotation, version or Gradle flag from memory. Project files, the installed official skill, then `developer.android.com` — and an unread one is UNVERIFIED.
- No library the project does not already have, unasked. "Recommended by a blog" is not a reason.
- Never bump AGP, `minSdk` or `targetSdk` as a side effect. Official screenshot testing says the same about AGP; the rule is general.
- Never present a community pattern, a StackOverflow answer or another platform's idiom as Google's guidance.
- When the official guidance is silent, say unknown and ask. An invented signature is worse than a gap.
- Never state a Play requirement from memory: the target API level and policy change on a schedule, so the policy page is read in the same session.

## Prove it

- Build: `./gradlew assembleDebug` or the project's task, then `lintDebug`.
- Tests: `testDebugUnitTest`, then `connectedDebugAndroidTest` when a device or emulator exists.
- Render: the screen at compact, medium and expanded — 400, 610 and 900 dp wide — plus one 1.5 font-scale pass and every theme. Android's V-case viewport is the window class and its size, not 390 and 1440.
- Interaction: back gesture, rotation with state restored, and the insets visible at the top and bottom of the screen.
- Performance claim: a trace or a benchmark, never a stopwatch impression.

## Never

- Give Android advice in a workspace that is not Android.
- Write a call the session has not verified.
- Upgrade the toolchain to make a feature fit.
- Ship a whole Compose rewrite in one change.
- Call a successful build a running app.
- Claim a screenshot without a device or Robolectric behind it.
- Quote a Play policy without the page in front of you.

<!-- source: android/skills (Apache-2.0), developer.android.com — 37 skills across 19 domains, distilled not copied -->