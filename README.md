# Vibe Coding Projects

记录我用 AI 辅助（vibe coding）做的项目。
A log of the projects I build with AI-assisted (vibe coding) workflows.

- **Fork 的项目**：只记录我在原项目之上新增/修改了什么
- **自己的项目**：记录实现了哪些功能

- **Forked projects**: only what I added or changed on top of the upstream project
- **My own projects**: what the project actually implements

[中文](#中文) | [English](#english)

---

## 中文

### Fork 项目

#### PiliNara — 基于 PiliPlus 的 B 站客户端

- 仓库：[zhouwjfi/PiliNara](https://github.com/zhouwjfi/PiliNara)
- 上游（直接 fork 的仓库）：[Starfallan/PiliNara](https://github.com/Starfallan/PiliNara)
- 我在其之上新增：
  - **投屏到 newBV**：按需扫描局域网 DLNA 设备，只在点击 TV 按钮时扫描并缓存；连接失败自动丢弃缓存重扫
  - 视频详情页、播放页、封面长按卡片新增 TV 投屏入口，扫不到设备时隐藏按钮
  - **直播投屏**：接收端按 roomId 复用自带直播播放器，可继续切画质、看弹幕
  - Android 端补充局域网 / 近场 Wi-Fi 权限与明文流量声明
- 文档改动：README 功能清单、CHANGELOG `[Unreleased]`

#### newBV — 基于 BV 的 Android TV 客户端

- 仓库：[zhouwjfi/newBV](https://github.com/zhouwjfi/newBV)
- 上游：[Frost819/newBV](https://github.com/Frost819/newBV)（重构自 [aaa1115910/bv](https://github.com/aaa1115910/bv)）
- 我在其之上新增：
  - **DLNA 投屏接收**：内置 SSDP（私有端口 `47823`）与 AVTransport 控制服务，手机可直接投屏
    - DIDL-Lite 携带 `aid/bvid/cid/epid/sid` 与直播 `roomId`，接收端按标识打开完整播放器（画质 / 弹幕 / 字幕 / 倍速）
    - `PlaybackGuard` 单播放器约束，投屏时电视上的本地播放立即停止
    - 每次投屏替换当前播放页（`popUpTo`），`MainActivity` 改 `singleTask`，切后台自动暂停
  - **杜比源码直通**：AC3 / E-AC3（Atmos）压缩码流不再本地解码，经 HDMI ARC 直出给回音壁 / 功放
    - 自定义 `AudioCapabilities`，并用无参 `DefaultAudioSink.Builder` 避免运行期能力被系统探测结果覆盖
    - AAC 等格式仍本地解码，DTS 系未声明以免建轨失败
- 文档改动：README 功能升级、PRD 新增 4.13 / 4.14

### 自己的项目

暂无。

---

## English

### Forked projects

#### PiliNara — a Bilibili client based on PiliPlus

- Repo: [zhouwjfi/PiliNara](https://github.com/zhouwjfi/PiliNara)
- Upstream (the repo forked directly): [Starfallan/PiliNara](https://github.com/Starfallan/PiliNara)
- What I added:
  - **Cast to newBV**: on-demand discovery of DLNA devices on the LAN — scanning only when the TV button is tapped, with cached results and automatic re-scan on failure
  - TV cast entries on the video detail page, player page and the long-press cover sheet; the entry hides itself when no device is found
  - **Live casting**: the receiver reuses its own live player by room ID, keeping quality switching and danmaku
  - Local-network / nearby Wi-Fi permissions and cleartext traffic for Android
- Docs: README feature list and CHANGELOG `[Unreleased]`

#### newBV — an Android TV client based on BV

- Repo: [zhouwjfi/newBV](https://github.com/zhouwjfi/newBV)
- Upstream: [Frost819/newBV](https://github.com/Frost819/newBV) (refactored from [aaa1115910/bv](https://github.com/aaa1115910/bv))
- What I added:
  - **DLNA cast receiver**: built-in SSDP server (private port `47823`) plus AVTransport control, so a phone can cast directly
    - DIDL-Lite carries `aid/bvid/cid/epid/sid` and the live `roomId`; the receiver opens the full player (quality / danmaku / subtitles / speed) based on those IDs
    - `PlaybackGuard` enforces a single player, stopping local playback on the TV as soon as a cast starts
    - Each cast replaces the current player screen (`popUpTo`), `MainActivity` uses `singleTask`, and playback pauses when the app goes to background
  - **Dolby passthrough**: AC3 / E-AC3 (Atmos) bitstreams are no longer decoded locally but sent through HDMI ARC to the soundbar / amplifier
    - Custom `AudioCapabilities` combined with a context-free `DefaultAudioSink.Builder`, avoiding runtime override by system-probed capabilities
    - AAC and other formats are still decoded locally; DTS is intentionally not declared to avoid track creation failures
- Docs: README feature list and new PRD sections 4.13 / 4.14

### My own projects

None yet.
