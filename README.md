# AEcho Audio Engine

**AEcho** is a minimal, gapless Web Audio engine for seamlessly handling complex playback sequences like **intro → loop → exit**. Designed for web projects that demand precise, responsive audio transitions (e.g., games, music players, interactive art).

---

## 🌟 Features

* Seamless **intro → loop → exit** playback with zero gaps
* Mobile browser support
* Sample-accurate timing using `AudioContext` scheduling
* Built-in stereo visualizer (optional)
* Simple API for integration and control

---

# Live interactive demo [here](https://bojay.be/AngelEcho)
# Intro and loop [here](https://bojay.be/ganthem)
# One audio file [here](https://bojay.be/singleaudio)

---
## 🔧 Setup

### 1. Include the Engine

Download or copy `aecho.js` into your project directory, and include it in your HTML:

```html
<script src="aecho.js"></script>
```

---

### 2. Prepare Your Audio Files

You must provide **three separate audio files**:

| Type    | Description                                 | File Format            |
| ------- | ------------------------------------------- | ---------------------- |
| `intro` | The segment that plays once at the start    | `.mp3`, `.wav`, `.ogg` |
| `loop`  | The looping segment (repeats endlessly)     | `.mp3`, `.wav`, `.ogg` |
| `exit`  | The final segment (plays once when stopped) | `.mp3`, `.wav`, `.ogg` |

If you only want to use one, that's fine. Look at step 3 below.

Put these files in a folder like `/audio/` in your project:

```
/your-project/
├── index.html
├── aecho.js
└── audio/
    ├── intro.mp3
    ├── loop.mp3
    └── exit.mp3
```

---

### 3. Initialize AEcho

Call the `AEchoAudioEngine.init()` method **after** the page loads.

You must pass in:

* `intro` (string) – path to the intro audio file
* `loop` (string) – path to the loop audio file
* `exit` (string) – path to the exit audio file

Example:

```html
<script>
  AEchoAudioEngine.init({
    intro: 'audio/intro.mp3',
    loop: 'audio/loop.mp3',
    exit: 'audio/exit.mp3',
  })
</script>
```

If you only want to use a single audio file, ignore `intro` and `exit`, use `loop`.

If you only want an `intro` and a `loop`, do so.

## ▶️ Basic Controls

Once initialized, you can control playback with:

```js
AEchoAudioEngine.play();  // Starts playback: intro → loop
AEchoAudioEngine.stop();  // Triggers exit: finishes current loop, then plays exit
```

---

## 📊 Audio Behavior Summary

```
[ intro ] → [ loop → loop → loop → … ] → (on stop) → [ exit ]
```

* The **intro** plays **once**
* The **loop** plays **repeatedly**
* When `stop()` is called, it finishes the **current loop**, then plays the **exit**
* All transitions are **sample-accurate and gapless** `to an extent`. Heavy ram usage or low-end devices may stutter. It happens sometimes, I need to work out the kinks. IWOMM.

---

## 📊 Visualizer (Optional)

To show the built-in stereo bar visualizer, include a `<canvas>` element and pass it in via the `canvas` option during `init()`:

```html
<canvas id="visualizer"></canvas>
```

The settings can be customized by changing the integer in the `state.analyser.fftSize = 4096;` line near the top, and adjusting certain`const` variables in the `startVisualizer` function found on line 198.

## ⚠️ Important Notes

* All audio files must be \*\*local\*\* or **hosted on the same domain** (or allow CORS).
* Audio files must be **fully preloaded** before playback will work.
* User interaction (click, tap) is required to start audio in modern browsers (especially mobile).
* AEcho only supports `.mp3`, `.wav`, and `.ogg` reliably. FLAC may not work in all browsers. You may add other options if you know how.

---

## 💡 Troubleshooting

| Problem                | Solution                                                                 |
| ---------------------- | ------------------------------------------------------------------------ |
| Nothing plays          | Ensure audio paths are correct, and play is triggered by user action     |
| Audio loops with a gap | Double-check the loop file is trimmed properly; AEcho handles the timing |
| Exit doesn't play      | Ensure `stop()` is called *after* `play()` has started                   |

# AEcho, in it's current state, can be ***highly unstable***. It is still *very* early in it's lifespan, and you may encounter numerous issues. If that is the case, create an issue in this repo, but don't rely on a fix immediately. If you know how to fix it, please do, and I can merge it. Or you can. I've never worked with people on Github so I'm not sure. Also I lied, A single audio file does not work yet.

---

## 📄 License

MIT License — free to use, modify, and distribute.

