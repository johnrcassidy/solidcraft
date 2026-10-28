# SolidCraft for Android (APK)

**Maintainer:** [johnrcassidy](https://github.com/johnrcassidy) — this is an **unofficial Android port** on the `android` branch of [johnrcassidy/solidcraft](https://github.com/johnrcassidy/solidcraft). It is **not** the tangency-labs desktop app, web build, or any upstream merge from [PR #2412](https://github.com/tangency-labs/solidcraft/pull/2412) (separate upstream effort).

**Shipped installable APKs** (sideload): [GitHub Releases](https://github.com/johnrcassidy/solidcraft/releases) — tags `v*-android*`.

| Release | APK |
|---------|-----|
| **Latest** | [v0.22.1-android2](https://github.com/johnrcassidy/solidcraft/releases/tag/v0.22.1-android2) — `solidcraft-0.22.1-android2-universal.apk` (arm64 + x86_64) |
| Previous | [v0.22.1-android1](https://github.com/johnrcassidy/solidcraft/releases/tag/v0.22.1-android1) |

| | Desktop / web (upstream `main`) | This fork (`android`) |
|---|--------------------------------|------------------------|
| Install | MSI, DMG, Flatpak, browser | **`.apk`** from Releases (above) |
| Version label | `v0.22.1` etc. | **`0.22.1-android2`** (`versionName` in Gradle) |
| Android package id | — | `io.archipelagos.solidcraft` (see below) |

### Why `io.archipelagos.solidcraft`?

Android needs a unique **application id** (reverse-DNS). This port has used **`io.archipelagos.solidcraft` since the first native APK commit** (`34ac3c04`, 2026-10-21): a publisher namespace for this fork’s installs, not SolidCraft upstream’s desktop packaging. It does **not** mean tangency-labs or another contributor owns the port — the Rust/Java Android work in `apps/solidcraft-android/` and `android/` is maintained here. Upstream may pick a different id if they merge an official Android app later.

**Build and modification details:** [android/README.md](android/README.md)

## Reach SolidCraft maintainers (tangency-labs)

1. **Issue [#2828](https://github.com/tangency-labs/solidcraft/issues/2828)** — native Android (public thread for this fork).
2. **Existing Android PRs** — [#2412](https://github.com/tangency-labs/solidcraft/pull/2412), [#2658](https://github.com/tangency-labs/solidcraft/pull/2658); coordinate before opening another large port PR.
3. **[Discord](https://discord.gg/FtU4NQNVDH)** — short intro + links above if GitHub is quiet.
4. **[CONTRIBUTING.md](https://github.com/tangency-labs/solidcraft/blob/main/CONTRIBUTING.md)** — draft PR, `approved-for-ci`, one topic per change.

## Commits and tags (so work is not lost in history)

- **Commit subject:** start with `android:` (example: `android: SMB save upload fix`).
- **Commit body (optional):** `APK: 0.22.1-androidN | package io.archipelagos.solidcraft | not desktop`
- **Tags / releases:** `v0.22.1-androidN` — never `v0.22.1` alone on this fork.
- **Pull requests to upstream:** title starts with `Android:` and links this file.
