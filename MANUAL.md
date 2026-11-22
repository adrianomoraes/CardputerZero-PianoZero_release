# CardputerZero Piano Zero — User Manual

Complete operating guide for **Piano Zero (`v0.2.0`)** on the **M5Stack CardputerZero**.

> **Note on SoundFont Banks:** The Piano Zero `.deb` package (`~730 KB`) installs the application, icon, and demo MIDI pieces only — **`.sf2` instrument banks are not bundled in the `.deb`**. On first launch (or anytime via **`BNK → Download Online Banks`**), you can download any of the 5 curated acoustic & electric piano banks directly over Wi-Fi onto your microSD card, or copy your own `.sf2` files via `scp`.

---

## 1. Navigation & Controls

### CardputerZero Keyboard
| Key | Action |
|---|---|
| **`Tab`** | Cycle through the 7 tabs (`PLY → BNK → TCH → FX → MID → AUD → MON`) |
| **`F` / `Up`** | Move selection up |
| **`X` / `Down`** | Move selection down |
| **`Z` / `Left`** | Decrease value / previous item / seek `-10s` |
| **`C` / `Right`** | Increase value / next item / seek `+10s` |
| **`Enter`** | Load bank, toggle setting, Play/Pause MIDI, or save configuration |
| **`Esc` (tap)** | **MIDI Panic** (immediately silences all sounding voices & releases sustain) |
| **`Esc` (hold 0.8 s)** | Cleanly save settings and exit back to `APPLaunch` |

### Standalone Mini-Keyboard (`PLY` Tab)
When on the **`PLY`** tab, you can play directly on the CardputerZero keyboard:
- **White keys**: `Q W E R T Y U I` (`C D E F G A B C+1`)
- **Black keys**: `2 3 5 6 7` (`C# D# F# G# A#`)
- **Sustain Pedal**: Hold **`Space`**
- **Octave Shift**: **`-`** (octave down) / **`=`** (octave up), range `C2 – C6`

---

## 2. USB MIDI Controllers & Alesis V61 Controls

Any class-compliant USB MIDI keyboard and sustain pedal (`CC 64`) works out-of-the-box via ALSA Sequencer hot-plugging (scanned every 2 seconds).

### Alesis V61 Latching Toggle Buttons (`CC 48–51`)
The Alesis V61's 4 illuminated buttons operate as **latching toggles** (LED ON = `127`, LED OFF = `0`), and Piano Zero syncs directly to their latched state:

| Button | CC | LED ON (`≥64`) | LED OFF (`<64`) |
|---|---|---|---|
| **Button 1** | `CC 48` | **`NAV` Mode ON** (knobs navigate UI) | **`NAV` Mode OFF** (knobs control live sound) |
| **Button 2** | `CC 49` | **MIDI Player PLAY / RESUME** | **MIDI Player STOP** |
| **Button 3** | `CC 50` | **Hall Reverb ON** | **Hall Reverb OFF** |
| **Button 4** | `CC 51` | **Piano Mechanics ON** | **Piano Mechanics OFF** |

### Alesis V61 Rotary Knobs (`CC 20–23`)
| Knob | CC | Play Mode (`NAV` OFF) | `NAV` Mode (`NAV` ON) |
|---|---|---|---|
| **`K1`** | `CC 20` | Piano Gain (`10% – 150%`) | Switch UI Tab (`PLY..MON`) |
| **`K2`** | `CC 21` | Touch Curve (`0.40 – 2.50`) | Select Row |
| **`K3`** | `CC 22` | Hall Reverb Send (`0 – 127`) | Adjust Selected Parameter |
| **`K4`** | `CC 23` | **Overall Master Volume (`0% – 100%`)** | Select Preset / MIDI Piece |

*(Standard MIDI `CC 7` also controls Master Piano Gain.)*

---

## 3. Tab-by-Tab Reference

### `PLY` — Play Dashboard
- Displays active bank name, streaming badge (`DFD STRM` or `IN RAM`), memory footprint, Gain/Curve/Octave readout, and live **`MCH`** (Mechanics), **`REV`** (Reverb), and **`PED`** (Sustain Pedal) status pills.
- Includes a real-time **24-bar oscilloscope**, stereo **`L`/`R` VU meters**, and an **88-key visualizer** (pressed keys light up in neon teal; sustained keys glow in cyan).

### `BNK` — Piano Banks, In-App Downloader & DFD Streaming
- **First-Run Auto-Prompt**: If no `.sf2` banks are installed yet, Piano Zero automatically opens **`Download Online Banks`** so you can pick a bank over Wi-Fi right on the CardputerZero.
- **Download Online Banks (`[ENTER]`)**: Available anytime (even when banks are already installed) to download from 5 curated acoustic & electric piano banks with instrument type, source/license, download size, DFD RAM usage, live progress bar, and `Esc` cancel:
  - **Salamander Grand Full** (`695 MB` + `36 MB` mechanics) — Yamaha C5 Concert Grand, 14 velocity layers (FreePats / A. Holm, CC-BY 3.0)
  - **Salamander Grand Lite** (`237 MB` + `36 MB` mechanics) — Yamaha C5 Concert Grand, 6 velocity layers (FreePats / A. Holm, CC-BY 3.0)
  - **Nice Steinway v3.8** (`205 MB`) — Steinway Concert Grand (`soundfonts4u` / Hugging Face)
  - **Rhodes EPs Plus v2.3** (`50 MB`) — Multi-preset Fender Rhodes MkI/II & Suitcase electric pianos (`soundfonts4u` / Hugging Face)
  - **Abbey Steinway D** (`38 MB`) — Compact Steinway D starter grand (`soundfonts4u` / Hugging Face)
- **Last-Bank Recall**: Piano Zero automatically remembers the most recently loaded bank and reloads it (along with its per-bank preset & mechanics profile) the next time you open the app.
- Lists all `.sf2` banks found in `/usr/share/APPLaunch/share/piano-banks/` and `~/.config/cardputerzero-pianozero/banks/` (excluding `mech.sf2`).
- Press **`Enter`** on any bank to hot-load it asynchronously (takes ~2–4 s via DFD).
- **Per-Bank Profiles**: Touch curve, gain, mechanics levels, and preset choice are automatically saved per bank in `~/.config/cardputerzero-pianozero/profile_<bank>.cfg`.
- **DFD Attack RAM**: Adjusts how much of each sample's attack transient is preloaded in RAM (`0.37 s / 0.56 s / 0.74 s`). Default `0.56 s` (`24k` frames) uses only `~39 MB` RAM for the 695 MB full Salamander bank.

### `TCH` — Touch Curve & Piano Mechanics
- **Touch Curve (`0.40 – 2.50`)**: Maps incoming MIDI velocity via `out = 127 * (in / 127)^curve`. Values `< 1.0` make soft playing louder/more sensitive; values `> 1.0` require a firmer touch. The live graph plots the curve and flashes an orange dot for your last played note.
- **Piano Gain (`0.05 – 2.00`)**: Synthesizer output gain (default `0.60` matches `exxpiano`'s zero-clip headroom for *La Campanella*).
- **Mechanics FX (`ON / OFF`)**: Enables/disables `mech.sf2` hammer/damper, string resonance, and pedal noise.
- **Hammer Boost (`-30..+30 dB`)** & **Hammer Exp (`0.0..4.0`)**: Controls key-release hammer/damper knock loudness and velocity scaling.
- **String Reso (`-30..+30 dB`)**: Sympathetic string resonance on key release (deferred until pedal lift when sustain is held).
- **Pedal Noise (`-30..+30 dB`)**: Mechanical sustain pedal press/release thump.
- **Randomness (`0.0..3.0`)**: Organic dB jitter and neighbor-key knock variation.
- **Reso Burst (`0..48` voices)**: Caps simultaneous resonance voices on pedal lift while applying makeup gain to preserve natural loudness.

### `FX` — Hall Reverb & Tone
- **Hall Reverb**: Freeverb stereo reverb (`Active`, `Send 0..127` → `0..50%`, `Room Size`, `HF Damping`, `Stereo Width`, `Reverb Level`).
- **Tone Tilt (`-6.0..+6.0 dB`)**: Smooth 1 kHz tilt EQ (negative = warmer/mellower, positive = brighter/crisper).
- **Bus Limiter**: C1-continuous soft-knee stereo limiter (`SOFT C1` / `OFF`) prevents digital clipping during fortissimo passages.

### `MID` — MIDI Player & Controller
- Plays standard Format 0 & 1 `.mid` files from `/usr/share/APPLaunch/share/piano-midi/` and `~/.config/cardputerzero-pianozero/midi/` through the active piano bank and mechanics engine.
- Supports **Play/Pause (`Enter`)**, **Stop (`S`)**, **Seek `-10s`/`+10s` (`Z`/`C` on Transport row)**, **Repeat Loop**, **Transpose (`-12..+12 st`)**, and **USB MIDI In** reconnection.

### `AUD` — Audio Output
- Automatically selects your USB audio interface on startup when connected; switch live between PipeWire sinks (`[USB]`, `[INT]`) and raw ALSA `plughw:` devices.
- Adjust **System Volume**, **Sample Rate (`44.1 / 48 kHz`)**, **Period Size (`64 / 128 / 256 / 512`)**, **Periods (`2 / 3 / 4`)**, and **Polyphony (`64 – 256`)**.
- Press **`Enter`** on **Test Grand Chord** to audition audio output without a MIDI controller.

### `MON` — System Monitor
- 1 Hz live telemetry: 4-core CPU bars, SoC temperature (`°C`), total RAM & app RSS (`MB`), DFD SD read speed (`MB/s`) & active stream count, active/peak voices, audio latency (`ms`), ALSA XRuns, DFD underruns, and SD free space (`GB`). Press **`Enter`** to reset XRun counters.

---

## 4. Adding Custom `.sf2` Banks or `.mid` Files
Copy files via `scp` onto your CardputerZero:
```bash
scp MyPiano.sf2 adrianomoraes@<ip>:~/.config/cardputerzero-pianozero/banks/
scp MyPiece.mid adrianomoraes@<ip>:~/.config/cardputerzero-pianozero/midi/
```
Then press **`Enter`** on **Rescan SD Banks** in the `BNK` tab (or open the `MID` tab).
