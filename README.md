<div align="center">

# 文件传输，不必如此复杂

</div>


![AltSendme Header](assets/header.png)

<div align="center">

![AltSendme working demo](assets/animation.gif)

</div>

<div align="center">


[![Discord][badge-discord]](https://discord.gg/xwb7z22Eve)
![Version][badge-version]
![Website][badge-website]
![Platforms][badge-platforms]
[![Sponsor][badge-sponsor]](https://github.com/sponsors/tonyantony300)


</div>



一款免费且开源的文件传输工具，借助 [前沿的点对点网络技术](https://www.iroh.computer)，让你无需将文件存储在云端服务器上，即可直接进行传输。

既然可以可靠、便捷地直接传输文件，且全程端到端加密、无需暴露任何个人信息，何必还要依赖 WeTransfer、Dropbox 或 Google Drive 呢？

欢迎加入我们的 [Discord](https://discord.gg/xwb7z22Eve) 一起贡献

## 功能特性

- **随时随地发送** – 在局域网内或跨大洲都能流畅工作。
- [**传输一切内容**](https://www.iroh.computer/proto/iroh-blobs) – 发送任意大小、任意格式的文件或目录，并通过基于 BLAKE3 的完整性校验进行验证。
- **无需账号或个人信息** – 无需注册，也不会泄露个人隐私。
- **点对点直接传输** – 设备之间直接传送文件，中间不经任何云存储。
- **身份验证** – 票据（Ticket）包含用于身份验证的加密身份信息。
- **端到端加密** – 始终开启的保护，基于 QUIC + TLS 1.3，提供前向和后向保密。
- **断点续传** – 下载中断后自动从断点处继续。
- **广播传输** – 将同一个文件/文件夹同时分享给任意数量的对端。
- **预览** – 下载前即可查看并验证内容
- **快速且可靠** – 能够跑满多千兆网络连接，实现闪电般的传输速度。
- [**通过 QUIC 实现 NAT 穿透**](https://www.iroh.computer/docs/faq#does-iroh-use-relay-servers) – 使用 QUIC 打洞技术建立安全、低延迟的连接，并以加密中继作为回退方案。
- **命令行集成** – 可与 [Sendme CLI](https://www.iroh.computer/sendme) 互操作。
- **免费且开源** – 无上传费用，无大小限制，完全由社区驱动。
- **即将推出** – 移动端和 Web 端版本




## 安装方式

最简单的开始方式是为你的操作系统下载以下对应版本之一：

<table>
  <tr>
    <td><b>平台</b></td>
    <td><b>下载</b></td>
  </tr>
  <tr>
    <td><b>Windows</b></td>
    <td><a href='https://github.com/tonyantony300/alt-sendme/releases/download/v0.3.5/AltSendme_0.3.5_x64-setup.exe'>AltSendme.exe</a></td>
  </tr>
  <tr>
    <td><b>macOS</b></td>
    <td><a href='https://github.com/tonyantony300/alt-sendme/releases/download/v0.3.5/AltSendme_0.3.5_universal.dmg'>AltSendme.dmg</a></td>
  <tr>
    <td><b>Linux </b></td>
    <td><a href='https://github.com/tonyantony300/alt-sendme/releases/download/v0.3.5/AltSendme_0.3.5_amd64.deb'>AltSendme.deb</a></td>
  </tr>
  <tr>
    <td><b>Android</b></td>
    <td><a href='https://github.com/tonyantony300/alt-sendme/releases/download/v0.3.6-beta.1/AltSendme-v0.3.6-beta-universal.apk'>AltSendme-beta.apk</a></td>
  </tr>

</table>

**Windows (Scoop)**  


```bash
scoop bucket add extras
scoop install extras/altsendme
```

更多下载选项请见 [GitHub Releases](https://github.com/tonyantony300/alt-sendme/releases)。


## 支持的语言
 🇺🇸 🇷🇺 🇫🇷 🇨🇳 🇩🇪 🇯🇵 🇮🇳 🇹🇭 🇮🇹 🇨🇿 🇪🇸 🇧🇷 🇸🇦 🇮🇷 🇰🇷  🇵🇱 🇺🇦 🇹🇷 🇳🇴 🇧🇩 🇭🇺 🇷🇸 🇹🇼 🇰🇭
 
## 工作原理 

1. 拖入你的文件或文件夹 - AltSendme 会生成一次性分享码（称为 "ticket"）。
2.  通过聊天、邮件或短信分享该票据。
3. 你的朋友将票据粘贴到他们的应用中，传输便开始。


## 底层原理 ⚙️🛠️

AltSendme 在底层使用 [Iroh](https://www.iroh.computer) 来实现点对点文件传输。它是 WebRTC 和 libp2p 等技术的现代模块化替代方案。

### 核心概念 

- *Blobs（数据块）*
- *Tickets（票据）*
- *Peer Discovery（对端发现）*、*Hole-punching（打洞）* 与 *NAT traversal（NAT 穿透）*
- *QUIC* 与 *端到端加密*
- *Relays（中继）*


### 1. Blobs（数据块）

基于内容寻址的数据块存储与传输。`iroh-blobs` 实现了任意大小字节数据块的请求/响应与流式传输，使用 BLAKE3 校验流和基于内容寻址的链接。

- Blob：一段不透明的字节序列（不包含嵌入式元数据）。
- Link：一个 32 字节的 BLAKE3 哈希，用于标识一个 blob。
- HashSeq：一个包含一系列链接的 blob（用于分块/树形结构）。
- Provider / Requester：提供方负责提供数据，请求方负责拉取数据。一个端点可以同时扮演两种角色。

### 2. Tickets（票据）

票据是在 iroh 端点之间分享拨号信息的一种方式。它是一个单一令牌，包含了连接到另一个端点所需的一切信息，或者在这种情况下，是拉取一个 blob 所需的信息。其中包含 Ed25519 NodeId：你的设备用于身份验证的加密身份标识。它们同时也非常强大。值得一提的是，这种设计显著优于完全点对点系统——后者会将你的 IP 广播给对端。而在 iroh 中，票据用于在你明确希望连接的对端之间形成一个"舒适网络"（cozy network）。虽然也可以"完全 p2p"，将应用配置为广播拨号信息，但票据是一种更好的折中默认方案。

### 3. 对端发现、NAT 穿透与打洞

对端在启动时向开源的公共中继服务器注册，以协助穿越防火墙和 NAT，从而建立连接。一旦连接成功，Iroh 会使用 QUIC 打洞技术尝试建立直接的点对点连接，绕过中继。如果直连可行，则通信将以端到端加密在对端之间直接进行；否则，中继仅作为临时回退。这一机制保证了局域网内和互联网上的对端之间都能建立顺畅可靠的连接。

###  4. QUIC 与加密

QUIC 是一种基于 UDP 构建的现代传输协议，旨在降低延迟并提升相比 TCP 的 Web 性能。它最初由 Google 开发，现已由 IETF 标准化为 HTTP/3 的基础，将 TLS 1.3 加密直接集成进协议中。

QUIC 带来了以下强大的能力：
* 加密与身份验证
* 多路复用流
    * 不会发生队头阻塞问题
    * 流优先级
    * 共享同一个拥塞控制器
* 加密的不可靠数据报传输
* 如果你之前连接过同一端点，则可以零往返时间建立连接


### 5. 中继（Relays）

AltSendme 使用开源的公共中继服务器来协助建立直连、加快初始连接速度，并在两端点之间的直连失败或不可行时提供回退。所有连接都是端到端加密的。中继不过是一个"普通的 UDP 套接字"，用来转发加密数据包。[了解更多。](https://docs.iroh.computer/about/faq)


## 路线图 🚧

- 跨平台移动端版本
- 基于短语的寻址（通过 Iroh-gossip 与 PAKE）
- 使用完全自托管中继的、无限速且可靠的传输
- Web 版本（在浏览器中发送与接收）
- 更完善的系统/网络层面的传输过程洞察


[📫 留下你的邮箱以接收更新](https://tally.so/r/ob2Vkx)




## 故障排除

### 1. AltSendme 在 Windows 上无法启动（缺少 Edge WebView2 Runtime）

#### 症状

- 双击 `AltSendme.exe` 后没有任何反应。窗口没有出现，任务管理器中也看不到该进程。
- 这种情况可能发生在标准安装版和便携版中。

#### 原因

- 系统中缺失、过时或未正确安装 Microsoft Edge WebView2 Runtime。  
  AltSendme 在 Windows 上依赖 WebView2 来渲染界面。

#### 解决方法

1. **检查 WebView2 是否已安装**
   - 打开 Windows 的 **添加或删除程序**（也叫 *应用和功能*）。
   - 查看是否有 **Microsoft Edge WebView2 Runtime**。

2. **安装或更新 WebView2**
   - 直接从 Microsoft 下载 WebView2 Runtime：[链接](https://developer.microsoft.com/en-us/microsoft-edge/webview2?form=MA13LH)。
   - 如果你更喜欢离线安装包，可下载离线安装包并以管理员身份运行。

3. **重新运行 AltSendme**
   - 安装/更新 WebView2 后，再次启动 `AltSendme.exe`。
   - 如果仍有问题，请重启电脑后重试。

#### 补充建议

- 如果重装一次仍无效，请完全卸载 Edge WebView2，然后以管理员权限重新安装。
- 确认你的 Windows 系统已安装来自微软的最新更新。

#### 仍然无法解决？

- 请前往我们的 [Discord](https://discord.gg/xwb7z22Eve) 服务器发起支持讨论，并提供你环境的相关日志以及你已经尝试过的步骤。


## 开发环境搭建

### 前置条件

- Rust 1.89+
- Node.js 18+
- npm 或 yarn

### 快速开始

1. **Fork 并克隆仓库**：
   ```bash
   git clone https://github.com/your-username/alt-sendme.git
   cd alt-sendme
   ```

2. **安装前端依赖**：
   ```bash
   npm install
   ```

3. **安装 Tauri**：
   ```bash
   cargo install tauri-cli
   ```

4. **以开发模式运行**：
   ```bash
   cargo tauri dev
   ```

5. **（可选）配置 Android 项目**：
   ```bash
   rm src-tauri/gen/android
   cargo tauri android init
   git checkout src-tauri/gen/android
   cargo tauri android dev
   ```
   

6. **本地构建** ：
   ```bash
    cargo tauri build --no-bundle
   ```

7. **在 Android 上安装** ：
   ```
   npm run android:build -- --debug --apk
      
   adb install -r src-tauri/gen/android/app/build/outputs/apk/universal/debug/app-universal-debug.apk
   ```

## 本地测试

安装 [Sendme CLI](https://www.iroh.computer/sendme) 工具，你就可以在同一台设备上分享文件以测试整个传输流程。文件不会离开你的设备，其效果等同于一次复制操作。

## 加入我们的 [Discord](https://discord.gg/xwb7z22Eve) 一起贡献

参与贡献的最佳方式是加入我们的 Discord 打个招呼。介绍一下自己，并分享你所拥有的技能或兴趣——无论是编码、测试、设计还是其他方向。你也可以提交 issue、提出修复建议或抛出想法。维护者会在每一步为你提供指引。

这是了解项目背景、对齐方向以及与 [社区](https://discord.gg/xwb7z22Eve) 协作的最佳场所。

## 许可证

AGPL-3.0

## 隐私政策

关于 AltSendme 如何处理你的数据和隐私，请参阅 [PRIVACY.md](PRIVACY.md)。

[![Sponsor](https://img.shields.io/badge/sponsor-30363D?style=for-the-badge&logo=GitHub-Sponsors&logoColor=#EA4AAA)](https://github.com/sponsors/tonyantony300) [![Buy Me Coffee](https://img.shields.io/badge/Buy%20Me%20Coffee-FF5A5F?style=for-the-badge&logo=coffee&logoColor=FFFFFF)](https://buymeacoffee.com/tny_antny)


## 贡献者

<a href="https://github.com/tonyantony300/alt-sendme/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=tonyantony300/alt-sendme" />
</a>


## 致谢


- [Iroh](https://www.iroh.computer)
- [Tauri](https://v2.tauri.app)


## 联系方式

如需提出建议、反馈或进行媒体相关的沟通，请通过 [此处](https://www.altsendme.com/en/contact) 与我联系。


感谢你关注本项目！如果你觉得它有用，不妨给它点个 star，并将它推荐给更多人。




<!-- <div align="center" style="color: gray;"></div> -->

[badge-website]: https://img.shields.io/badge/website-altsendme.com-orange
[badge-version]: https://img.shields.io/badge/version-0.3.5-blue
[badge-discord]: https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white
[badge-platforms]: https://img.shields.io/badge/platforms-macOS%2C%20Windows%2C%20Linux%2C%20Android%2C%20-green
[badge-sponsor]: https://img.shields.io/badge/sponsor-ff69b4


