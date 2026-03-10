---
name: pwa
description: "PWA Features Agent - Analyze project and add relevant Progressive Web App capabilities based on whatpwacando.today"
category: enhancement
complexity: standard
mcp-servers: [serena, playwright]
---

# /pwa - PWA Features Agent

> Analyzes the current project and recommends + implements relevant PWA features based on project type, tech stack, and existing capabilities.

## Triggers
- Adding PWA functionality to a project
- Auditing existing PWA implementation
- Improving app installability and offline experience
- Adding native-like features (push, camera, geolocation, etc.)

## Context Trigger Pattern
```
/pwa [--audit] [--implement <feature>] [--list] [--stack <tech>]
```

### Options
| Flag | Effect |
|------|--------|
| `--audit` | Audit existing PWA implementation and score it |
| `--implement <feature>` | Implement a specific PWA feature |
| `--list` | List all available PWA features with browser support |
| `--stack <tech>` | Specify tech stack (laravel, react, vue, next, nuxt, svelte) |

## Behavioral Flow

### 1. Project Analysis (auto-detect)
```yaml
Detect:
  - Framework/stack (package.json, composer.json, artisan, etc.)
  - Existing service worker (sw.js, service-worker.js)
  - Existing web manifest (manifest.json, manifest.webmanifest)
  - Existing PWA features already implemented
  - App category (maps/geospatial, media, ecommerce, communication, productivity, blog)
  - Target platforms (mobile-first, desktop, hybrid)
```

### 2. Feature Categorization by Project Type

#### 🌍 Geospatial / Maps Apps
**Priority**: Geolocation, Offline Support, Background Sync, Network Info, Orientation, Motion, Installation
```
Use case: Alert systems, delivery tracking, field apps
Examples: Zonex, mapping tools, logistics dashboards
```

#### 💬 Communication / Social Apps
**Priority**: Notifications, Declarative Web Push, Incoming Call Notifications, Web Share, Contact Picker, Audio Recording
```
Use case: Chat apps, social networks, collaboration tools
```

#### 🎬 Media / Content Apps
**Priority**: Media Capture, Picture-in-Picture, Audio (Media Session), Audio Recording, Screen Capturing, Element Capture, Speech Recognition, Speech Synthesis
```
Use case: Video conferencing, streaming, podcasts, education
```

#### 📁 File / Document Apps
**Priority**: File System, File Handling API, Compression, Storage API, Background Fetch API, Offline Support
```
Use case: Document editors, note-taking, design tools
```

#### 🛒 E-commerce / Business Apps
**Priority**: Payment, Authentication (WebAuthn), Notifications, Shortcuts, Installation, Web Share
```
Use case: Online stores, booking systems, SaaS dashboards
```

#### 🔧 IoT / Hardware Apps
**Priority**: Bluetooth, NFC, Vibration, Barcode Detection, Face Detection, Geolocation
```
Use case: Device controllers, inventory scanners, access systems
```

#### 📰 Blog / News / Publishing
**Priority**: Offline Support, Notifications, Web Share, Background Sync, Shortcuts, View Transitions, Speech Synthesis
```
Use case: News sites, blogs, content platforms
```

#### 🏭 General / Productivity Apps
**Priority**: Installation, Offline Support, Notifications, Storage API, Background Sync, Authentication, Shortcuts, View Transitions
```
Use case: To-do apps, dashboards, admin panels
```

### 3. PWA Feature Catalog

Each feature below includes: browser support, implementation complexity, and implementation guidance.

---

#### ✅ TIER 1 — Universal (implement in every PWA)

**Offline Support (Service Worker)**
- Browser: All modern browsers
- Complexity: Medium
- Implement: Register SW, cache shell + API responses with Workbox or manual Cache API
- Laravel: Use `vite-plugin-pwa` (Vite) or manual `public/sw.js`

**Web App Manifest**
- Browser: All modern browsers
- Complexity: Low
- Implement: `manifest.webmanifest` with name, icons (192/512px), display, theme_color, start_url
- Include: screenshots for app store-like install UI

**Installation (beforeinstallprompt)**
- Browser: Chrome/Edge/Samsung (not Safari)
- Complexity: Low
- Implement: Intercept `beforeinstallprompt`, show custom install button, track installs

**Storage API (IndexedDB / Cache Storage)**
- Browser: All modern browsers
- Complexity: Medium
- Implement: Use IndexedDB for structured data, Cache API for assets; use `navigator.storage.persist()` for durable storage

**Background Sync API**
- Browser: Chrome/Edge/Android (not Safari/Firefox)
- Complexity: Medium
- Implement: Queue failed requests in IndexedDB, register sync tag, retry on `sync` event in SW

---

#### ✅ TIER 2 — Highly Recommended

**Push Notifications**
- Browser: Chrome/Firefox/Edge/Safari 16.4+ (iOS 16.4+)
- Complexity: Medium-High
- Implement: Request permission, subscribe with VAPID keys, handle `push` event in SW, show notification
- Laravel: Use `webpush()` helper + `WebPushChannel`

**Declarative Web Push**
- Browser: Chrome 128+ (experimental)
- Complexity: Low (no SW needed)
- Implement: Send push via subscription with pre-built notification payload

**Shortcuts**
- Browser: Chrome/Edge (desktop + Android)
- Complexity: Low
- Implement: Add `shortcuts` array to manifest with `name`, `url`, `description`, `icons`

**View Transitions**
- Browser: Chrome/Edge/Safari 18+
- Complexity: Low-Medium
- Implement: Wrap navigation in `document.startViewTransition()`, use CSS `view-transition-name`

**Web Share**
- Browser: All mobile browsers, Safari, Chrome
- Complexity: Low
- Implement: `navigator.share({title, text, url})` with fallback copy-to-clipboard

**Authentication (WebAuthn)**
- Browser: All modern browsers
- Complexity: High
- Implement: Use `PublicKeyCredential` API; integrate with Laravel Fortify or custom auth

---

#### ✅ TIER 3 — Feature-Specific

**Geolocation**
- Browser: All modern browsers (HTTPS required)
- Complexity: Low
- Implement: `navigator.geolocation.getCurrentPosition()` / `watchPosition()`

**Media Capture (Camera/Mic)**
- Browser: All modern browsers (HTTPS required)
- Complexity: Medium
- Implement: `navigator.mediaDevices.getUserMedia({video: true, audio: true})`

**File System Access**
- Browser: Chrome/Edge (not Safari/Firefox)
- Complexity: Medium
- Implement: `window.showOpenFilePicker()` / `showSaveFilePicker()`

**File Handling API**
- Browser: Chrome/Edge desktop only
- Complexity: Medium
- Implement: Add `file_handlers` to manifest, handle `launchQueue` in SW

**Barcode Detection**
- Browser: Chrome/Edge/Android WebView
- Complexity: Low
- Implement: `new BarcodeDetector({formats: ['qr_code']}).detect(imageSource)`

**Background Fetch API**
- Browser: Chrome/Edge/Android
- Complexity: Medium
- Implement: `registration.backgroundFetch.fetch(id, urls, options)` for large downloads

**Wake Lock**
- Browser: Chrome/Edge/Safari 16.4+
- Complexity: Low
- Implement: `navigator.wakeLock.request('screen')` to prevent screen dimming

**Network Info**
- Browser: Chrome/Edge/Android (not Safari/Firefox)
- Complexity: Low
- Implement: `navigator.connection.effectiveType` to adapt content quality

**Vibration**
- Browser: Chrome/Firefox/Android (not iOS)
- Complexity: Low
- Implement: `navigator.vibrate([200, 100, 200])` for haptic feedback

**Payment Request API**
- Browser: All modern browsers
- Complexity: High
- Implement: `new PaymentRequest(methods, details)` with Apple Pay / Google Pay / card methods

**Protocol Handling**
- Browser: Chrome/Edge
- Complexity: Low
- Implement: `navigator.registerProtocolHandler('web+app', '/handle?url=%s', 'App Name')` + manifest `protocol_handlers`

**Bluetooth (Web Bluetooth)**
- Browser: Chrome/Edge/Android (not iOS/Firefox)
- Complexity: High
- Implement: `navigator.bluetooth.requestDevice({filters: [{services: [...]}]})`

**NFC (Web NFC)**
- Browser: Android Chrome only
- Complexity: Medium
- Implement: `new NDEFReader()`, call `.scan()` and `.write()`

**Speech Synthesis**
- Browser: All modern browsers
- Complexity: Low
- Implement: `new SpeechSynthesisUtterance(text)`, `speechSynthesis.speak(utterance)`

**Speech Recognition**
- Browser: Chrome/Edge (not Firefox/Safari)
- Complexity: Medium
- Implement: `new (window.SpeechRecognition || window.webkitSpeechRecognition)()`

**Picture-in-Picture**
- Browser: All modern browsers + Document PiP in Chrome 116+
- Complexity: Low-Medium
- Implement: `videoElement.requestPictureInPicture()` or `documentPictureInPicture.requestWindow()`

**Audio (Media Session API)**
- Browser: All modern browsers
- Complexity: Low
- Implement: Set `navigator.mediaSession.metadata` and action handlers for lock screen controls

**Contact Picker**
- Browser: Android Chrome / iOS Safari 14.5+ only
- Complexity: Low
- Implement: `navigator.contacts.select(['name', 'email', 'tel'])`

**Face Detection**
- Browser: Chrome/Edge (behind flag on most platforms)
- Complexity: Low
- Implement: `new FaceDetector().detect(imageSource)`

**Orientation / Motion**
- Browser: All mobile browsers (iOS requires permission)
- Complexity: Low
- Implement: Listen to `deviceorientation` / `devicemotion` events; call `DeviceOrientationEvent.requestPermission()` on iOS

**Screen Capturing**
- Browser: Chrome/Edge
- Complexity: High
- Implement: `navigator.mediaDevices.getDisplayMedia()` + Capture Handle API

**AR/VR (WebXR)**
- Browser: Chrome/Edge Android, Meta Quest browser
- Complexity: High
- Implement: `navigator.xr.requestSession('immersive-ar')` with WebXR Device API

**Incoming Call Notifications**
- Browser: Chrome (desktop + Android)
- Complexity: High
- Implement: Use notification with `actions`, handle `notificationclick` for call accept/decline

**Multi Touch**
- Browser: All mobile browsers
- Complexity: Low
- Implement: Listen to `touchstart`, `touchmove`, `touchend` with `event.touches`

**Compression (Streams API)**
- Browser: All modern browsers
- Complexity: Medium
- Implement: `new CompressionStream('gzip')` / `DecompressionStream` for file processing

**Viewport (Virtual Keyboard API)**
- Browser: Chrome/Edge
- Complexity: Low
- Implement: Set `viewport-fit=cover` meta, handle `visualViewport` resize events for keyboard

**Audio Session API**
- Browser: Safari only (experimental elsewhere)
- Complexity: Low
- Implement: `navigator.audioSession.type = 'play-and-record'` for audio mixing behavior

---

### 4. Implementation Templates

#### Service Worker (Universal)
```javascript
// public/sw.js
const CACHE_NAME = 'app-v1';
const SHELL_ASSETS = ['/', '/app.css', '/app.js', '/manifest.webmanifest'];

self.addEventListener('install', e => {
  e.waitUntil(caches.open(CACHE_NAME).then(c => c.addAll(SHELL_ASSETS)));
  self.skipWaiting();
});

self.addEventListener('activate', e => {
  e.waitUntil(caches.keys().then(keys =>
    Promise.all(keys.filter(k => k !== CACHE_NAME).map(k => caches.delete(k)))
  ));
  self.clients.claim();
});

self.addEventListener('fetch', e => {
  e.respondWith(
    caches.match(e.request).then(cached => cached || fetch(e.request))
  );
});
```

#### Web App Manifest
```json
{
  "name": "App Name",
  "short_name": "App",
  "description": "App description",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#1a1a2e",
  "icons": [
    {"src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png", "purpose": "any maskable"},
    {"src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png"}
  ],
  "shortcuts": [],
  "screenshots": [],
  "categories": ["utilities"],
  "protocol_handlers": [],
  "file_handlers": []
}
```

#### Install Prompt
```javascript
let deferredPrompt;
window.addEventListener('beforeinstallprompt', e => {
  e.preventDefault();
  deferredPrompt = e;
  document.getElementById('install-btn').style.display = 'block';
});

document.getElementById('install-btn').addEventListener('click', async () => {
  if (!deferredPrompt) return;
  deferredPrompt.prompt();
  const { outcome } = await deferredPrompt.userChoice;
  deferredPrompt = null;
});
```

#### Background Sync
```javascript
// In app code - queue failed requests
async function syncLater(data) {
  const db = await openDB();
  await db.put('sync-queue', { ...data, timestamp: Date.now() });
  const reg = await navigator.serviceWorker.ready;
  await reg.sync.register('sync-data');
}

// In SW
self.addEventListener('sync', e => {
  if (e.tag === 'sync-data') {
    e.waitUntil(processSyncQueue());
  }
});
```

#### Push Notifications (Laravel)
```php
// Install: composer require laravel-notification-channels/webpush
// In .env: VAPID_PUBLIC_KEY=... VAPID_PRIVATE_KEY=...

// In Notification class:
public function via($notifiable) {
    return [WebPushChannel::class];
}

public function toWebPush($notifiable, $notification) {
    return (new WebPushMessage)
        ->title('Alert Title')
        ->body('Notification body')
        ->icon('/icons/icon-192.png')
        ->action('View', '/alerts');
}
```

---

### 5. Audit Checklist

When running `--audit`, check:

```yaml
Manifest:
  - [ ] manifest.webmanifest exists and linked
  - [ ] name + short_name set
  - [ ] Icons: 192px and 512px (maskable)
  - [ ] display: standalone or fullscreen
  - [ ] theme_color and background_color set
  - [ ] start_url defined
  - [ ] screenshots for install UI

Service Worker:
  - [ ] SW registered on page load
  - [ ] Offline fallback page exists
  - [ ] App shell cached on install
  - [ ] Cache versioning strategy
  - [ ] Background sync registered

Notifications:
  - [ ] Push permission requested appropriately (not on load)
  - [ ] VAPID keys configured
  - [ ] Notification click handler in SW

Performance:
  - [ ] Lighthouse PWA score > 90
  - [ ] Core Web Vitals passing
  - [ ] HTTPS enforced

Features Used:
  - [ ] Installation prompt handled
  - [ ] Share target / Web Share implemented
  - [ ] Relevant hardware APIs added
```

---

### 6. Stack-specific Notes

**Laravel + Vite**:
```bash
npm install vite-plugin-pwa
# In vite.config.js: import { VitePWA } from 'vite-plugin-pwa'
# Handles SW + manifest generation automatically
```

**Next.js**:
```bash
npm install next-pwa
# In next.config.js: withPWA({ dest: 'public' })
```

**Nuxt**:
```bash
npm install @vite-pwa/nuxt
# In nuxt.config.ts: modules: ['@vite-pwa/nuxt']
```

**React / Vue (Vite)**:
```bash
npm install vite-plugin-pwa
# In vite.config.ts: VitePWA({ registerType: 'autoUpdate', ... })
```

---

### 7. Browser Support Quick Reference

| Feature | Chrome | Firefox | Safari | iOS Safari | Edge |
|---------|--------|---------|--------|-----------|------|
| Service Worker | ✅ | ✅ | ✅ | ✅ 11.1+ | ✅ |
| Web Push | ✅ | ✅ | ✅ 16.4+ | ✅ 16.4+ | ✅ |
| Install Prompt | ✅ | ❌ | ❌ | ❌ | ✅ |
| Background Sync | ✅ | ❌ | ❌ | ❌ | ✅ |
| Background Fetch | ✅ | ❌ | ❌ | ❌ | ✅ |
| File System | ✅ | ❌ | ❌ | ❌ | ✅ |
| File Handling | ✅ | ❌ | ❌ | ❌ | ✅ |
| Web Bluetooth | ✅ | ❌ | ❌ | ❌ | ✅ |
| Web NFC | ✅ Android | ❌ | ❌ | ❌ | ❌ |
| Geolocation | ✅ | ✅ | ✅ | ✅ | ✅ |
| Web Share | ✅ | ✅ | ✅ | ✅ | ✅ |
| WebAuthn | ✅ | ✅ | ✅ | ✅ | ✅ |
| Payment API | ✅ | ✅ | ✅ | ✅ | ✅ |
| View Transitions | ✅ | ❌ | ✅ 18+ | ✅ 18+ | ✅ |
| Wake Lock | ✅ | ❌ | ✅ 16.4+ | ✅ 16.4+ | ✅ |
| Barcode Detection | ✅ | ❌ | ❌ | ❌ | ✅ |
| Speech Recognition | ✅ | ❌ | ✅ | ✅ | ✅ |
| Contact Picker | ✅ Android | ❌ | ❌ | ✅ 14.5+ | ❌ |

---

## Output Format

After analysis, provide:

1. **PWA Score** — current implementation completeness (0-100%)
2. **Recommended Features** — prioritized list for this project type
3. **Quick Wins** — features implementable in < 30 minutes
4. **Implementation Plan** — step-by-step for selected features
5. **Code snippets** — ready-to-paste implementation

## Boundaries
**Will**: Audit, recommend, and implement PWA features appropriate for the project
**Won't**: Force Chrome-only features without clearly marking browser limitations
**Always**: Note browser support caveats before implementing limited-support features
