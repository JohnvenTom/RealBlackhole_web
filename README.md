<div align="center">

# BlackholeWeb

**WebGL Black Hole Real-time Renderer**

[![WebGL](https://img.shields.io/badge/WebGL-2.0-990000?style=flat&logo=webgl&logoColor=white)](https://www.khronos.org/webgl/)
[![WebAssembly](https://img.shields.io/badge/WebAssembly-654FF0?style=flat&logo=webassembly&logoColor=white)](https://webassembly.org/)
[![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat&logo=vercel&logoColor=white)](https://vercel.com/)
[![Live Demo](https://img.shields.io/badge/Live_Demo-🚀_在线体验-00C4B4?style=flat)](https://real-blackhole-web.vercel.app/)

*基于 WebAssembly + WebGL 的实时黑洞引力透镜效应可视化*

**👉 [在线体验 / Live Demo](https://real-blackhole-web.vercel.app/)**

</div>

---

## 概述

BlackholeWeb 是 ArikaShow 黑洞渲染模块的独立 Web 构建产物，通过 Emscripten 将 C/C++ 渲染引擎编译为 WebAssembly，在浏览器中实现实时 3D 黑洞可视化。项目包含完整的 GPU 检测、集成显卡告警及性能降级策略。

---

## 构建产物

| 文件 | 类型 | 说明 |
|:-----|:-----|:-----|
| `BlackholeWeb.html` | HTML | 入口页面，含 Canvas 容器与 GPU 检测逻辑 |
| `BlackholeWeb.js` | JavaScript | Emscripten 生成的胶水代码，负责 WASM 加载与 WebGL 桥接 |
| `BlackholeWeb.wasm` | WebAssembly | 核心渲染引擎编译产物 |
| `BlackholeWeb.data` | Binary | 资源数据包（3D 模型、纹理等） |
| `vercel.json` | Config | Vercel 部署配置，含 MIME 类型与路由规则 |

---

## 技术架构

```
┌─────────────────────────────────────────────┐
│              BlackholeWeb.html              │
│  ┌─────────────┐  ┌──────────────────────┐  │
│  │  WebGL Canvas │  │  GPU Warning System │  │
│  │  (fullscreen) │  │  (集成显卡检测/告警) │  │
│  └──────┬───────┘  └──────────────────────┘  │
└─────────┼───────────────────────────────────┘
          │
┌─────────┼───────────────────────────────────┐
│  BlackholeWeb.js (Emscripten Glue Code)     │
│  ├── WASM Module Loader                     │
│  ├── Data Package Downloader (.data)        │
│  ├── WebGL Context Bridge                   │
│  └── Memory Management (Heap)               │
└─────────┼───────────────────────────────────┘
          │
┌─────────┼───────────────────────────────────┐
│  BlackholeWeb.wasm (Core Engine)            │
│  ├── 3D Scene Graph                         │
│  ├── Gravitational Lensing Shader           │
│  ├── Accretion Disk Particle System         │
│  └── Camera & Input Handling                │
└─────────────────────────────────────────────┘
```

---

## 功能特性

### 实时渲染

- 黑洞引力透镜效应模拟
- 吸积盘粒子系统与色温渐变
- Bloom 后期处理泛光效果
- 深空背景星场渲染

### GPU 智能检测

- 自动识别集成显卡 / 独立显卡
- 集成显卡环境下弹出性能告警面板
- 提供显卡切换操作指引（Windows / macOS / NVIDIA / AMD）
- 支持一键关闭告警继续运行

### 交互控制

- 鼠标左键拖拽：旋转视角
- 鼠标右键拖拽：平移画面
- 滚轮：缩放距离
- 右键菜单已禁用（防止上下文菜单干扰交互）

---

## 部署配置

`vercel.json` 配置说明：

```json
{
  "headers": [
    {
      "source": "/(.*).wasm",
      "headers": [{ "key": "Content-Type", "value": "application/wasm" }]
    },
    {
      "source": "/(.*).data",
      "headers": [{ "key": "Content-Type", "value": "application/octet-stream" }]
    }
  ],
  "rewrites": [
    { "source": "/", "destination": "/BlackholeWeb.html" }
  ]
}
```

**关键配置项：**

| 配置 | 说明 |
|:-----|:-----|
| `Content-Type: application/wasm` | 确保 WASM 文件被浏览器正确识别，避免流式编译失败 |
| `Content-Type: application/octet-stream` | 确保 `.data` 资源包以二进制流传输 |
| `rewrites / → BlackholeWeb.html` | 根路径自动路由到入口页面 |

---

## 系统要求

| 项目 | 最低要求 | 推荐配置 |
|:-----|:---------|:---------|
| 浏览器 | Chrome 90+ / Firefox 88+ / Edge 90+ | Chrome 最新版 |
| WebGL | WebGL 2.0 | WebGL 2.0 + 独立显卡 |
| GPU | 支持 WebGL 2.0 | NVIDIA / AMD 独立显卡 |
| 内存 | 4 GB RAM | 8 GB+ RAM |
| 网络 | 需下载 ~数 MB 资源包 | 稳定宽带连接 |

---

## 本地运行

由于项目依赖 WASM 与资源包的 HTTP 流式加载，**必须通过 HTTP 服务器访问**：

```bash
# Python
python -m http.server 8080

# Node.js
npx http-server -p 8080 -c-1

# 访问 http://localhost:8080/BlackholeWeb.html
```

> ⚠️ `file://` 协议下 WASM 流式编译与 `.data` 资源包加载将失败。

---

## Vercel 部署

**在线地址：** [https://real-blackhole-web.vercel.app/](https://real-blackhole-web.vercel.app/)

```bash
cd build_web
npx vercel --prod
```

Vercel 将自动读取 `vercel.json` 配置，根路径 `/` 自动映射到 `BlackholeWeb.html`。

---

## 与主项目的关系

本目录是 [ArikaShow](../) 项目的黑洞渲染模块的独立 Web 构建版本：

| 版本 | 位置 | 渲染技术 | 说明 |
|:-----|:-----|:---------|:-----|
| Three.js 版 | `blackhole.html` + `js/blackhole.js` | Three.js r128 + GLTFLoader | 轻量级，CDN 依赖，集成在主站 |
| **WebAssembly 版** | `build_web/` | Emscripten + WebGL 2.0 | 高性能，独立部署，完整渲染引擎 |

---

## 许可证

[MIT License](../LICENSE) © 2024-2026 ArikaShow Contributors
