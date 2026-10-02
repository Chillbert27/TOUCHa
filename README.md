TOUCHa 1.5.1-beta — Linux host
==============================

You need: a Linux PC (x86_64, Wayland) + a TOUCHa viewer (Quest app from
the Meta Horizon Store, or the device app for phones/tablets — both are
distributed separately, no viewer APK lives here). This download is the
Linux host side only: streamer, control GUI, installer, runtime libs.

What's in this folder
---------------------
  install.sh                   installer (native, or Flatpak)
  toucha-streamer              the streamer, release build, stripped
  toucha_gui.pyc               control GUI, compiled (needs Python 3.14+)
  toucha_icon.png              GUI/start-menu icon
  lib/                         bundled runtime libs (9 files: ffmpeg set,
                               x264, sdbus-c++, pipewire, ei, xkbcommon —
                               deliberately no opus/av1, see below)
  com.toucha.Streamer.flatpak  Flatpak bundle (GUI included, sandboxed)
  SHA256SUMS.txt / .asc        checksums, GPG-signed
  toucha-release.gpg           public release key
  LICENSE, COMMERCIAL-EULA.md, THIRD_PARTY_NOTICES.md

1. Verify the download (public key included):
  gpg --import toucha-release.gpg
  gpg --verify SHA256SUMS.asc
  sha256sum -c SHA256SUMS.txt
All files must report OK / good signature. Key fingerprint:
  DB7A 3F89 6919 DA51 825F 3C59 715A 9113 AF69 D487

2. Install:
   ./install.sh --verify-only     check only, install nothing
   ./install.sh                   install (asks before touching system packages)
   ./install.sh --yes             install without asking (scripts)
   ./install.sh --no-sysdeps      never touch the package manager, only report
   ./install.sh --launch          install and start the GUI
   ./install.sh --flatpak         Flatpak instead of native install
   ./install.sh --uninstall       remove a previous install first (on update)
No root needed for the install itself. Missing system packages (libopus,
PipeWire daemon, PyQt6) are installed only with your consent — never
silently. Without consent the installer prints the exact commands.
Note: the GUI ships compiled (toucha_gui.pyc) and needs Python 3.14+
plus PyQt6; install.sh checks both and points at the Flatpak otherwise.

3. Run headless (example):
   toucha-streamer --source portal --monitors 3 \
       --codec hevc --fps 30 --port 8778 --bitrate 7500 \
       --audio system --audio-codec pcm --restore
The portal asks once for screen sharing and remembers the token
(--restore); without it the dialog returns every start.
Or GUI: open TOUCHaDESKTOP from the start menu and press Start. No
terminal, no flags needed. The streamer log lives in its own Log tab;
the exact start command is shown under Advanced → Command.

Alternative: Flatpak bundle (sandboxed, same GUI via start menu):
  flatpak --user install ./com.toucha.Streamer.flatpak
Shared dependencies (KDE runtime + PyQt) come from Flathub automatically.

4. Connect a viewer: open TOUCHa on the Quest (or the device app), pick
your PC from the host list ("name (ip) — N monitors"). First connect
shows a Trust dialog: compare the SHA-256 fingerprint character by
character with the streamer log, then tap Trust ONCE. It is pinned
from then on. Every monitor shows a real thumbnail — empty cards mean
something is wrong, check the logs first.

System requirements
-------------------
  * PipeWire with a running session bus (screen/audio capture)
  * libopus (loader remainder — installed on demand, see step 2)
  * Python 3.14+ with PyQt6 (GUI only)
  * OpenSSL, X11 (base system), x86_64 Linux
Without the PipeWire daemon the streamer exits with a message (a fresh
PipeWire install may need a re-login).

New in 1.5.1
------------
  * Connect thumbnails work again (monitor/port miscalculation fixed).
  * Audio stutter gone; latency ~300 ms down to ~160 ms; routing and
    stuck-takeover bugs fixed. Sound is plain PCM (48 kHz stereo).
  * Video: HEVC expected, H.264 fallback. Opus/AV1 removed.
  * lib/ now carries PipeWire, libei, xkbcommon (9 files instead of 6).
  * The installer resolves missing system packages itself — only with
    consent (--yes or [j/N]), never silently.

Licensing and commercial distribution
======================================

TOUCHa includes or is derived in part from code from MetaShare:

  https://github.com/makemake-kbo/metashare

MetaShare is distributed under the MIT License, including the copyright
notice for makemake. The original MIT licence text and attribution must be
kept with every distribution containing MetaShare-derived code. The MIT
License permits commercial use, modification, and sale, subject to its
conditions.

TOUCHa's original code, branding, artwork, release configuration, and other
original materials may be distributed under a separate TOUCHa commercial
licence. That licence does not remove or restrict the rights granted by the
MIT License or any other third-party licence. Third-party components remain
under their respective licences and may require additional notices, source
code, or relinking information.

This README is a project notice, not legal advice. Obtain a final review from
a lawyer experienced in software and open-source licensing before commercial
release.
