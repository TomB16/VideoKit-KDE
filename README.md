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

On bash:
git clone https://github.com/TomB16/VideoKit-KDE.git
cd VideoKit-KDE
makepkg -si



On Arch:

yay -S videokit-kde



## Instructions

Once these scripts and servicemenu are installed, you will be able to right click on a video file and have some options.  Right now, there are four options:  x265 transcode, no default subtitles, file to title, title to file

*****

x265 transcode is the most elaborate part of these scripts, by far.  If you select this, it will create a Transcode directory, a transcode queue, add this file, and launch a transcode process with nice -19.  All output will be in the Transcode directory.  You can add more videos to the queue while it's transcoding.  They will be processed in FIFO.  You will end up with files and a transcode log in ~/Transcode.

To adjust transcode parameters, edit ~/.config/videokit.conf.  I will add more detail on how to configure this over time but it should be reasonably self explanitory.  Notice, you can configure different settings for different quality levels and you can define the quality levels based on the lines of resolution of the source.

Transcode will squeeze the video and copy the audio.  This seems to be a common mode for ffmpeg, based on forum discussion.  If there is need to squeeze audio, it can be easily added.


*****

No default subtitle will strip the "forced" and "default" flags from the video.  Subtitles remain but they will no longer appear by default.  Handbrake has a bug which sets these metadata flags in many cases, regardless of how handbrake is configured.  This bug has been around for many years.  It takes massive amounts of time to turn these flags off using MKVToolNix.  With videokit, you can select a bunch of files, right click...  "strip forced and default subtitles".  Note, it will not modify any file unless it has one of these metadata flags set.


*****

File 2 title will simply assign the filename to the metadata title.  It's a quick way to manipulate a title:  rename the file to something meaningful and then file2title.  My cameras create files with arbitrary names so this makes it quick and easy to set the title.


*****

Title 2 file will rename the file to the metadata title.  I find it handy on rare occasion but this isn't a well used feature for me.


*****

Future development:

- more modules
- better error handling, although it has been stable for me for a few months

This will always be a simple system with a focus on speed of both transcode and UI.  MKVToolNix takes a long time to load and even to unload, on my system.  This is essentially instant.  If you need something elaborate, use MKVToolNix and/or Handbrake.  I still use both of these utilities on rare occasion but they seem slow and bloated now that I'm used to videokit-kde.

<img src="https://github.com/TomB16/VideoKit-KDE/blob/master/Screenshot1.png" width="800" alt="Screenshot">
