<div align="center">

<img src="assets/piano-zero-icon.png" width="180" height="180" alt="CardputerZero Piano Zero Icon" />

# CardputerZero Piano Zero

**Direct-From-Disk (DFD) Sampled Concert Grand Piano Workstation for M5Stack CardputerZero (`APPLaunch`)**

*Created by **Adriano Dias Moraes***

[![Latest Release](https://img.shields.io/badge/Release-v0.1.1-00E5B0?style=for-the-badge)](releases/v0.1.1/piano-zero_0.1.1-m5stack1_arm64.deb)
[![Platform](https://img.shields.io/badge/Platform-M5Stack%20CardputerZero%20(ARM64)-2563EB?style=for-the-badge)](#installation)
[![Manual](https://img.shields.io/badge/User%20Manual-MANUAL.md-00D4FF?style=for-the-badge)](MANUAL.md)
[![Changelog](https://img.shields.io/badge/Changelog-CHANGELOG.md-FFB020?style=for-the-badge)](CHANGELOG.md)

</div>

---

## Overview

**CardputerZero Piano Zero** transforms the pocket-sized **M5Stack CardputerZero** (`320×170` IPS display, 56-key matrix keyboard, ARM64 Raspberry Pi Zero 2 W) into a **studio-grade sampled concert grand piano** inspired by **[exxpiano](https://github.com/exxbrain/exxpiano)**.

Using a custom C++17 **Direct-From-Disk (DFD) SoundFont 2 streaming engine**, Piano Zero streams the full **695 MB 14-velocity-layer Salamander Grand Piano V3** (`44.1 kHz / 16-bit` stereo, 12-second tails) directly from your microSD card inside **~80 MB total RAM** (only 39 MB for all 840 attack transients + 36 MB for real piano mechanics), with **zero OOMs, zero swap thrashing, and zero underruns**.

Plug in **any class-compliant USB MIDI keyboard** and **USB audio interface** (or use the CardputerZero's built-in speaker/headphones and `Q..I` mini-piano) and play immediately.

---

## Releases & Downloads

| Version | Release Date | Package (ARM64 Debian) | Checksum | Notes |
|---|---|---|---|---|
| **`v0.1.1`** | 2026-11-22 | [`piano-zero_0.1.1-m5stack1_arm64.deb`](releases/v0.1.1/piano-zero_0.1.1-m5stack1_arm64.deb) | [`SHA256SUMS.txt`](releases/v0.1.1/SHA256SUMS.txt) | Initial public release — 700 MB DFD SF2 streaming engine, `exxpiano` real piano mechanics, Freeverb Hall Reverb, SMF MIDI player (*La Campanella* & *Un Sospiro*), instant DSP Master Volume on V61 `K4`, latching V61 toggle buttons (`CC 48–51`), fluid non-blocking `AUD` system volume & neon B2 icon — see [CHANGELOG.md](CHANGELOG.md) |

---

## Installation

No compiler or build tools required. Install the prebuilt ARM64 `.deb` package directly on your M5Stack CardputerZero:

### Option 1: Install Directly on CardputerZero
Download the `.deb` package to your CardputerZero and run:
```bash
sudo apt install -y ./piano-zero_0.1.1-m5stack1_arm64.deb
```

### Option 2: Install Remotely Over SSH (from macOS / Linux / Windows)
```bash
scp releases/v0.1.1/piano-zero_0.1.1-m5stack1_arm64.deb <user>@<cardputer-ip>:/tmp/
ssh <user>@<cardputer-ip> "sudo apt install -y /tmp/piano-zero_0.1.1-m5stack1_arm64.deb"
```

After installation, **Piano Zero** appears automatically in the CardputerZero **`APPLaunch`** launcher with its neon icon.

### SoundFont Banks Setup (Required)
Because the Salamander Grand Piano V3 SoundFonts are large (~700 MB) and licensed CC-BY, they are installed separately on your microSD card:

1. Build `piano-full.sf2`, `piano-lite.sf2`, and `mech.sf2` on macOS/Linux using [exxpiano](https://github.com/exxbrain/exxpiano)'s `scripts/make-soundfonts.sh -p full` (or drop in any standard `.sf2` piano SoundFont!).
2. Copy the `.sf2` files onto your CardputerZero:
   ```bash
   scp piano-full.sf2 mech.sf2 <user>@<cardputer-ip>:~/.config/cardputerzero-pianozero/banks/
   ```
   *(Or install system-wide into `/usr/share/APPLaunch/share/piano-banks/`.)*

### Verify Package Integrity (Optional)
```bash
cd releases/v0.1.1
sha256sum -c SHA256SUMS.txt
```

### Uninstall
```bash
sudo apt remove piano-zero
```
*(Your `.sf2` banks, `.mid` files, and per-bank profiles in `~/.config/cardputerzero-pianozero/` are preserved.)*

---

## Key Features

- **Direct-From-Disk (DFD) 700 MB SF2 Streaming**: Preloads `0.56 s` attack transients in RAM (~39 MB) and streams 12-second stereo tails lock-free from microSD via `pread()` (`~80 MB` total RSS on 414 MB Pi Zero 2 W, loads in ~4.2 s, 0 underruns).
- **Real Piano Mechanics (`exxpiano FXCore`)**: Hammer/damper release noise, deferred sympathetic string resonance (`ResL`/`ResS`/`ResV3`) with voice-budget floor & burst limiting, sustain pedal down/up noise, randomness jitter, and live exponential touch curve (`0.40–2.50`).
- **Studio Hall Reverb & Tone (`FX` Tab)**: Freeverb stereo reverb (`Active`, `Send 0–127`, `Room`, `Damp`, `Width`, `Level`), 1 kHz Tilt Tone EQ (`-6..+6 dB`), and C1 soft-knee stereo bus limiter.
- **Built-in SMF 0/1 MIDI Player (`MID` Tab)**: Ships with MAESTRO concert recordings (*La Campanella* & *Un Sospiro*) played live through the piano + mechanics engine with half-pedaling, `-10s/+10s` seek, repeat, and play-along support.
- **USB Audio & USB MIDI Hot-Plug**: Auto-prefers external USB audio interfaces (e.g., M-Audio M-Track Duo / `PCM2900C`) with live switching to internal ES8389 audio; supports Alesis V61 latching toggle buttons (`CC 48–51`), instant DSP Master Volume (`K4` / `CC 23`), and `NAV` UI mode (`CC 20–23`).
- **7-Tab Neon `320×170` LVGL 9.5 UI**: `PLY | BNK | TCH | FX | MID | AUD | MON` with live 88-key visualizer, 24-bar oscilloscope, dual L/R VU meters, live touch-curve graph, and 1 Hz CPU/Temp/RAM/DFD monitor.

---

## License & Acknowledgments

- **Piano Zero**: MIT License ([`LICENSE`](LICENSE)).
- **Piano mechanics & SF2 builder**: [exxpiano](https://github.com/exxbrain/exxpiano) (MIT).
- **Salamander Grand Piano V3**: Alexander Holm ([CC-BY 3.0](https://creativecommons.org/licenses/by/3.0/)).
- **Demo MIDI recordings**: MAESTRO dataset v3.0.0 ([CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)).
