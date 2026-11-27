# Platform

**Index** — Android · Figma · Libraries. Read the sections the routing row names, not the file.

## Android

The official way, and nothing past it. Read only when the gate below passes: this is a hub, not a tutorial. It names the official skill that owns each job and the decision that job forces; the skills themselves, and `developer.android.com`, are the depth. Google's own framing is that they cover the work models get wrong rather than the basics — so do not paraphrase them into vaguer advice.

### The gate

Three answers, one line each, before any Android act.

1. **Is this Android?** A marker in the project — `settings.gradle[.kts]`, `AndroidManifest.xml`, `gradlew`, `app/src/main/…`, `gradle/libs.versions.toml` — or the user asking for an Android app. Quote the marker. A web page that mentions Android is not a trigger; build that page as web work.
2. **Which surface?** The module, the form factor (phone and tablet by default; wear, tv, cars, xr only when the module says so), and the stack as it is: Compose or Views, DI, navigation, `minSdk`, `targetSdk`, AGP. Read them, never assume them.
3. **What proves it?** The Gradle task that builds, the test task that runs, and a render path — emulator, device, or Robolectric. With no render path the visual case is UNVERIFIED, and saying so is the correct answer.

### Read the project once

Four files answer the stack, and guessing it wastes the rounds that follow.

- `gradle/libs.versions.toml`, or the build files: Compose BOM, AGP, Kotlin, `minSdk`, `targetSdk`, DI, navigation, test libraries.
- `AndroidManifest.xml`: exported components, permissions, deeplink filters.
- The sourceset shape: one app module or several, Compose or Views, where the theme lives.
- The test setup: JUnit version, mock or fake, Robolectric, a screenshot tool, Espresso or Compose test.

### The official routes

| Task | Official skill | The decision it enforces |
| --- | --- | --- |
| Scaffold a project, or run the Android tooling | `android-cli` | the official template, not a hand-built Gradle tree |
| Gradle, AGP, build configuration | `agp` | the project's AGP and catalog decide, no side upgrades |
| Compose UI, theme, state | `jetpack-compose/*` | Compose and Material 3 before anything custom |
| Compose performance | `jetpack-compose-performance` | recomposition is measured, not guessed |
| Views to Compose | `jetpack-compose/migration` | migrate per screen, never as one rewrite |
| Adaptive layouts, phone to tablet | `adaptive` | window size classes, not a single width |
| Navigation | `navigation-3` | one graph, typed routes |
| Test strategy and harnesses | `testing-setup` | fakes before mocks, the ladder in `code.md` § Ladder |
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

### The floors

Eight lines that hold in every Android task, whatever the route.

- Kotlin, and Compose for anything new. Views only because the project already has them, or because the task is the migration.
- Material 3 (`androidx.compose.material3`) before a custom theme, with the system's dynamic colour where it is on.
- Edge-to-edge with insets (`enableEdgeToEdge`, `WindowInsets`), never padding that fakes a bar.
- Window size classes instead of a phone width; one layout that adapts beats two that fork.
- One navigation library, one DI library, one async library — the ones in the catalog.
- State in the ViewModel, unidirectional: a composable reads state and emits events, it does not fetch.
- Every dependency through the version catalog; a new one is a decision the user makes.
- R8, baseline profiles and `targetSdk` left as the project has them unless the task is exactly that.

### The fence

Android moves faster than any file, so the fence is where the guidance stops.

- No API, annotation, version or Gradle flag from memory. Project files, the installed official skill, then `developer.android.com` — and an unread one is UNVERIFIED.
- No new library unasked — `## Libraries` states the rule once, with its reasons.
- Never bump AGP, `minSdk` or `targetSdk` as a side effect. Official screenshot testing says the same about AGP; the rule is general.
- Never present a community pattern, a StackOverflow answer or another platform's idiom as Google's guidance.
- When the official guidance is silent, say unknown and ask. An invented signature is worse than a gap.
- Never state a Play requirement from memory: the target API level and policy change on a schedule, so the policy page is read in the same session.

### Prove it

- Build: `./gradlew assembleDebug` or the project's task, then `lintDebug`.
- Tests: `testDebugUnitTest`, then `connectedDebugAndroidTest` when a device or emulator exists.
- Render: the screen at compact, medium and expanded — 400, 610 and 900 dp wide — plus one 1.5 font-scale pass and every theme. Android's V-case viewport is the window class and its size, not 390 and 1440.
- Interaction: back gesture, rotation with state restored, and the insets visible at the top and bottom of the screen.
- Performance claim: a trace or a benchmark, never a stopwatch impression.

### Never

- Give Android advice in a workspace that is not Android.
- Write a call the session has not verified.
- Upgrade the toolchain to make a feature fit.
- Ship a whole Compose rewrite in one change.
- Call a successful build a running app.
- Claim a screenshot without a device or Robolectric behind it.
- Quote a Play policy without the page in front of you.

## Figma

Read before touching any Figma link. The file is the source of truth; the screenshot is the target; neither is an asset for the build.

### Gate

Check the catalog for Figma MCP tools. Absence is not proof — lazily attached servers get one bounded attempt with a known name first.

If they are genuinely absent, say what to add — the Figma MCP server in the client's MCP configuration — and end the turn. No scaffolding, no approximation from a screenshot, no partial work. The design source is missing, so everything downstream would be invention.

Then, once per session, enumerate the Figma tools and record four roles: read nodes, render a screenshot, download assets, write nodes. Honour any tool whose description declares a mandatory companion skill. Write the map down; re-deriving it every turn is the token leak.

### Read the design

- **Screenshot first.** Request the render in the initial design call. If none comes back, call the screenshot tool directly before editing. That render, not the node tree, is what the build is compared against.
- **Sparse is not a result.** A response flagged sparse cannot be implemented from. Correlate its child node IDs against the screenshot, then request the visible children in **one parallel batch**.
- **Traverse to the leaves.** Resolve the link to its frame, enumerate children, recurse. Mark every node covered, skipped with a reason, or missing. Done means the ledger accounts for every visible node — "I got the main sections" is a failed traversal.
- **Past the first viewport.** Long frames continue below the fold. Overlapping and scroll-revealed nodes are nodes. Variant states — hover, focus, empty, error — are nodes.
- **Never re-request a node already in the ledger.** Batch independent reads: siblings, assets and screenshots together. Cost here is round trips, not payload.

The ledger is a `## Nodes` block at the top of `.cykeek/verify/<task>.md`:

```
frame: Dashboard 1440x1024
- [x] Header / nav            3 nodes
- [x] KPI row                 4 cards
- [ ] Chart panel             sparse -> 12 children requested
- [~] Footer                  not in this design, skipped
```

### Build

- **Inspect before editing.** Read the likely project paths and the installed design dependencies for matching components, assets and tokens *first*. Reuse or compose a match; write raw markup, inline styles or hardcoded values only when nothing can express the design.
- **Adapt, don't paste.** Returned code is a high-fidelity non-interactive prototype. Translate absolute positioning into the project's layout system unless fixed positioning is the only faithful option, and make the interactive parts interactive.
- **Code Connect** applies precisely at its mapped node: reuse the connected component unless it truly cannot be configured or extended. Styling effort is not an exception.
- **Assets are exact.** Every visible static asset — image or SVG — goes in the exact slot it occupies in the design, substituted only for an exact match already in the codebase. Never omit, redraw, extract paths from, or inline one. Keep API- and data-supplied imagery dynamic.
- **Download every asset** as the design response describes, so no temporary Figma URL survives in the code. Do not reach for other tools to download. Supplied SVGs carry root `width`/`height` that must not be overridden when changing wrappers — no blanket `100% × 100%`. Avoid reading asset bytes unless the task requires it.
- **Build in dependency order**: tokens, primitives, components, shell, screen.

### Verify

- Verify only the requested screen or component. Record — do not alter — pre-existing mismatches outside it.
- Every visible asset is checked four ways: a non-empty local file, the right design slot, the right callsite, and correct rendered geometry. One mismatched or misproportioned asset is a FAIL.
- Compare against the screenshot render, add a row per frame or section to `.cykeek/verify/<task>.md`, and iterate from the first case until all read PASS or the user approves the remainder.

## Libraries

Read before hand-rolling something a library already does well, or when the project already uses one.

### Choose before you build

A toast, drawer, dropdown, dialog, combobox or command menu is weeks of edge cases — focus trapping, escape handling, scroll lock, collision-aware positioning, interruptible motion. Use the library. Hand-rolling is how a `<div>` dropdown with no focus management ships.

- Default primitives: Radix or Base UI. Default styling layer: shadcn/ui. Default sheet: Vaul.
- Choose one system per project and stay in it. Adding a second component library doubles the tokens, the bundle and the inconsistency.
- No library the project does not already have, unasked. "Recommended by a blog" is not a reason. The project's existing components win over any of this; check before installing anything.

### shadcn/ui

- It is a **registry**, not a dependency: components are copied into your repo and then belong to you. Edit them freely; do not expect upstream updates.
- `components.json` defines aliases, style and tokens. Read it before adding anything.
- Add with the CLI so paths, aliases and dependencies stay consistent. Do not hand-copy from docs.
- Style through CSS variables and Tailwind tokens, not by patching every component. If you must patch, patch the one component that needs it.
- A design system or `DESIGN.md` maps onto the token layer first — colour, radius, spacing — and only then onto components.

### Toasts (Sonner)

- Mount `<Toaster />` once, at the root. Two toasters means duplicated toasts.
- `toast()` for a message; `toast.promise()` for async flow; `toast.loading()` then update by id rather than stacking messages.
- Give every toast a stable `id` when it can fire repeatedly, and `dismiss()` it explicitly.
- Toast styling comes from the Toaster's props and tokens — portal and z-index problems are why a toast hides behind a modal.
- Keep toast copy short and past tense; it reports, it does not explain.

### Vaul (sheets and drawers)

- Use it for any bottom sheet or drawer gesture. It encodes velocity-aware dismissal, overlay fading tied to drag progress, and the iOS timing curve.
- Mark interactive children `data-vaul-no-drag`.
- A small floating drawer overrides the transition to ~200ms; the 500ms iOS timing only reads right full-width and edge-attached.

### ui-sdk

When a project depends on `@ui-sdk/*`, compose its components rather than recreating behaviour it ships; when it lacks one, build the behaviour on `@ui-sdk/core` and keep accessibility, keyboard, focus and RTL intact. Check its documented patterns before inventing a local one.

<!-- source: the Android hub (android/skills, Apache-2.0, developer.android.com — 37 skills across 19 domains, distilled not copied), the Figma hub (Arlan Marat — figma-design-to-code, figma-use), and the libraries route (shadcn, ask-sonner, pick-ui-library, ui-sdk) — merged from references/android.md, references/figma.md, references/libraries.md -->
