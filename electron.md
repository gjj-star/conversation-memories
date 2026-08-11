# Electron

## 是什么

Electron 是 GitHub 于 2013 年开源的**跨平台桌面应用框架**。核心理念简单粗暴：

> Electron = **Chromium 浏览器 + Node.js 运行时**，用 HTML/CSS/JavaScript 就能构建 Windows/macOS/Linux 桌面应用。

## 架构

```
┌─────────────────────────────────────┐
│         你的 HTML/CSS/JS App         │  ← 你写的代码
├─────────────────────────────────────┤
│         Chromium（渲染进程）          │  ← 显示界面
├─────────────────────────────────────┤
│  Node.js（主进程）                   │  ← 访问文件系统、系统托盘、原生菜单等
│  - 文件 I/O、系统通知、自动更新       │
├─────────────────────────────────────┤
│        操作系统（Win/Mac/Linux）       │
└─────────────────────────────────────┘
```

- **主进程（Main Process）**：跑 Node.js，管理窗口、系统菜单、文件操作
- **渲染进程（Renderer Process）**：跑 Chromium，显示 UI，默认不能直接调 Node.js API（安全隔离）

## 优缺点

### 优点
- **一套代码三平台**：Web 开发者零成本进入桌面应用开发
- **生态巨大**：npm 社区的所有库都能用
- **热更新**：业务逻辑可以不通过应用商店审核直接更新

### 缺点
- **包体积大**：每个 App 都内置一个 Chromium，最小 ~100MB
- **内存占用高**：Chromium 是吃内存大户（你的 App = 一个 Chromium Tab）
- **性能不如原生**：CPU 密集型任务不如 C++/Rust 写的原生应用

## 知名 Electron 应用

| 应用 | 说明 |
|------|------|
| **VS Code** | 微软的代码编辑器（Electron 性能优化的天花板） |
| **Slack** | 企业即时通讯 |
| **Figma** | UI 设计工具 |
| **Discord** | 游戏语音/社区 |
| **Notion** | 笔记和知识管理 |
| **Postman** | API 测试工具 |

## Electron vs 其他桌面方案

| 方案 | 语言 | 性能 | 包体积 | 跨平台 |
|------|------|------|--------|--------|
| **Electron** | JS/HTML/CSS | 中 | 大（100MB+） | ✅ |
| **Tauri** | JS + Rust | 好 | 小（~5MB） | ✅ |
| **Flutter Desktop** | Dart | 好 | 中 | ✅ |
| **WPF / WinUI** | C# | 很好 | 小 | ❌（仅 Windows） |
| **Qt** | C++ | 很好 | 中 | ✅ |

Tauri 是 Electron 的新兴竞争者——用系统自带的 WebView 取代内嵌 Chromium，包体积缩小 20 倍。

## 一句话总结

Electron = 让你用写网页的方式写桌面应用。优点是开发快、跨平台，代价是每个 App 背着 100MB+ 的 Chromium 和相应的内存开销。
