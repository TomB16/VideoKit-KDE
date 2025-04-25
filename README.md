# VideoKit-KDE

**VideoKit-KDE** is a lightweight video utility toolkit designed to extend the functionality of the Dolphin file manager in KDE. It provides quick access to common video operations like transcoding, renaming, and subtitle management via right-click context menu entries.

> ⚙️ 100% BASH-based  
> 🧠 Built for KDE and Dolphin  
> 📽️ Uses powerful tools like `ffmpeg` and `mediainfo`  
> 🔓 Licensed under MIT

---

## ✨ Features

- Rename files based on embedded title metadata
- Rename embedded titles to match filenames
- Convert duration to HH:MM:SS format
- Queue and process video transcodes
- Filter out forced subtitle tracks
- Run with `systemd-inhibit` to prevent sleep during processing
- GUI-driven via Dolphin context menus
- Designed to run at **user level** — no root access needed to use

---

## 📦 Installation

This package is available as an [AUR package](https://aur.archlinux.org/) for Arch Linux-based systems.

To install manually:

```bash
git clone https://github.com/TomB16/VideoKit-KDE.git
cd VideoKit-KDE
makepkg -si
