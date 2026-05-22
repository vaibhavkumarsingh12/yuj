<div align="center">

<!-- ANIMATED SVG HERO BANNER -->
<svg viewBox="0 0 700 140" xmlns="http://www.w3.org/2000/svg" width="700" height="140">
  <defs>
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:#0ea5e9;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#8b5cf6;stop-opacity:1" />
    </linearGradient>
    <style>
      .dot { animation: bounce 1.4s ease-in-out infinite; }
      .dot1 { animation-delay: 0s; }
      .dot2 { animation-delay: 0.2s; }
      .dot3 { animation-delay: 0.4s; }
      .dot4 { animation-delay: 0.6s; }
      .dot5 { animation-delay: 0.8s; }
      .line { animation: flow 2.5s linear infinite; }
      @keyframes bounce {
        0%, 100% { transform: translateY(0px); opacity: 0.5; }
        50% { transform: translateY(-12px); opacity: 1; }
      }
      @keyframes flow {
        0% { stroke-dashoffset: 200; }
        100% { stroke-dashoffset: 0; }
      }
      .title { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; }
      .fade { animation: fadeIn 1s ease-in forwards; }
      @keyframes fadeIn { from { opacity:0; } to { opacity:1; } }
    </style>
  </defs>

  <!-- Background pill -->
  <rect rx="16" ry="16" width="700" height="140" fill="url(#bg)" opacity="0.08"/>

  <!-- Animated flowing line -->
  <path d="M 80 70 Q 200 20 350 70 Q 500 120 620 70"
        stroke="url(#bg)" stroke-width="2" fill="none"
        stroke-dasharray="200" class="line"/>

  <!-- Animated dots on the line -->
  <circle cx="140" cy="48" r="8" fill="#0ea5e9" class="dot dot1"/>
  <circle cx="250" cy="38" r="8" fill="#38bdf8" class="dot dot2"/>
  <circle cx="350" cy="70" r="10" fill="#8b5cf6" class="dot dot3"/>
  <circle cx="450" cy="100" r="8" fill="#a78bfa" class="dot dot4"/>
  <circle cx="560" cy="58" r="8" fill="#0ea5e9" class="dot dot5"/>

  <!-- Title text -->
  <text x="350" y="118" text-anchor="middle" font-size="13"
        fill="#64748b" class="title fade">
    ✦ Render beautiful Lottie &amp; dotLottie animations on the web ✦
  </text>
</svg>

# 🎬 @lottiefiles/dotlottie-web

**The official, high-performance Lottie &amp; dotLottie animation player for the web.**
Powered by WebAssembly. Framework-agnostic. Tiny footprint.

[![npm version](https://img.shields.io/npm/v/@lottiefiles/dotlottie-web?label=%40lottiefiles%2Fdotlottie-web&color=0ea5e9)](https://www.npmjs.com/package/@lottiefiles/dotlottie-web)
[![Bundle Size](https://img.shields.io/bundlephobia/minzip/@lottiefiles/dotlottie-web?color=8b5cf6)](https://bundlephobia.com/package/@lottiefiles/dotlottie-web)
[![npm downloads](https://img.shields.io/npm/dw/@lottiefiles/dotlottie-web?color=38bdf8)](https://www.npmjs.com/package/@lottiefiles/dotlottie-web)
[![License](https://img.shields.io/github/license/LottieFiles/dotlottie-web?color=10b981)](https://github.com/LottieFiles/dotlottie-web/blob/main/LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/LottieFiles/dotlottie-web?color=f59e0b)](https://github.com/LottieFiles/dotlottie-web)

</div>

---

## ✨ Features

- 🚀 **WebAssembly-powered** rendering engine for buttery-smooth animations
- 📦 **Tiny bundle** — minzipped, tree-shakeable, no dependencies
- 🖼️ **Dual format support** — `.lottie` (dotLottie) and `.json` (Lottie)
- 🎛️ **Full playback control** — play, pause, stop, speed, loop, direction
- 🔔 **Rich event system** — onLoad, onComplete, onFrame, onError, and more
- 🌐 **Framework wrappers** — React, Vue, Svelte, SolidJS, Web Components
- 🧩 **State machine support** — interactive, stateful animation flows
- 🎨 **Theme support** — dynamic color/property overrides at runtime

---

## 🏗️ Architecture

> How dotlottie-web is structured internally — from your code down to the canvas.

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
    I --> J["⚛️ React\n@lottiefiles/dotlottie-react"]
    I --> K["💚 Vue\n@lottiefiles/dotlottie-vue"]
    I --> L["🔥 Svelte\n@lottiefiles/dotlottie-svelte"]
    I --> M["🔷 SolidJS\n@lottiefiles/dotlottie-solid"]
    I --> N["🌐 Web Component\n@lottiefiles/dotlottie-wc"]

    style pkg fill:#f0f9ff,stroke:#0ea5e9,stroke-width:2px
    style A fill:#ede9fe,stroke:#8b5cf6
    style H fill:#dcfce7,stroke:#10b981
```

---

## 🔄 Animation Lifecycle

> Every animation goes through these states. Understanding this helps you hook into the right events.

```mermaid
stateDiagram-v2
    [*] --> Idle : new DotLottie()

    Idle --> Loading : load(src) called
    Loading --> Ready : ✅ onLoad fired
    Loading --> Error : ❌ onLoadError fired
    Error --> [*]

    Ready --> Playing : play()
    Playing --> Paused : pause()
    Paused --> Playing : play()
    Playing --> Stopped : stop()
    Stopped --> Playing : play()

    Playing --> FrameUpdate : each RAF tick\nonFrame fires
    FrameUpdate --> Playing : next frame

    Playing --> Complete : last frame reached\nonComplete fires
    Complete --> Playing : loop = true → restart
    Complete --> Stopped : loop = false → stop

    Stopped --> [*]

    note right of Loading
        Supports URL, base64,
        ArrayBuffer, and
        local file paths
    end note

    note right of Playing
        onFrame fires on every
        rendered frame with
        current frame number
    end note
```

---

## 🔀 API Request / Data Flow

> What happens under the hood from `new DotLottie()` → animation playing on screen.

```mermaid
sequenceDiagram
    autonumber
    participant Dev as 👨‍💻 Developer
    participant API as DotLottie API
    participant Loader as File Loader
    participant WASM as ⚙️ WASM Engine
    participant Canvas as 🖼️ Canvas

    Dev->>API: new DotLottie({ canvas, src, autoplay, loop })
    API->>Loader: fetch(src)
    alt .lottie format
        Loader->>Loader: unzip → extract JSON + assets
    else .json format
        Loader->>Loader: parse JSON directly
    end
    Loader->>WASM: load animation data
    WASM-->>API: ✅ onLoad event emitted
    API-->>Dev: onLoad callback fires

    opt autoplay = true
        Dev->>API: (auto) play()
    end

    API->>WASM: start render loop
    loop 🔁 Animation Loop (RAF)
        WASM->>Canvas: render frame N
        WASM-->>API: onFrame(frameNumber)
        API-->>Dev: onFrame callback fires
    end

    alt loop = true
        WASM->>WASM: reset to frame 0
    else loop = false
        WASM-->>API: ✅ onComplete event
        API-->>Dev: onComplete callback fires
    end
```

---

## 🚀 Installation

```bash
# npm
npm install @lottiefiles/dotlottie-web

# yarn
yarn add @lottiefiles/dotlottie-web

# pnpm
pnpm add @lottiefiles/dotlottie-web

# CDN (ESM)
import { DotLottie } from 'https://cdn.jsdelivr.net/npm/@lottiefiles/dotlottie-web/+esm';
```

---

## 📖 Usage

### Basic Setup

```html
<!-- 1. Add a canvas to your HTML -->
<canvas id="my-canvas" width="400" height="400"></canvas>
```

```js
// 2. Import and initialize
import { DotLottie } from '@lottiefiles/dotlottie-web';

const dotLottie = new DotLottie({
  canvas: document.getElementById('my-canvas'),
  src: 'https://lottie.host/your-animation-id/animation.lottie',
  autoplay: true,
  loop: true,
});
```

### Playback Controls

```js
dotLottie.play();           // ▶️ Start / resume
dotLottie.pause();          // ⏸️ Pause at current frame
dotLottie.stop();           // ⏹️ Stop and reset to frame 0
dotLottie.setSpeed(2);      // ⏩ 2× speed
dotLottie.setLoop(true);    // 🔁 Enable looping
dotLottie.setFrame(30);     // ⏭️ Jump to frame 30
```

### Event Handling

```js
dotLottie.addEventListener('load', () => {
  console.log('✅ Animation loaded!');
});

dotLottie.addEventListener('complete', () => {
  console.log('🏁 Animation complete!');
});

dotLottie.addEventListener('frame', ({ currentFrame }) => {
  console.log(`🎞️ Frame: ${currentFrame}`);
});

dotLottie.addEventListener('loadError', (err) => {
  console.error('❌ Load failed:', err);
});
```

---

## ⚙️ API Reference

### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `canvas` | `HTMLCanvasElement` | **required** | The canvas element to render into |
| `src` | `string` | **required** | URL or data URI of `.lottie` or `.json` file |
| `autoplay` | `boolean` | `false` | Start playing immediately after load |
| `loop` | `boolean` | `false` | Loop the animation |
| `speed` | `number` | `1` | Playback speed multiplier |
| `mode` | `string` | `"forward"` | Direction: `"forward"`, `"reverse"`, `"bounce"` |
| `backgroundColor` | `string` | `"transparent"` | Canvas background color (hex) |
| `segment` | `[number, number]` | `undefined` | Play only a subset of frames `[startFrame, endFrame]` |
| `useFrameInterpolation` | `boolean` | `true` | Smooth inter-frame blending |
| `themeId` | `string` | `undefined` | Apply a named theme from the .lottie bundle |

### Instance Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `.play()` | `void` | Start or resume animation |
| `.pause()` | `void` | Pause at current frame |
| `.stop()` | `void` | Stop and reset to first frame |
| `.setSpeed(n)` | `void` | Set playback speed (e.g. `2` = 2×) |
| `.setLoop(bool)` | `void` | Enable or disable looping |
| `.setFrame(n)` | `void` | Jump to a specific frame number |
| `.setSegment(start, end)` | `void` | Restrict playback to frame range |
| `.setMode(mode)` | `void` | Set direction (`forward` / `reverse` / `bounce`) |
| `.destroy()` | `void` | Clean up WASM memory and remove listeners |
| `.resize()` | `void` | Recalculate canvas size (call on container resize) |
| `.addEventListener(event, cb)` | `void` | Subscribe to events |
| `.removeEventListener(event, cb)` | `void` | Unsubscribe from events |

### Events

| Event | Payload | Fires When |
|-------|---------|------------|
| `load` | — | Animation file fully loaded and ready |
| `loadError` | `{ error }` | File failed to load or parse |
| `play` | — | Playback started |
| `pause` | — | Playback paused |
| `stop` | — | Playback stopped |
| `complete` | — | Last frame reached (non-looping) |
| `loop` | `{ loopCount }` | Loop iteration completed |
| `frame` | `{ currentFrame }` | Each frame rendered |
| `destroy` | — | Instance destroyed |
| `freeze` | — | Animation frozen (tab inactive) |

---

## 🌐 Framework Support

```mermaid
graph LR
    core["⚙️ @lottiefiles/dotlottie-web\n(Core — Vanilla JS)"]

    core --> react["⚛️ dotlottie-react\nnpm install @lottiefiles/dotlottie-react"]
    core --> vue["💚 dotlottie-vue\nnpm install @lottiefiles/dotlottie-vue"]
    core --> svelte["🔥 dotlottie-svelte\nnpm install @lottiefiles/dotlottie-svelte"]
    core --> solid["🔷 dotlottie-solid\nnpm install @lottiefiles/dotlottie-solid"]
    core --> wc["🌐 dotlottie-wc\nnpm install @lottiefiles/dotlottie-wc"]

    style core fill:#f0f9ff,stroke:#0ea5e9,stroke-width:2px,color:#0c4a6e
    style react fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style vue fill:#dcfce7,stroke:#16a34a,color:#14532d
    style svelte fill:#fff7ed,stroke:#ea580c,color:#7c2d12
    style solid fill:#eff6ff,stroke:#3b82f6,color:#1e3a8a
    style wc fill:#fdf4ff,stroke:#c026d3,color:#701a75
```

### React Example

```jsx
import { DotLottieReact } from '@lottiefiles/dotlottie-react';

export default function App() {
  return (
    <DotLottieReact
      src="https://lottie.host/your-id/animation.lottie"
      loop
      autoplay
      style={{ width: 300, height: 300 }}
    />
  );
}
```

### Vue Example

```vue
<template>
  <DotLottieVue
    src="https://lottie.host/your-id/animation.lottie"
    :loop="true"
    :autoplay="true"
    style="width: 300px; height: 300px"
  />
</template>

<script setup>
import { DotLottieVue } from '@lottiefiles/dotlottie-vue';
</script>
```

---

## 🎬 Live Demo (GitHub Pages / Docs)

> Paste this in your `docs/index.html` or any HTML page — GitHub strips `<script>` from `.md` files.

```html
<canvas id="demo-canvas" width="300" height="300"></canvas>
<script type="module">
  import { DotLottie } from 'https://cdn.jsdelivr.net/npm/@lottiefiles/dotlottie-web/+esm';

  new DotLottie({
    canvas: document.getElementById('demo-canvas'),
    src: 'https://lottie.host/4db68bbd-31f6-4cd8-84eb-189de081159a/IGmMCqhzpt.lottie',
    autoplay: true,
    loop: true,
  });
</script>
```

---

## 📁 Package Structure

```mermaid
graph LR
    root["📦 dotlottie-web (monorepo)"]

    root --> packages["📁 packages/"]
    root --> apps["📁 apps/"]
    root --> docs["📁 docs/"]

    packages --> core["dotlottie-web\n(core library)"]
    packages --> react["dotlottie-react"]
    packages --> vue["dotlottie-vue"]
    packages --> svelte["dotlottie-svelte"]
    packages --> solid["dotlottie-solid"]
    packages --> wc["dotlottie-wc\n(web component)"]

    apps --> demo["demo/\n(dev playground)"]
    docs --> site["documentation site"]

    style root fill:#fef9c3,stroke:#ca8a04
    style packages fill:#f0f9ff,stroke:#0ea5e9
    style apps fill:#f0fdf4,stroke:#16a34a
```

---

## 🤝 Contributing

Contributions are welcome! Please read the [contributing guide](CONTRIBUTING.md) first.

```bash
# Clone the repo
git clone https://github.com/LottieFiles/dotlottie-web.git
cd dotlottie-web

# Install dependencies (uses pnpm workspaces)
pnpm install

# Build all packages
pnpm build

# Run dev server
pnpm dev
```

---

## 📄 License

MIT © [LottieFiles](https://lottiefiles.com)

---

<div align="center">

Made with ❤️ by the [LottieFiles](https://lottiefiles.com) team

[Website](https://lottiefiles.com) · [Docs](https://developers.lottiefiles.com) · [Discord](https://discord.gg/lottiefiles) · [Twitter](https://twitter.com/lottiefiles)

</div>
