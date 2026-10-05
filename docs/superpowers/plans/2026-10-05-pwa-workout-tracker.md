# PWA Workout Tracker Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the workout tracker into a high-performance, offline-capable PWA with direct CDN GIFs, haptic vibration, rich technique modal, sound alerts, and weekly streak tracking.

**Architecture:** A standalone Progressive Web App with vanilla JS, CSS custom properties for sleek dark/light theme, Service Worker with cache-first strategy for shell and assets, Web Audio API, Web Vibration API, Wake Lock API, and pre-indexed exercise metadata.

**Tech Stack:** HTML5, CSS3, Vanilla JavaScript (ES6+), Web App Manifest, Service Worker Cache API, Web Audio API, Vibration API, jsDelivr / ExerciseDB static CDN.

## Global Constraints
- Must run cleanly on both iOS (Safari standalone) and Android (Chrome PWA).
- No external heavy dependencies or frameworks (keep it fast, zero build step, instant load).
- Preserve all existing user workout routines, timer durations, and medical core notes (scar safety).
- Ensure 100% offline functionality after first load via Service Worker.

---

### Task 1: Exercise Metadata Catalog & Direct CDN URLs
**Files:**
- Create/Modify: `index.html` (embedded catalog data structure)
- Scratch: `C:\Users\Felipe\.gemini\antigravity\brain\768bb41d-fe01-4b2d-9ffb-e1fcee89a0bb\scratch\resolved_exercises.json`

**Interfaces:**
- Produces: `EX_MAP` dictionary containing `{ name, sets, reps, rest, work, gif, tips }` for every exercise across Warmup, Days 1-5, and Core.

- [ ] **Step 1: Validate resolved exercise GIFs**
Verify that all 27 exercises have valid direct URLs and fallback YouTube queries.
- [ ] **Step 2: Embed structured catalog into index.html**
Replace dynamic fetch queries with pre-indexed data structure containing technique tips and muscle targets.
- [ ] **Step 3: Test card rendering with direct GIFs**
Verify that exercise cards load GIFs instantly without API search latency.

---

### Task 2: PWA Assets (Manifest, Service Worker & Icons)
**Files:**
- Create: `manifest.json`
- Create: `sw.js`
- Create: `icon.svg`
- Modify: `index.html` (add manifest link, theme-color meta, and service worker registration)

**Interfaces:**
- Produces: PWA installability on Android/iOS and offline asset caching.

- [ ] **Step 1: Create icon.svg**
Clean, modern fitness dumbbell / lightning bolt SVG icon with gradients suitable for app icons.
- [ ] **Step 2: Create manifest.json**
PWA manifest with name "Entrenamiento", theme `#0e141a`, background `#0e141a`, display `standalone`.
- [ ] **Step 3: Create sw.js**
Service Worker with cache-first strategy for app shell (`index.html`, `manifest.json`, `icon.svg`) and runtime caching for exercise GIFs.
- [ ] **Step 4: Register Service Worker in index.html**
Add registration snippet and offline fallback logic.

---

### Task 3: Exercise Technique Modal & Video Fallback
**Files:**
- Modify: `index.html` (modal markup, CSS, and JS handlers)

**Interfaces:**
- Consumes: `EX_MAP` tips and GIFs from Task 1.
- Produces: `openExerciseModal(key)` and `closeExerciseModal()`.

- [ ] **Step 1: Create modal dialog UI and CSS**
Modal with high-res GIF preview, target muscles, step-by-step technique tips, and YouTube search link.
- [ ] **Step 2: Hook tap on exercise card/thumbnail to open modal**
Allow user to tap any exercise card or thumbnail to view details.
- [ ] **Step 3: Test modal opening, backdrop click, and close button**
Verify smooth opening, closing with ESC key or tap outside.

---

### Task 4: Haptic Vibration, Audio & Bottom Timer Upgrades
**Files:**
- Modify: `index.html` (timer logic, audio context, vibration calls, CSS)

**Interfaces:**
- Produces: `vibrate(pattern)` utility, circular/linear progress timer bar, distinct work vs rest states.

- [ ] **Step 1: Implement vibration helper**
Call `navigator.vibrate([100])` on 3-2-1 countdown and `navigator.vibrate([150, 100, 200])` on completion.
- [ ] **Step 2: Upgrade bottom timer bar UI**
Add visual progress indicator (% of interval remaining), distinct colors for Work vs Rest, large touch buttons (+15s, Saltar, Cancelar).
- [ ] **Step 3: Integrate Wake Lock and audio chime**
Ensure screen stays awake and sound triggers reliably when timer ends.

---

### Task 5: Weekly Streak Tracker & Workout Completion Celebration
**Files:**
- Modify: `index.html` (header stats, confetti/celebration screen, localStorage history)

**Interfaces:**
- Consumes: Workout completion state.
- Produces: Weekly streak dots (Lun-Vie), active days count, celebration dialog with stats.

- [ ] **Step 1: Calculate weekly workout history**
Read `localStorage` to compute days trained this week and total completed sessions.
- [ ] **Step 2: Render weekly mini-calendar bar**
Visual pill showing Lun Mar Mié Jue Vie with checkmarks for trained days.
- [ ] **Step 3: Add workout completion banner / confetti celebration**
When all sets of the day are done, display an encouraging completion card with stats.

---

### Task 6: UI Refinement, Verification & Deployment
**Files:**
- Modify: `index.html`
- Git commit & push: all files to `origin/main`

- [ ] **Step 1: Audit responsive design & touch ergonomics**
Test on mobile viewport dimensions, dark mode contrast, safe-area-insets.
- [ ] **Step 2: Run verification checks**
Check console errors, service worker registration, git status.
- [ ] **Step 3: Commit and push to GitHub**
Push all improvements to `git@github.com:Felivander/Entrenamiento.git`.
