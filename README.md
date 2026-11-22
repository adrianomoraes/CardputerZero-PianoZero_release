<div align="center">

<img src="assets/piano-zero-icon.png" width="180" height="180" alt="CardputerZero Piano Zero Icon" />

# CardputerZero Piano Zero

**Direct-From-Disk (DFD) Sampled Piano & Electric Piano Workstation for M5Stack CardputerZero (`APPLaunch`)**

*Created by **Adriano Dias Moraes***

[![Latest Release](https://img.shields.io/badge/Release-v0.2.0-00E5B0?style=for-the-badge)](releases/v0.2.0/piano-zero_0.2.0-m5stack1_arm64.deb)
[![Platform](https://img.shields.io/badge/Platform-M5Stack%20CardputerZero%20(ARM64)-2563EB?style=for-the-badge)](#installation)
[![Manual](https://img.shields.io/badge/User%20Manual-MANUAL.md-00D4FF?style=for-the-badge)](MANUAL.md)
[![Changelog](https://img.shields.io/badge/Changelog-CHANGELOG.md-FFB020?style=for-the-badge)](CHANGELOG.md)

</div>

---

## Overview

**CardputerZero Piano Zero** transforms the pocket-sized **M5Stack CardputerZero** (`320×170` IPS display, 56-key matrix keyboard, ARM64 Raspberry Pi Zero 2 W) into a **multi-instrument sampled concert grand & electric piano workstation** inspired by **[exxpiano](https://github.com/exxbrain/exxpiano)**.

Using a custom C++17 **Direct-From-Disk (DFD) SoundFont 2 (`.sf2`) streaming engine**, Piano Zero streams huge multi-velocity acoustic pianos (up to 700 MB+) directly from your microSD card inside **~80 MB total RAM** (and loads smaller electric piano banks like Fender Rhodes `<=64 MB` 100% into RAM), with **zero OOMs, zero swap thrashing, and zero underruns**.

> **Important — SoundFont Banks Are Downloaded Separately:**
> The `.deb` package (`~730 KB`) installs the **Piano Zero** application, icon, and two demo MIDI pieces only — **it does not bundle `.sf2` SoundFont banks**. On first launch (or anytime via **`BNK → Download Online Banks`**), Piano Zero's built-in Wi-Fi downloader lets you choose and install from **5 curated acoustic & electric piano banks** directly onto your microSD card (or copy any custom `.sf2` file via `scp`).

---

## Releases & Downloads

| Version | Release Date | Package (ARM64 Debian) | Checksum | Notes |
|---|---|---|---|---|
| **`v0.2.0`** | 2026-11-22 | [`piano-zero_0.2.0-m5stack1_arm64.deb`](releases/v0.2.0/piano-zero_0.2.0-m5stack1_arm64.deb) | [`SHA256SUMS.txt`](releases/v0.2.0/SHA256SUMS.txt) | **In-App Sound Bank Downloader & First-Run Auto-Prompt** (5 curated acoustic & electric piano banks), SF2 loop-offset & per-voice lowpass filter fixes (clean Rhodes loops & Steinway tails), 70% startup HW volume — see [CHANGELOG.md](CHANGELOG.md) |
| **`v0.1.1`** | 2026-11-22 | [`piano-zero_0.1.1-m5stack1_arm64.deb`](releases/v0.1.1/piano-zero_0.1.1-m5stack1_arm64.deb) | [`SHA256SUMS.txt`](releases/v0.1.1/SHA256SUMS.txt) | Initial public release — 700 MB DFD SF2 streaming engine, `exxpiano` real piano mechanics, Freeverb Hall Reverb, SMF MIDI player (*La Campanella* & *Un Sospiro*), instant DSP Master Volume on V61 `K4`, latching V61 toggle buttons (`CC 48–51`), fluid non-blocking `AUD` system volume & neon B2 icon — see [CHANGELOG.md](CHANGELOG.md) |

---

## Installation

No compiler or build tools required. Install the prebuilt ARM64 `.deb` package directly on your M5Stack CardputerZero:

### Option 1: Install Directly on CardputerZero
Download the `.deb` package to your CardputerZero and run:
```bash
sudo apt install -y ./piano-zero_0.2.0-m5stack1_arm64.deb
```

### Option 2: Install Remotely Over SSH (from macOS / Linux / Windows)
```bash
scp releases/v0.2.0/piano-zero_0.2.0-m5stack1_arm64.deb <user>@<cardputer-ip>:/tmp/
ssh <user>@<cardputer-ip> "sudo apt install -y /tmp/piano-zero_0.2.0-m5stack1_arm64.deb"
```

After installation, **Piano Zero** appears automatically in the CardputerZero **`APPLaunch`** launcher with its neon icon.

---

## Downloading SoundFont Banks (First Launch & Anytime)

The `.deb` package does **not** include `.sf2` instrument banks so the installer stays tiny (`~730 KB`) and you can pick the instruments that fit your microSD card and style.

1. **First Launch**: When you open **Piano Zero** with no `.sf2` banks installed, it automatically opens **`BNK → Download Online Banks`** over Wi-Fi.
2. **Add More Anytime**: Even after installing one or more banks, go to **`BNK → Download Online Banks`** anytime to browse and download additional instruments:

| Bank | Instrument Type | Download Size | DFD RAM | Source / License |
|---|---|---|---|---|
| **Salamander Grand Full** | Yamaha C5 Concert Grand (14 vel layers, 12 s tails) | `695 MB` *(+36 MB mech)* | `~40 MB` | FreePats / Alexander Holm (CC-BY 3.0) |
| **Salamander Grand Lite** | Yamaha C5 Concert Grand (6 vel layers, 9 s tails) | `237 MB` *(+36 MB mech)* | `~20 MB` | FreePats / Alexander Holm (CC-BY 3.0) |
| **Nice Steinway v3.8** | Steinway Concert Grand | `205 MB` | `~22 MB` | `soundfonts4u` / Hugging Face |
| **Rhodes EPs Plus v2.3** | Multi-preset Fender Rhodes MkI/II & Suitcase | `50 MB` | `~50 MB` *(full RAM)* | `soundfonts4u` / Hugging Face |
| **Abbey Steinway D** | Compact Steinway D Grand (quick starter) | `38 MB` | `~38 MB` *(full RAM)* | `soundfonts4u` / Hugging Face |

3. **Custom `.sf2` Banks**: You can also copy any standard `.sf2` SoundFont via `scp` into `~/.config/cardputerzero-pianozero/banks/` (or `/usr/share/APPLaunch/share/piano-banks/`) and press **Rescan SD Banks** in the `BNK` tab.

### Verify Package Integrity (Optional)
```bash
cd releases/v0.2.0
sha256sum -c SHA256SUMS.txt
```

### Uninstall
```bash
sudo apt remove piano-zero
```
*(Your downloaded `.sf2` banks, `.mid` files, and per-bank profiles in `~/.config/cardputerzero-pianozero/` are preserved.)*

---

## Key Features

- **Direct-From-Disk (DFD) Multi-Instrument SF2 Engine**: Streams 200–700+ MB acoustic grand pianos lock-free from microSD (`~40 MB` RAM attack cache, 0 underruns), loads `<=64 MB` banks (e.g. Fender Rhodes) 100% into RAM, supports SF2 loop-offset generators (`gen 0–4, 12, 45, 50`) and per-voice lowpass filtering (`gen 8`), and remembers your last-loaded bank across restarts.
- **Built-In Wi-Fi Bank Downloader**: First-run auto-prompt + anytime online catalog with instrument details, size/SD space check, live progress bar (`%`, `MB/MB`, `MB/s`), `Esc` cancel, `RIFF sfbk` verification, and automatic `mech.sf2` install for Salamander banks.
- **Real Piano Mechanics (`exxpiano FXCore`)**: Hammer/damper release noise, deferred sympathetic string resonance (`ResL`/`ResS`/`ResV3`) with voice-budget floor & burst limiting, sustain pedal down/up noise, randomness jitter, and live exponential touch curve (`0.40–2.50`) — saved per bank so electric pianos can keep mechanics OFF while acoustic grands keep them ON.
- **Studio Hall Reverb & Tone (`FX` Tab)**: Freeverb stereo reverb (`Active`, `Send 0–127`, `Room`, `Damp`, `Width`, `Level`), 1 kHz Tilt Tone EQ (`-6..+6 dB`), and C1 soft-knee stereo bus limiter.
- **Built-in SMF 0/1 MIDI Player (`MID` Tab)**: Ships with MAESTRO concert recordings (*La Campanella* & *Un Sospiro*) played live through your active instrument with half-pedaling, `-10s/+10s` seek, repeat, and play-along support.
- **USB Audio & USB MIDI Hot-Plug**: Auto-prefers external USB audio interfaces (e.g., M-Audio M-Track Duo / `PCM2900C`) with `70%` startup HW volume and live switching to internal ES8389 audio. **Any class-compliant USB MIDI keyboard & sustain pedal (`CC 64`/`CC 7`) works out-of-the-box**; hardware knob/button defaults (`CC 20–23`, `CC 48–51`, `NAV` mode) are currently tested and pre-configured for the **Alesis V61** *(configurable MIDI Learn for other controllers is planned)*.
- **7-Tab Neon `320×170` LVGL 9.5 UI**: `PLY | BNK | TCH | FX | MID | AUD | MON` with live 88-key visualizer, 24-bar oscilloscope, dual L/R VU meters, live touch-curve graph, and 1 Hz CPU/Temp/RAM/DFD monitor.

---

## License & Acknowledgments

- **Piano Zero**: MIT License ([`LICENSE`](LICENSE)).
- **Piano mechanics & SF2 builder**: [exxpiano](https://github.com/exxbrain/exxpiano) (MIT).
- **Salamander Grand Piano V3**: Alexander Holm ([CC-BY 3.0](https://creativecommons.org/licenses/by/3.0/)).
- **Steinway & Rhodes SF2 banks**: Hosted via `projectlosangeles/soundfonts4u` on Hugging Face.
- **Demo MIDI recordings**: MAESTRO dataset v3.0.0 ([CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)).
