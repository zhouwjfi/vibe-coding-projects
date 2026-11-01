# Vibe Coding 项目记录

[English](README.md) | [简体中文](README.zh-CN.md)

记录我用 AI 辅助（vibe coding）做的项目。

- **Fork 的项目**：只记录我在直接 fork 的仓库之上新增或修改了什么
- **自己的项目**：记录实现了哪些功能

## Fork 项目

### PiliNara — 基于 PiliPlus 的 B 站客户端

- 仓库：[zhouwjfi/PiliNara](https://github.com/zhouwjfi/PiliNara)
- 上游（直接 fork 的仓库）：[Starfallan/PiliNara](https://github.com/Starfallan/PiliNara)
- 我在其之上新增：
  - **投屏到 newBV**：按需扫描局域网 DLNA 设备，只在点击 TV 按钮时扫描并缓存；连接失败自动丢弃缓存重扫
  - 视频详情页、播放页、封面长按卡片新增 TV 投屏入口，扫不到设备时隐藏按钮
  - **直播投屏**：接收端按 roomId 复用自带直播播放器，可继续切画质、看弹幕
  - Android 端补充局域网 / 近场 Wi-Fi 权限与明文流量声明
- 文档改动：README 功能清单、CHANGELOG `[Unreleased]`

### newBV — 基于 BV 的 Android TV 客户端

- 仓库：[zhouwjfi/newBV](https://github.com/zhouwjfi/newBV)
- 上游（直接 fork 的仓库）：[Frost819/newBV](https://github.com/Frost819/newBV)（重构自 [aaa1115910/bv](https://github.com/aaa1115910/bv)）
- 我在其之上新增：
  - **DLNA 投屏接收**：内置 SSDP（私有端口 `47823`）与 AVTransport 控制服务，手机可直接投屏
    - DIDL-Lite 携带 `aid/bvid/cid/epid/sid` 与直播 `roomId`，接收端按标识打开完整播放器（画质 / 弹幕 / 字幕 / 倍速）
    - `PlaybackGuard` 单播放器约束，投屏时电视上的本地播放立即停止
    - 每次投屏替换当前播放页（`popUpTo`），`MainActivity` 改 `singleTask`，切后台自动暂停
  - **杜比源码直通**：AC3 / E-AC3（Atmos）压缩码流不再本地解码，经 HDMI ARC 直出给回音壁 / 功放
    - 自定义 `AudioCapabilities`，并用无参 `DefaultAudioSink.Builder` 避免运行期能力被系统探测结果覆盖
    - AAC 等格式仍本地解码，DTS 系未声明以免建轨失败
- 文档改动：README 功能升级、PRD 新增 4.13 / 4.14

## 自己的项目

### ZCode — ZCode Remote Control 的 Android 客户端

- 仓库：[zhouwjfi/zcode-app](https://github.com/zhouwjfi/zcode-app)
- 实现内容：
  - 扫描 ZCode 桌面端 Remote Control 弹窗的二维码即可连接，也支持手动输入地址
  - 沉浸式全屏 WebView（隐藏状态栏 / 导航栏，无浏览器 UI），支持 Cookie / localStorage / JS，外链交给系统处理
  - 连接地址持久化，配置后打开 App 直接进入页面；长按桌面图标「Change URL」或返回键退出弹窗可重新配置
  - 允许明文 HTTP，方便直接访问局域网地址
