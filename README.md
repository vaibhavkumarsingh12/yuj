<div align="center">

<!-- 
  ✅ ANIMATION WORKS because banner.svg is a separate file in the repo.
  GitHub STRIPS inline <style>/@keyframes from markdown — but renders
  them fully when SVG is served as an <img src="..."> file.
-->
<img src="./banner.svg" alt="@lottiefiles/dotlottie-web banner" width="100%"/>

# 🎬 @lottiefiles/dotlottie-web

**The official, high-performance Lottie & dotLottie animation player for the web.**  
Powered by WebAssembly. Framework-agnostic. Tiny footprint.

[![npm version](https://img.shields.io/npm/v/@lottiefiles/dotlottie-web?label=%40lottiefiles%2Fdotlottie-web&color=0ea5e9)](https://www.npmjs.com/package/@lottiefiles/dotlottie-web)
[![Bundle Size](https://img.shields.io/bundlephobia/minzip/@lottiefiles/dotlottie-web?color=8b5cf6)](https://bundlephobia.com/package/@lottiefiles/dotlottie-web)
[![npm downloads](https://img.shields.io/npm/dw/@lottiefiles/dotlottie-web?color=38bdf8)](https://www.npmjs.com/package/@lottiefiles/dotlottie-web)
[![License](https://img.shields.io/github/license/LottieFiles/dotlottie-web?color=10b981)](https://github.com/LottieFiles/dotlottie-web/blob/main/LICENSE)

</div>

---

## ✨ Features

- 🚀 **WebAssembly-powered** rendering engine for buttery-smooth animations
- 📦 **Tiny bundle** — minzipped, tree-shakeable, zero dependencies
- 🖼️ **Dual format support** — `.lottie` (dotLottie) and `.json` (Lottie)
- 🎛️ **Full playback control** — play, pause, stop, speed, loop, direction
- 🔔 **Rich event system** — onLoad, onComplete, onFrame, onError and more
- 🌐 **Framework wrappers** — React, Vue, Svelte, SolidJS, Web Components

---

## 🏗️ Architecture

```mermaid
graph TD
    A["👨‍💻 Your Application Code"] --> B

    subgraph pkg ["📦 @lottiefiles/dotlottie-web"]
        B["DotLottie Class\n(Public API)"]
        B --> C["Event Emitter"]
        B --> D["File Loader\n(.lottie / .json)"]
        D --> E["WASM Core Engine\n(dotlottie-rs)"]
        E --> F["Animation State Machine"]
        E --> G["Frame Renderer"]
        G --> H["🖼️ Canvas Element (DOM)"]
    end

    B --> I["Framework Wrappers"]
    I --> J["⚛️ React"]
    I --> K["💚 Vue"]
    I --> L["🔥 Svelte"]
    I --> M["🔷 SolidJS"]
    I --> N["🌐 Web Component"]

    style pkg fill:#0f172a,stroke:#0ea5e9,stroke-width:2px,color:#f1f5f9
    style H fill:#14532d,stroke:#10b981,color:#dcfce7
```

---

## 🔄 Animation Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Idle : new DotLottie()
    Idle --> Loading : load(src) called
    Loading --> Ready : ✅ onLoad fired
    Loading --> Error : ❌ onLoadError fired
    Ready --> Playing : play()
    Playing --> Paused : pause()
    Paused --> Playing : play()
    Playing --> Stopped : stop()
    Playing --> Complete : last frame
    Complete --> Playing : loop=true
    Complete --> Stopped : loop=false
    Stopped --> [*]
```

---

## 🔀 API Data Flow

```mermaid
sequenceDiagram
    autonumber
    participant Dev as 👨‍💻 Developer
    participant API as DotLottie API
    participant WASM as ⚙️ WASM Engine
    participant Canvas as 🖼️ Canvas

    Dev->>API: new DotLottie({ canvas, src, autoplay, loop })
    API->>WASM: load animation data
    WASM-->>API: ✅ onLoad fired
    API-->>Dev: onLoad callback

    Dev->>API: play()
    loop 🔁 Animation Loop
        WASM->>Canvas: render frame N
        WASM-->>Dev: onFrame(frameNumber)
    end
    WASM-->>Dev: onComplete
```

---

## 🚀 Installation

```bash
npm install @lottiefiles/dotlottie-web
```

---

## 📖 Usage

```js
import { DotLottie } from '@lottiefiles/dotlottie-web';

const dotLottie = new DotLottie({
  canvas: document.getElementById('my-canvas'),
  src: 'https://lottie.host/your-animation-id/animation.lottie',
  autoplay: true,
  loop: true,
});

dotLottie.addEventListener('complete', () => console.log('Done!'));
```

---

## ⚙️ API Reference

| Option / Method | Type | Description |
|---|---|---|
| `canvas` | `HTMLCanvasElement` | Canvas to render into (**required**) |
| `src` | `string` | URL of `.lottie` or `.json` (**required**) |
| `autoplay` | `boolean` | Auto-start on load |
| `loop` | `boolean` | Loop animation |
| `speed` | `number` | Playback speed (default `1`) |
| `.play()` | Method | Start / resume |
| `.pause()` | Method | Pause |
| `.stop()` | Method | Stop + reset |
| `.setSpeed(n)` | Method | Set speed multiplier |
| `.destroy()` | Method | Clean up memory |

---

## 🌐 Framework Support

```mermaid
graph LR
    core["⚙️ dotlottie-web\n(Core)"]
    core --> react["⚛️ dotlottie-react"]
    core --> vue["💚 dotlottie-vue"]
    core --> svelte["🔥 dotlottie-svelte"]
    core --> solid["🔷 dotlottie-solid"]
    core --> wc["🌐 dotlottie-wc"]

    style core fill:#0f172a,stroke:#0ea5e9,color:#f1f5f9
```

---

## 📄 License

MIT © [LottieFiles](https://lottiefiles.com)
