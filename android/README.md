# SolidCraft for Android (APK)

**Maintainer:** [johnrcassidy](https://github.com/johnrcassidy). This directory plus `apps/solidcraft-android/` are the **Android port maintained on the `android` branch** of [johnrcassidy/solidcraft](https://github.com/johnrcassidy/solidcraft).

This tree is **not** the desktop app. It builds an **Android APK**: a native `.so` (`apps/solidcraft-android`) plus this Gradle shell (`android/`). The CAD UI and engine are the same Rust crates as desktop (`solidcraft-ui-egui`, `solidcraft-engine`), run through **eframe / winit NativeActivity** and **wgpu** (Vulkan, GLES fallback).

**Install (no build required):** published APKs on [johnrcassidy/solidcraft releases](https://github.com/johnrcassidy/solidcraft/releases) — latest **`0.22.1-android2`** (`v0.22.1-android2`). See repo root [ANDROID.md](../ANDROID.md).

Until something like this lands on [tangency-labs/solidcraft](https://github.com/tangency-labs/solidcraft) `main`, treat releases here as **this fork’s deliverable**, independent of upstream Android PRs ([#2412](https://github.com/tangency-labs/solidcraft/pull/2412), [#2658](https://github.com/tangency-labs/solidcraft/pull/2658)).

| Label | Meaning |
|--------|---------|
| `Cargo.toml` `version` | Engine/UI library line (matches the upstream release this branch synced from, e.g. `0.22.1`). |
| `versionName` in `app/build.gradle.kts` | What users see on the APK; suffix `-androidN` marks Android packaging builds (`android1`, `android2`, …). |
| `applicationId` | `io.archipelagos.solidcraft` — chosen in the **first** commit of this port for Android installs (publisher namespace for this fork only). |

## What this fork adds (paths)

| Path | Role |
|------|------|
| `android/` | Gradle app, `MainActivity.java`, launcher assets, JNI entry for pickers/IME/clipboard/secrets. |
| `apps/solidcraft-android/` | Rust shell: `SolveApp`, SAF file links, touch navigation, agent HTTP, SMB browser. |
| `Cargo.toml` `[profile.android-small]` | Smaller release `.so` for phones (LTO, `opt-level = "z"`). |
| `xtask/src/layers.rs` | `solidcraft-android` registered as **exempt** (app crate). |

## Modifications vs upstream desktop

Shared crates (minimal hooks only where Android has no desktop API):

- **`crates/ui-egui`**: `shell::on_model_view` (touch camera vs panels); agent chat paste skips **arboard** on Android (`arboard` has no Android backend).
- **Everything else in `crates/*`**: unchanged for Android unless pulled in by normal desktop development.

Android-only behaviour (see `apps/solidcraft-android` and `MainActivity.java`):

- **Files**: Storage Access Framework open/create/save; documents linked by URI; write-back via `bridge::Links`; open from other apps (`ACTION_VIEW`).
- **Network (SMB)**: saved connections (`connections.json`), passwords in Keystore, in-app SMB browse for open/save/show-in-folder (`smb_core`, `smb_ui`).
- **Input**: soft keyboard inset + IME composition; long-press text menu; `touch.rs` orbit/pan/pinch on the model view.
- **Agent chat**: HTTPS via `HttpURLConnection`; API keys in Keystore (`net.rs`). Clipboard images/files for chat are **not** supported on Android.
- **Not in the APK**: desktop CLI, MCP control server, update installer, slicer pickers, SpaceMouse, arboard-based paste.

Emulator-tested feature notes for older `0.19.0-android*` builds lived in `dist/android/PARITY.md` on branch `android-before-sync`; update or restore that file when you publish a fresh parity pass for `0.22.1-android*`.

## Build

Prerequisites: Android SDK, NDK, `rustup target add aarch64-linux-android x86_64-linux-android`, [cargo-ndk](https://github.com/bbqsrc/cargo-ndk).

```bash
# From repo root
cargo ndk -t arm64-v8a,x86_64 -o android/app/src/main/jniLibs build -p solidcraft-android --profile android-small
cd android && ./gradlew assembleRelease
```

APK: `android/app/build/outputs/apk/release/app-release.apk` (debug-signed release type in this tree). For sharing, attach the same binary to a GitHub Release as `solidcraft-<version>-androidN-universal.apk`.

## Contributing upstream

Do **not** open a second large Android PR without checking existing work ([#2412](https://github.com/tangency-labs/solidcraft/pull/2412), [#2658](https://github.com/tangency-labs/solidcraft/pull/2658)). Prefer folding ideas into the lowest-numbered active Android PR or coordinating in comments there.

When proposing a merge to `tangency-labs/solidcraft`, state explicitly that **official releases can include APKs** with the `X.Y.Z-androidN` naming scheme and that **`applicationId` may differ** from this fork’s `io.archipelagos.solidcraft`.
