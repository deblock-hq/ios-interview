## Stack Assumptions (Deblock iOS)

- **Swift 5.9+**, **iOS 16.4+**
- **SwiftUI** for UI; UIKit only where required (e.g. legacy or system integration)
- **Swift Package Manager (SPM)** for the main shared library; Xcode project for the app target
- **Factory** (hmlongco/Factory) for dependency injection; composition root in the app
- **SwiftGen** for generated code: `L10n` (localized strings)
- **async/await** and **Swift Concurrency**; `@MainActor` for UI and observable state
- **fastlane** for build, test, and metadata

If the repository differs, keep the same architectural intent and adapt only stack-specific syntax.

---

## iOS Coding Standards

### Design & Structure

- Prefer **SwiftUI** and **value types**; use reference types only for shared mutable state (e.g. `ObservableObject` stores).
- Follow **SOLID** and composition at the **composition root** (app layer); use adapters, decorators, and factories as in the architecture docs.
- Keep **feature-driven** layout: group by feature/flow (e.g. Dashboard/Home, Wallet, Onboarding).
- Prefer **explicit names**: `isPending`, `hasValidSession`, `displayCurrency`; avoid abbreviations except common ones (e.g. `id`, `url`).

### Naming Conventions (Mandatory)

- **Avoid generic names**: do not use `Utils`, `Helpers`, `Common`, `Shared`, `Manager` as standalone file or module names; use purpose-driven names (e.g. `AmountFormatting`, `SessionValidator`).
- **Use purpose-driven names**: every file and type should clearly indicate its role (e.g. `HomeAccountBalanceView`, `WalletProvider`, `HomeStore`).
- **Views**: suffix with `View` (SwiftUI) or `ViewController` (UIKit); **stores/state**: `*Store`, `*Provider`.
- **Modules/targets**: PascalCase and domain-prefixed.
- **Assets, strings, animations**: use SwiftGen-generated symbols (`L10n.*`); do not hardcode keys or asset names in view code.

### Module & Target Ownership

- **Core** – Foundation types, extensions, no UI or network.
- **Model** – Domain models and DTOs; depends on Base and Localisation where needed.
- **DI** – Factory container and registration helpers; minimal dependencies.
- **Core** – Business logic, flows, stores; may depend on WebService, Format, Locale, UI-related packages only where necessary.
- **UI** – Feature UI (screens, flows); depends on Core, UIComponents, Wallet, etc.
- **UIComponents** – Reusable UI building blocks (atoms/molecules); depends on Format, SharedUI.
- **Localisation** – Strings and SwiftGen output; single source of truth for copy.
- **WebService** / **Data** – Domain-specific data and services.

Respect dependency direction: UI → Core → Data/WebService → Model/Base; no reverse dependencies.

### SwiftUI & Concurrency

- **Stores and UI-bound state**: annotate with `@MainActor` and use `ObservableObject` (or `@Observable` when migrating); keep state ownership clear (store vs view).
- **Use-case views**: thin views that observe a store or receive data; put orchestration and side effects in the store.
- **async/await**: prefer structured concurrency; avoid raw completion handlers in new code. Use `Task { }` and `async let` where appropriate.
- **Main thread**: all UI updates and observable mutations must happen on the main actor; use `@MainActor` or `MainActor.assumeIsolated` only when justified.

### Dependency Injection (Factory)

- Register dependencies at the **composition root** (app startup) if the dependency needs proper implementation; avoid ad-hoc resolution inside feature code where avoidable.
- Do not hold `Container.shared` or raw `Factory` in long-lived types; resolve once at construction or use explicitly injected dependencies.

### Localisation & Assets

- **No hardcoded user-facing strings** in UI code; use `L10n` (SwiftGen) with the appropriate key. Add new keys to the source `.strings` and regenerate.
- **Images and colors**: use asset catalogs and reference (or project-specific generated enums); do not use string-based `Image(name:)` for catalog assets.
- **Pluralisation and parameters**: use the generated `L10n` APIs that take parameters (e.g. `L10n.Home.FeedUpcoming.nextDays(number.string)`).

### Code Quality Tools

- **SwiftLint**: run as part of CI; follow project rules (line length, force unwraps, etc.).
- **SwiftGen**: run `swiftgen-all.sh` (or equivalent) after changing strings or asset catalogs; do not edit generated files by hand.
- **Tests**: use XCTest; run the relevant test plan (e.g. CI_iOS.xctestplan) before pushing. Prefer testing behaviour and public APIs.

---

## Security Rules (Mandatory)

### No Secrets in Code or Logs

- Never log or embed API keys, tokens, or private keys in source or in app bundles.
- Use xcconfig / environment / secure storage for secrets; access only through dedicated infrastructure code.

### Keychain & Sensitive Data

- Store sensitive data (e.g. wallet keys, tokens) in Keychain with appropriate accessibility and access control.
- Do not expose key material to the UI layer or to analytics/logging.

### App Transport & Certificates

- Use HTTPS only; follow pinning and certificate handling as per project security docs.
- Do not disable ATS or weaken security for convenience in production builds.

---

## Testing Rules

### Unit Tests

- Test business logic and state transitions in stores and use cases; mock dependencies via protocols or test doubles.
- Prefer deterministic tests (fixed clock, controlled async, no real network).

### UI & Previews

- Use SwiftUI previews for rapid iteration; keep previews buildable and, when possible, representative of real data.
- For critical flows, add UI or integration tests as defined in the test plan.

### Test Plans

- Use the project test plan (e.g. `CI_iOS.xctestplan`) for CI; ensure new code is covered by the appropriate target and scheme.

---