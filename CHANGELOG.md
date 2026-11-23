# Changelog

All notable changes to **CardputerZero Piano Zero** are documented in this file.

## [0.3.0] - 2026-11-23

### Added
- **Universal Configurable MIDI Learn (`MID` Tab + `L` Key Shortcut)**:
  - Remap any USB MIDI controller's rotary knobs, faders, and buttons to 15 targets (`Master Volume`, `Piano Gain`, `Touch Curve`, `Reverb Send`, `Hammer Boost`, `String Reso`, `Tone Tilt`, `NAV Mode Btn`, `MIDI Play Btn`, `Reverb Toggle`, `Mechanics Btn`, and `NAV` Tab/Row/Val/Preset knobs).
  - Supports both **`LATCH (Toggle)`** (e.g. Alesis V61 illuminated buttons) and **`PUSH (Momentary)`** button behaviors, persists mappings to `~/.config/cardputerzero-pianozero/midi_map.cfg`, and includes **`Reset V61 Map`**.
- **Persistent Last-Loaded Bank & Profile Recall**:
  - Atomically saves `last_bank=<filename>` to `config.cfg` whenever a bank finishes loading (and on exit), plus saves the previous bank's profile before switching so your last instrument and per-bank settings always reload cleanly.

### Fixed
- **Smooth Reverb / FX Fade-Out**: Replaced the hard reverb cut-off at `send == 0` with per-sample smooth ramping of `cur_send_gain_` & `cur_wet_scale_` plus tail-energy tracking, so turning Reverb Send down to zero lets the wet tail decay naturally with zero abrupt drop.

## [0.2.0] - 2026-11-22

### Added
- **In-App Sound Bank Downloader & First-Run Auto-Prompt (`BNK → Download Online Banks`)**:
  - Automatically opens the online bank catalog on first launch when no `.sf2` banks are installed, and remains accessible anytime to add more instruments.
  - Curated 5-bank catalog with instrument type, source/license, download size, DFD RAM footprint, and SD free space check:
    - **Salamander Grand Full** (`695 MB` + `36 MB` mech — Yamaha C5, 14 vel layers)
    - **Salamander Grand Lite** (`237 MB` + `36 MB` mech — Yamaha C5, 6 vel layers)
    - **Nice Steinway v3.8** (`205 MB` — Steinway Concert Grand)
    - **Rhodes EPs Plus v2.3** (`50 MB` — Multi-preset Fender Rhodes MkI/II & Suitcase)
    - **Abbey Steinway D** (`38 MB` — Compact Steinway D starter bank)
  - Resumable `curl` downloads with live neon progress bar (`%`, `MB/MB`, `MB/s`), `Esc` cancel, `RIFF sfbk` validation, atomic install, auto-fetch of `mech.sf2` for Salamander banks, and automatic DFD load on completion.

### Fixed
- **SF2 Loop-Offset Generators (`gen 0, 1, 2, 3, 4, 12, 45, 50`)**: Fixed sustain loop points on multi-sample electric piano banks (eliminates ghost attack repetitions on `Rhodes-EPs-Plus.sf2`).
- **SF2 Per-Voice Lowpass Filter (`gen 8: initialFilterFc`) & DFD Loop Safety**: Added per-voice 1-pole lowpass filter, raised full-RAM threshold to `64 MB`, preloaded loop regions for large DFD banks, and prevented ring-buffer wrap hiss on 17-second Steinway tails.
- **Startup HW System Volume (`70%`)**: Applied hardware/PipeWire unmute & `70%` startup volume *after* `audio_.start()` activates the USB PipeWire sink.

## [0.1.1] - 2026-11-22

### Changed
- **Alesis V61 `K4` (`CC 23`)** in Play mode (`NAV` OFF) now controls **Overall Master Volume (`0%–100%`)** via an instant, zipper-free tapered DSP ramp (also synced with `CC 7` and `AUD → Master Vol (K4)`).
- **Non-blocking `HW System Vol` (`AUD` tab)**: Moved PipeWire `wpctl` & ALSA mixer updates to a coalescing background worker thread with cached reads, eliminating the ~200 ms UI freeze per step.

## [0.1.0] - 2026-11-22

### Added
- **Direct-From-Disk (DFD) SoundFont 2 Streaming Engine (`pz_sf2_dfd`)**:
  - Full RIFF `.sf2` parser supporting preset/instrument global zones, velocity layers, release timecents, and `imod` linear/concave velocity-to-attenuation modulators.
  - Hybrid RAM attack preload (`0.37 s / 0.56 s / 0.74 s`) + lock-free 128-slot background `pread()` ring-buffer streaming from microSD.
  - Verified on CardputerZero (Pi Zero 2 W, 414 MB RAM): loads full **695.2 MB** 14-layer Salamander Grand Piano V3 + **35.9 MB** mechanics in **4.21 s** using **80 MB RSS** with **0 DFD underruns**.
- **Real Piano Mechanics Engine (`pz_mechanics`)**:
  - 1:1 C++17 port of `exxpiano`'s `FXCore` (exponential touch curve, hammer/damper release noise, deferred sympathetic string resonance `ResL`/`ResS`/`ResV3` with `RES_LEVEL` floor & burst limiting, pedal down/up noise, and randomness jitter).
- **Stereo Hall Reverb & Master Bus (`pz_reverb`)**:
  - FluidSynth-compatible Freeverb stereo reverb (`Active`, `Send`, `Room`, `Damp`, `Width`, `Level`), 1 kHz Tilt Tone EQ, and C1 soft-knee stereo bus limiter.
- **Audio & MIDI Subsystem (`pz_audio`, `pz_midi`)**:
  - Multi-device PipeWire/ALSA audio engine with automatic USB audio preference (`PCM2900C` / M-Track Duo), internal ES8389 fallback, live latency/DSP/XRun telemetry, and system mixer volume control.
  - ALSA Sequencer USB MIDI hot-plugging with **Alesis V61 latching toggle button sync (`CC 48–51`)** and **`NAV` mode rotary control (`CC 20–23`)**.
  - Built-in Standard MIDI File (Format 0 & 1) player with half-pedaling (`FILE_PEDAL_DOWN = 40`), `-10s/+10s` seek, repeat, and bundled MAESTRO recordings (*La Campanella*, *Un Sospiro*).
- **320×170 LVGL 9.5 Neon UI (`main.cpp`)**:
  - 7-tab sliding ribbon (`PLY | BNK | TCH | FX | MID | AUD | MON`), live 88-key visualizer, 24-bar oscilloscope, dual L/R VU meters, live touch-curve graph, per-bank profile persistence, and 1 Hz system monitor.
  - Custom DX Zero-family neon launcher icon (`piano-zero.png`, Cobalt Blue → Neon Teal border with glossy ebony `PIANO` + glowing cyan-teal `Zero`).
