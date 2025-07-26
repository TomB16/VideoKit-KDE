# VideoKit-KDE

**VideoKit-KDE** is a lightweight video utility toolkit designed to extend the functionality of the Dolphin file manager in KDE. It provides quick access to common video operations like transcoding, renaming, and subtitle management via right-click context menu entries.

> ⚙️ 100% BASH-based  
> 🧠 Built for KDE and Dolphin  
> 📽️ Uses powerful tools like `ffmpeg` and `mediainfo`  
> 🔓 Licensed under MIT

Special thanks to ChatGPT by OpenAI for providing helpful guidance and brainstorming support throughout the development of VideoKit-KDE.

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

### Manual installation

In Bash shell:

```bash
git clone https://github.com/TomB16/videokit-kde.git
cd videokit-kde


### On Arch variants

yay -S videokit-kde



## Instructions

With these scripts and servicemenu, you will be able to right click on a video file and have a menu of options.  There are four options:  x265 transcode, , x265 transcode next, no default subtitles, file to title, title to file

*****

The transcoding feature is the primary purpose of this system.  Files and groups of files can be selected, rightg click, and manipulated directly.  If one of the transcode options are selected, the item(s) are queued and a background process is launched to transcode the media.  The background process runs at `nice -19` to maintain system responsiveness. Output and logs are stored in `~/Transcode`. You can add more files while transcoding; they will be processed in FIFO order when transcode is selected.  This is a very light weight system.  Performance is extreme.  I regularly get 100fps and higher with 1080p content (Note:  I don't compress audio but you can by maniuplating the ffmpeg options).

"Queue for next transcode" works to add a file to be transcoded next.  If the file is already in the queue, it is removed from it's current position and added into the next transcode position.  Notice, the top transcode position is the file currently being transcoded by the queue processor so next position is the maximum expedite possible.  Feel free to highlight an entire directory of videos and add them to transcode next.  

The transcode log includes CPU frequency, CPU temperature, GPU Frequency, GPU Temperature, and ambient temperature.  Ambient temperature requires a TEMPer USB sensor.  This system is quite useful for benchmarking or testing system cooling under load.

To adjust transcode parameters, edit `~/.config/videokit.conf`. Different quality levels can be configured based on the source resolution.

Transcoding squeezes the video and copies the audio by default. Audio transcoding can be added by changing ffmpeg parameters in the config, as needed.


*****

videokit-noforcedsubs

Strips "forced" and "default" subtitle flags while keeping subtitle tracks intact. Useful because Handbrake often sets these flags incorrectly, and MKVToolNix is slow for this task.


*****

videokit-file2title

Sets the embedded metadata title to the filename — handy for giving meaningful titles to camera files with arbitrary names.


*****

videokit-title2file

Renames the file to match the embedded metadata title. Less frequently used but occasionally handy.

*****

videokit-util

videokit-util is the central CLI utility for the VideoKit-KDE suite. It consolidates multiple small utility scripts into a single, modular tool with a clean command-line interface. This architecture improves usability, maintainability, and consistency across the toolkit.
Features

    ✅ Preflight system checks: --preflight

    ✅ Version reporting: --version

    ✅ Process lock reset: --reset

    ✅ Log anonymization: --anon

    ✅ Log rotation support: --log-rotate [monthly|weekly|daily]

    ✅ Log restoration (unrotate): --log-unrotate <filename>

    ✅ Transcode queueing: --queue <video-file>
    

# Run preflight checks
videokit-util --preflight

# Rotate transcode log using weekly rotation policy
videokit-util --log-rotate weekly

# Extract a rotated archive log
videokit-util --log-unrotate ~/Transcode/Archive/transcode.weekly.202530.log.gz

# Queue a video for background transcode
videokit-util --queue my-large-video.mp4

# Report the installed version
videokit-util --version

# This will anonymize transcode.log with the result in transcode-anon.log
videokit-util --anon


*****

📊 Transcode Spreadsheet

A spreadsheet is included in the Transcode/ directory to help track and audit your transcode activity over time. This file is automatically updated by the system and includes detailed log entries such as:

    Input video filename

    Output format and resolution

    Codec used

    Transcode duration

    CPU/GPU utilization data (if available)

    Timestamps and queue metadata

🔍 Purpose

The spreadsheet provides:

    A high-level overview of what has been transcoded

    A reference for past work when debugging or comparing results

    A lightweight alternative to parsing raw logs for common statistics

📁 File Location

Transcode/Transcode Log Analysis v0.01.ods

This is a LibreOffice-compatible .ods file, but can also be opened with Excel or other spreadsheet tools that support OpenDocument formats.

*****

~/.config/videokit.conf - contains the transcode and other configs.

FFMPEG_PROCESSOR=CPU
FFMPEG_PARAMS_CPU='-rtbufsize 2G -c:v libx265 -x265-params asm=avx512 -c:a copy'
FFMPEG_PARAMS_GPU='-c:v hevc_amf -cq 23 -c:a copy'
#FFMPEG_PARAMS_GPU='-c:v hevc_nvenc -cq 23 -c:a copy'


Here are some audio codec ffmpeg parameters.

-c:a copy                       # copy audio unchanged

-acodec aac -b:a 128k           # AAC

-acodec libmp3lame -b:a 128k    # MP3

-acodec libopus -b:a 96k        # Opus


*****

Power monitoring.

Power monitoring requires the presence and user level access to RAPL (Running Average Power Limit) interfaces exposed by the kernel under /sys/class/powercap for Intel or AMD CPUs.  By default, this access is not available.  Interfaces can be exposed by creating a udev rule like or similar to this (depending on your system):

/etc/udev/99-rapl.rules
{
    # /etc/udev/rules.d/99-rapl.rules
    SUBSYSTEM=="powercap", KERNEL=="intel-rapl:0", RUN+="/bin/chmod o+r /sys/class/powercap/%k/energy_uj"
}

run these commands:

sudo udevadm control --reload-rules
sudo udevadm trigger


As well as access to RAPL, you will need to install the powerstat utility.

yay -S powerstat


*****

error: videokit-transcodeprocess is already running

Response: run "videokit-util --reset"

Problem: The transcode queue processor previously terminated ungracefully.  The current instance can see PID and lock files from the previous instance.


*****

Future development:

This will always be a simple system with a focus on speed of both transcode and UI.  MKVToolNix takes a long time to load and even to unload, on my system.  This is essentially instant.  If you need something elaborate, use MKVToolNix and/or Handbrake.  I still use both of these utilities on rare occasion but they seem slow and bloated now that I'm used to videokit-kde.

<img src="https://github.com/TomB16/VideoKit-KDE/blob/master/Screenshot1.png" width="800" alt="Screenshot">
