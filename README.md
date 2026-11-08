# 微信视频号下载器（macOS 版）

> 基于 [ltaoo/wx_channels_download](https://github.com/ltaoo/wx_channels_download) v261106 源码编译的 **macOS Apple Silicon (M1/M2/M3/M4, arm64)** 发行版。
>
> 原项目官方仅发布 Windows 包（另有 darwin_arm64 构建包但下载较慢），本仓库提供 **macOS arm64 免编译安装包 + 完整源码**，开箱即用。

微信视频号内容下载工具，**支持下载视频号视频、直播、直播回放**，可选多种清晰度，自动监听。

| 项目 | 状态 |
|---|---|
| 适用系统 | macOS 12+（Apple Silicon / arm64） |
| 安装包大小 | 18 MB |
| 软件本体 | Go 编译，单文件 47 MB，无其他依赖 |
| 上游项目 | [ltaoo/wx_channels_download](https://github.com/ltaoo/wx_channels_download) ⭐ 10k+，活跃维护 |

## 📦 下载安装

**方式一：直接下载安装包（推荐）**

本仓库 `dist/` 目录内提供编译好的安装包：

- [`wx_video_download_v261106_macos_arm64.zip`](dist/wx_video_download_v261106_macos_arm64.zip)（18 MB）

校验和见 [`dist/checksums.txt`](dist/checksums.txt)。

```bash
# 下载后解压并运行
unzip wx_video_download_v261106_macos_arm64.zip
cd wx_video_download_v261106_macos_arm64   # 或解压所在目录
chmod +x wx_video_download
./wx_video_download
```

**方式二：从源码编译（本仓库已含全部源码）**

```bash
git clone https://github.com/yuming77/wxChannelsDownload.git
cd wxChannelsDownload
GOPROXY=https://goproxy.cn,direct bash build/build.sh macos-arm64
./wx_video_download_macos_arm64
```

> 需要 Go 1.20+ 和 Xcode Command Line Tools。Intel Mac 将 `macos-arm64` 换成 `macos-x86_64`。

## 🚀 使用方法

1. **启动服务**

   ```bash
   ./wx_video_download
   ```

   启动成功会看到：

   ```
   API service started successfully, address: http://127.0.0.1:2022
   Proxy service started successfully, address: http://127.0.0.1:2023
   System proxy for network service Ethernet has been set ...
   ```

2. **打开微信 Mac 客户端 → 视频号**，播放想下载的视频

3. 视频下方操作栏会多出 **下载按钮**（如果没有，看页面侧边或底部的悬浮按钮），点击即下载

   - 默认下载当前播放画质（体积最小），下拉菜单可选更高清晰度
   - 下载的文件保存在 `~/Downloads`
   - 管理界面：<http://127.0.0.1:2022>

## ⚠️ 首次运行须知

程序通过 **本地代理 + 自签名证书** 的方式捕获微信视频号流量，首次运行会自动完成两件事：

1. **安装根证书**（SunnyNet CA）到系统钥匙串 —— macOS 会弹出系统授权框，输入开机密码即可
2. **设置系统代理** 指向 `127.0.0.1:2023`

如遇证书安装失败，可用管理员权限运行一次：

```bash
sudo ./wx_video_download
```

## 🛑 停止服务（重要）

按 `Ctrl+C` 正常退出，程序会**自动恢复系统代理设置**。

> ❗ 不要直接 `kill -9`：强杀会遗留代理配置，导致**所有网络流量指向已关闭的本地代理而断网**。
> 万一发生，前往 系统设置 → 网络 → 详细信息 → 代理，关闭 HTTP/HTTPS 代理即可恢复。

**用完建议及时退出**，避免日常流量都经过本地代理。

## 📁 仓库结构

```
├── README.md                    # 本文件
├── LICENSE                      # 原项目许可证（MIT + Commons Clause）
├── dist/                        # macOS arm64 编译好的安装包
├── main.go / cmd/ / internal/ / pkg/   # Go 源码（与上游 v261106 一致）
├── frontend/                    # 注入微信的前端脚本源码
├── build/                       # 各平台构建脚本
└── docs/                        # 上游文档站源文件
```

## ❓ 常见问题

| 问题 | 处理 |
|---|---|
| 视频下没有出现下载按钮 | 确认服务在运行（终端没退出）、微信已登录、刷新视频号页面；或看页面侧边悬浮按钮 |
| 提示证书安装失败 | `sudo ./wx_video_download` 以管理员运行一次 |
| 微信更新后失效 | 等上游项目更新后重新编译，或关注 [上游 Releases](https://github.com/ltaoo/wx_channels_download/releases) |
| 下载速度慢 / 失败 | 检查是否开启了其他代理软件与本工具的 2023 端口冲突 |
| Windows / Linux 用户 | 请直接使用[上游官方构建包](https://github.com/ltaoo/wx_channels_download/releases) |

## 🙏 致谢与声明

- 本仓库内容来自开源项目 **[ltaoo/wx_channels_download](https://github.com/ltaoo/wx_channels_download)**，感谢原作者 [ltaoo](https://github.com/ltaoo)
- 前端解密部分参考 [kanadeblisst00/WechatVideoSniffer2.0](https://github.com/kanadeblisst00/WechatVideoSniffer2.0)，后端解密来自 [Hanson/WechatSphDecrypt](https://github.com/Hanson/WechatSphDecrypt)
- 本仓库按原项目许可证（MIT + Commons Clause）**免费分发**，禁止售卖

> ⚠️ **免责声明**：本项目仅供技术交流学习和研究目的，请遵守法律法规，请勿用作任何非法用途，否则造成一切后果自负。下载并使用即视为知晓并同意上述声明。请尊重内容创作者的版权。
