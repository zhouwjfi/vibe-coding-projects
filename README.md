# Vibe Coding Projects

[English](README.md) | [简体中文](README.zh-CN.md)

A log of the projects I build with AI-assisted (vibe coding) workflows.

- **Forked projects**: only what I added or changed on top of the project I forked
- **My own projects**: what the project actually implements

## Forked projects

### PiliNara — a Bilibili client based on PiliPlus

- Repo: [zhouwjfi/PiliNara](https://github.com/zhouwjfi/PiliNara)
- Upstream (the repo forked directly): [Starfallan/PiliNara](https://github.com/Starfallan/PiliNara)
- What I added:
  - **Cast to newBV**: on-demand discovery of DLNA devices on the LAN — scanning only when the TV button is tapped, with cached results and automatic re-scan on failure
  - TV cast entries on the video detail page, player page and the long-press cover sheet; the entry hides itself when no device is found
  - **Live casting**: the receiver reuses its own live player by room ID, keeping quality switching and danmaku
  - Local-network / nearby Wi-Fi permissions and cleartext traffic for Android
- Docs: README feature list and CHANGELOG `[Unreleased]`

### newBV — an Android TV client based on BV

- Repo: [zhouwjfi/newBV](https://github.com/zhouwjfi/newBV)
- Upstream (the repo forked directly): [Frost819/newBV](https://github.com/Frost819/newBV) (refactored from [aaa1115910/bv](https://github.com/aaa1115910/bv))
- What I added:
  - **DLNA cast receiver**: built-in SSDP server (private port `47823`) plus AVTransport control, so a phone can cast directly
    - DIDL-Lite carries `aid/bvid/cid/epid/sid` and the live `roomId`; the receiver opens the full player (quality / danmaku / subtitles / speed) based on those IDs
    - `PlaybackGuard` enforces a single player, stopping local playback on the TV as soon as a cast starts
    - Each cast replaces the current player screen (`popUpTo`), `MainActivity` uses `singleTask`, and playback pauses when the app goes to background
  - **Dolby passthrough**: AC3 / E-AC3 (Atmos) bitstreams are no longer decoded locally but sent through HDMI ARC to the soundbar / amplifier
    - Custom `AudioCapabilities` combined with a context-free `DefaultAudioSink.Builder`, avoiding runtime override by system-probed capabilities
    - AAC and other formats are still decoded locally; DTS is intentionally not declared to avoid track creation failures
- Docs: README feature list and new PRD sections 4.13 / 4.14

## My own projects

### ZCode — an Android client for ZCode Remote Control

- Repo: [zhouwjfi/zcode-app](https://github.com/zhouwjfi/zcode-app)
- What it implements:
  - Connect by scanning the QR code in ZCode desktop's Remote Control dialog, with manual URL input as a fallback
  - Immersive fullscreen WebView (no status / navigation bar, no browser UI) with full Cookie / localStorage / JS support; external schemes are handed to the system
  - The connection URL persists, so the app opens straight into the page; long-press the launcher icon → “Change URL”, or use the exit dialog to reconfigure
  - Cleartext HTTP is allowed so intranet addresses work directly

### TV QR Installer — install APKs on Android TV by scanning a QR code

- Repo: [zhouwjfi/tv-qr-installer](https://github.com/zhouwjfi/tv-qr-installer)
- What it implements:
  - A foreground service on the TV serves an HTTP upload page on the LAN and shows its URL as a QR code on screen
  - Scan the QR code, pick an APK on the phone and upload it with a progress bar; the system installer opens automatically
  - Missing the "install unknown apps" permission? It opens the settings page and continues installing when you come back
  - Data stays inside the local network; Leanback launcher entry and landscape always-on screen
