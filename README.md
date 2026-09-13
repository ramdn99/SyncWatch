# SyncWatch — Real-Time Synchronized Torrent & Local Video Player

![SyncWatch Banner](public/SyncWatch-banner.jpg)

[![Platform](https://img.shields.io/badge/Platform-Windows%20(Portable%20%7C%20MSI)-blue)](https://github.com/zeroXmoRamadan/SyncWatch)
[![Electron](https://img.shields.io/badge/Electron-37-47848F?logo=electron&logoColor=white)](https://electronjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green?logo=node.js&logoColor=white)](https://nodejs.org/)
[![WebTorrent](https://img.shields.io/badge/WebTorrent-1.9-red)](https://webtorrent.io/)
[![FFmpeg](https://img.shields.io/badge/FFmpeg-Static-007808?logo=ffmpeg&logoColor=white)](https://ffmpeg.org/)
[![PeerJS](https://img.shields.io/badge/PeerJS-WebRTC-orange)](https://peerjs.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

> [!WARNING]
> **Deprecation Notice: Web Browser Version**
> 
> The browser-only web version of SyncWatch is officially deprecated and unsupported. Standard web browsers cannot natively decode non-web formats (such as MKV containers, AC-3 / E-AC-3 / DTS audio, and HEVC video) and lack local transcode toolchain access.
> 
> SyncWatch is now exclusively maintained and distributed as a native desktop application, integrating an embedded local streaming engine, hardware-assisted FFmpeg pipelines, and WebTorrent directly.

---

## Overview

SyncWatch is an open-source, peer-to-peer desktop media synchronization platform. It enables groups of users to watch video content in frame-accurate synchronization (playback, pause, seek, and subtitles) directly over WebRTC without routing video data through a central relay server.

Whether streaming a BitTorrent magnet link during download or synchronizing a shared local file, SyncWatch automatically handles container remuxing, audio transcoding, and network traversal.

---

## Key Features

### Native Desktop Distribution
- **Standalone Portable Executable (`SyncWatch.exe`)**: Single-file executable with zero installation footprint. Bundles Node.js, WebTorrent, and statically linked FFmpeg.
- **Windows Installer (`SyncWatch-Setup.msi`)**: Standard Windows Installer (MSI) with setup wizard, Start Menu & Desktop shortcuts, and Windows Programs & Features integration.
- **Frameless Desktop Window**: Borderless application window without default system menu bars.
- **High-Definition Multi-Resolution Branding**: 7-tier uncompressed icon assets (16x16 to 256x256, derived from a 1080x1080 master asset) optimized for all Windows Explorer layout modes.

### Media Engine & Transcoding Pipeline
- **Universal Container Support**: Native compatibility with MKV, MP4, WebM, and MOV containers.
- **3-Stage Adaptive Conversion Pipeline**:
  - **Stage 1 (Direct Play)**: Codec-compatible containers (H.264/AAC MP4 or VP8/Vorbis WebM) stream immediately with zero CPU transcode overhead.
  - **Stage 2 (Fast Remux)**: Containers with compatible video but unsupported audio codecs (E-AC-3, AC-3, DTS, TrueHD) undergo high-speed container remuxing (`-c:v copy -c:a aac`) in seconds.
  - **Stage 3 (Full Transcode)**: Incompatible video streams (HEVC/H.265, VP9, etc.) are converted to browser-safe H.264 using adaptive profile and level selection.
- **Growing-File Streaming**: Piped MediaSource Extensions (MSE) and fragmented container delivery allow playback to begin within seconds of download or conversion startup.

### Room Scalability & Synchronization
- **Configurable Room Capacity**: Supports room sizes of up to 12 Members (Large Group) and 16 Members (Party), alongside standard capacities (2 to 8 members).
- **Dynamic State Handshake**: New peers automatically receive the active playback position, playback rate, subtitle state, and room capacity limits upon joining.
- **Sub-Second P2P Synchronization**: Frame-accurate play, pause, and seek event propagation over WebRTC data channels via PeerJS.

### Interface & Design System
- **App-Themed Scrollbars**: Global custom scrollbars matching the obsidian palette (`#0b0e12`, `#2e3d50`, `#e8a33d`) across all panels, modal dialogs, and chat.
- **Balanced Landing Layout**: Standardized 540px fixed-width room creation card with responsive mobile collapse.
- **3-Column Centered Stage**: Dedicated panels for Room Chat (310px), Central Video Stage, and Host/Member Controls (320px).
- **Inactivity Auto-Fade**: Player interface controls and mouse cursor automatically conceal after 2.5 seconds of user inactivity during playback.
- **Multilingual Refresh Guard**: Physical hardware keycode detection prevents accidental page reloads (`F5`, `Ctrl+R`, `Cmd+R`) across all international keyboard layouts.

### Lifecycle & Resource Management
- **Deterministic Teardown**: Active FFmpeg processes and WebTorrent swarms are immediately terminated when leaving a room, switching media, closing the window, or upon peer disconnect.
- **Automatic Seed Termination**: WebTorrent processes cease seeding upon reaching 100% download completion, shifting to local disk streaming to eliminate background upload consumption.
- **Automatic Cache Purge**: Temporary transcoding artifacts in `%TEMP%\SyncWatch` are cleared on application startup and exit.

---

## Installation & Deployment

### Portable Executable
1. Download `SyncWatch.exe` from the latest release or from `SyncWatch App/`.
2. Launch `SyncWatch.exe`. The application operates portably; downloads are maintained in the adjacent `SyncWatch Downloads/` directory.

### Windows Installer (MSI)
1. Download `SyncWatch-Setup.msi` from the releases page or from `SyncWatch App/`.
2. Execute the installer and follow the setup wizard.
3. Launch SyncWatch via the Start Menu or Desktop shortcut.

---

## Operating Instructions

### Method A: Stream via Torrent Magnet
1. On the landing page, select **Stream Torrent**.
2. Paste the **Magnet URI** of the media file.
3. Configure the maximum member limit (up to 16 members) and select **Create Room**.
4. Distribute the generated 6-character Room Code to participants.
5. Playback initializes once initial metadata and buffer thresholds are met.

### Method B: Synchronize Local Media File
For participants possessing identical local video copies:
1. Select **Local Video File** on the landing page.
2. Select the local video file (`.mp4`, `.webm`, or `.mkv`).
3. Select **Create Room** and provide the Room Code to participants.
4. Joining participants are prompted to select their matching local copy.
5. Playback synchronizes across all peers without consuming network bandwidth for media delivery.

### Host Controls: Subtitles
- **External Subtitles**: Hosts can load external `.srt` or `.vtt` subtitle files; tracks are broadcast and rendered synchronously across all member clients.

---

## Developer Guide

### Environment Requirements
- **Node.js** 18 or higher ([nodejs.org](https://nodejs.org/))
- **npm** 9 or higher
- **Windows OS** (required for compiling `.exe` and `.msi` targets)

### Local Development Setup

```bash
# Clone the repository
git clone https://github.com/zeroXmoRamadan/SyncWatch.git
cd SyncWatch

# Install dependencies
npm install

# Run the desktop application in development mode
npm run desktop
```

### Build Commands

```bash
# Compile standalone portable executable (SyncWatch.exe)
npm run package

# Compile Windows Installer package (SyncWatch-Setup.msi)
npm run package:msi

# Compile both distribution targets
npm run package:all
```

Compiled deliverables are located in `../SyncWatch App/`.

---

## Repository Structure

```
SyncWatch/
├── main.js                  # Electron main process (window lifecycle, IPC handlers)
├── preload.js               # Electron preload script (context-isolated bridges)
├── server.js                # Express streaming backend, FFmpeg pipeline, & WebTorrent
├── package.json             # Application metadata, scripts, and electron-builder configuration
├── build/                   # Installer assets and source icon definitions
│   ├── icon.ico             # Multi-resolution Windows icon (16x16 through 256x256)
│   └── icon.png             # 1080x1080 master branding asset
├── public/                  # Application frontend assets
│   ├── index.html           # Document structure, modal dialogs, layout templates
│   ├── style.css            # Design tokens, custom scrollbars, component styles
│   ├── app.js               # WebRTC peer coordination, player sync, DOM controllers
│   └── SyncWatch-logo-icon.png
└── downloads/               # Local download cache in development environment
```

---

## Network Architecture & NAT Traversal

SyncWatch relies on direct peer-to-peer data channels via PeerJS and WebRTC.

- **STUN**: Public Google STUN servers are queried by default to resolve external IP mappings for standard NAT environments.
- **TURN**: For environments behind symmetric NATs, carrier-grade NAT (CGNAT), or corporate firewalls, a TURN relay server is necessary.
- Custom ICE servers (such as [Metered.ca](https://www.metered.ca/) or private COTURN instances) can be configured via the **Advanced WebRTC Settings** panel on the landing page:

```json
[
  { "urls": "stun:stun.relay.metered.ca:80" },
  {
    "urls": "turn:global.relay.metered.ca:80",
    "username": "your-username",
    "credential": "your-password"
  }
]
```

---

## License

This project is licensed under the **MIT License**. Refer to the [LICENSE](LICENSE) file for terms.

---

**Author:** Mohamed Ramadan
