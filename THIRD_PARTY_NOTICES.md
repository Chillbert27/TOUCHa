# Third-Party Notices

TOUCHa includes, incorporates, or is derived in part from third-party software.
These components are not relicensed by the proprietary TOUCHa licence. The
applicable notices and licence texts must accompany every binary, Flatpak, APK,
installer, and source distribution.

This inventory is based on the public MetaShare build files and the current
TOUCHa release documentation. It is a release-audit checklist, not a legal
opinion. Record the exact versions and build flags used for every release.

## MetaShare — MIT

Parts of TOUCHa are based on or derived from MetaShare:

- Project: <https://github.com/makemake-kbo/metashare>
- Original licence: <https://github.com/makemake-kbo/metashare/blob/main/LICENSE>

Copyright (c) 2026 makemake

MetaShare is used under the MIT License. The original copyright notice and the
full MIT text must remain with all copies or substantial portions of the
MetaShare-derived code. The MIT License permits commercial use, modification,
distribution, sublicensing, and sale, subject to its conditions.

## FFmpeg — GPL build in the MetaShare Flatpak

- Project: <https://ffmpeg.org/>
- Source version used by the inspected manifest: `n7.1`
- Source: <https://github.com/FFmpeg/FFmpeg/releases/tag/n7.1>
- Legal page: <https://ffmpeg.org/legal.html>
- Licence files: <https://github.com/FFmpeg/FFmpeg/tree/n7.1>

The inspected Flatpak manifest configures FFmpeg with `--enable-gpl` and
`--enable-libx264`. This is a critical compliance issue: the resulting FFmpeg
build must be treated as GPL-covered unless a qualified licence review confirms
otherwise. A proprietary combined distribution cannot simply impose a
proprietary licence over that GPL-covered component. Provide the corresponding
source and GPL notices as required by the GPL, or rebuild without GPL
components and verify the resulting configuration.

## x264 — GPL

- Project: <https://code.videolan.org/videolan/x264>
- Licence information: <https://code.videolan.org/videolan/x264/-/blob/stable/COPYING>
- Source branch used by the inspected manifest: `stable`

The inspected Flatpak build vendors x264 and links FFmpeg against it. Pin an
exact x264 commit for each release and ship the matching source, copyright
notices, and licence text. Because x264 is GPL-licensed and FFmpeg is built
with `--enable-gpl`, obtain a specific GPL distribution review before shipping
any proprietary binary containing this combination.

## sdbus-c++ — Apache-2.0

- Project: <https://github.com/Kistler-Group/sdbus-cpp>
- Version used by the inspected manifest: `v1.5.0`
- Licence: <https://github.com/Kistler-Group/sdbus-cpp/blob/v1.5.0/LICENSE>

Keep the Apache-2.0 licence, copyright notices, and any NOTICE file supplied by
the project. Apache-2.0 permits commercial distribution subject to its terms.

## PipeWire — MIT / LGPL components

- Project: <https://pipewire.org/>
- Source and licences: <https://gitlab.freedesktop.org/pipewire/pipewire>
- Licence directory: <https://gitlab.freedesktop.org/pipewire/pipewire/-/tree/master/LICENSES>

The exact PipeWire libraries and versions used at runtime must be recorded.
Do not assume that one licence applies to every dependency in the PipeWire
stack. Preserve the relevant notices and comply with any LGPL obligations when
redistributing linked libraries.

## GTK, GLib, gtkmm, glibmm, cairomm, pangomm, libsigc++, and mm-common

Projects and licence references:

- GTK: <https://gitlab.gnome.org/GNOME/gtk>
- GTK licence files: <https://gitlab.gnome.org/GNOME/gtk/-/tree/main/LICENSES>
- GLib: <https://gitlab.gnome.org/GNOME/glib>
- gtkmm: <https://gitlab.gnome.org/GNOME/gtkmm>
- glibmm: <https://gitlab.gnome.org/GNOME/glibmm>
- cairomm: <https://gitlab.freedesktop.org/cairo/cairomm>
- pangomm: <https://gitlab.gnome.org/GNOME/pangomm>
- libsigc++: <https://github.com/libsigcplusplus/libsigcplusplus>
- mm-common: <https://gitlab.gnome.org/GNOME/mm-common>

The inspected GUI Flatpak manifest builds these components at pinned versions.
For every release, preserve the exact source commit/tag and include each
project's applicable licence and copyright notices. GTK-family projects often
contain multiple licence files, so do not replace them with one guessed notice.

## SDL2 — zlib licence

- Project: <https://github.com/libsdl-org/SDL>
- Licence: <https://github.com/libsdl-org/SDL/blob/main/LICENSE.txt>

Include the SDL2 licence text if SDL2 is present in the shipped product. If it
is only used by a disabled test client and is not distributed, document that
fact in the release bill of materials.

## libdrm — MIT

- Project: <https://gitlab.freedesktop.org/mesa/drm>
- Licence and notices: <https://gitlab.freedesktop.org/mesa/drm/-/tree/main>

Record the exact libdrm version and retain its licence notices when bundled or
redistributed.

## Opus — BSD-style licence

- Project: <https://github.com/xiph/opus>
- Licence: <https://github.com/xiph/opus/blob/main/COPYING>

The current MetaShare Android documentation refers to Opus RTP payloads, while
the inspected Android build declares no external media dependency. Confirm
whether Opus is decoded by the Android framework, bundled in an APK, or supplied
by another runtime before release. Add the exact applicable notice only after
that determination.

## Qt / PyQt6 — release-specific review required

- Qt licensing: <https://www.qt.io/licensing>
- Qt open-source licences: <https://doc.qt.io/qt-6/qtlicenses.html>
- PyQt licensing: <https://www.riverbankcomputing.com/commercial/pyqt>
- PyQt GPL/commercial information: <https://www.riverbankcomputing.com/software/pyqt/license>

The TOUCHa README says the GUI requires PyQt6. Confirm the exact PyQt6 and Qt
versions, whether the distribution is dynamically linked, and whether your
chosen commercial or open-source licence permits the intended sale. Do not
ship a proprietary PyQt6-based binary commercially until this is confirmed.

## Android SDK, NDK, Gradle, and Android framework

- Android SDK terms: <https://developer.android.com/studio/terms>
- Android SDK licence: <https://developer.android.com/studio/terms>
- Android NDK: <https://developer.android.com/ndk/downloads>
- Gradle licence: <https://gradle.org/license/>
- Android Open Source Project licences: <https://source.android.com/docs/setup/about/licenses>

The Android framework APIs such as `MediaCodec`, `AudioTrack`, `Surface`, and
`java.net` are not automatically evidence of a bundled third-party library.
Audit the final APK with its exact Gradle dependencies, SDK/NDK packages, and
embedded native libraries. Keep required notices and comply with the Meta
Horizon Store and Android distribution terms separately.

## Flatpak runtimes and KDE/GNOME platform components

- Flatpak: <https://github.com/flatpak/flatpak>
- Freedesktop runtimes: <https://gitlab.com/freedesktop-sdk/freedesktop-sdk>
- GNOME runtimes: <https://gitlab.gnome.org/GNOME/gnome-build-meta>
- KDE runtimes: <https://invent.kde.org/packaging/flatpak-kde-runtime>
- Flathub legal information: <https://docs.flathub.org/docs/for-app-authors/requirements>

A Flatpak runtime is normally installed separately by the user and is not the
same as bundling every runtime library into the application. State exactly
which runtime is required and do not claim that its components are proprietary
TOUCHa code. Verify the notices and distribution policy for each runtime used.

## Nixpkgs and build inputs

- Nixpkgs: <https://github.com/NixOS/nixpkgs>
- Nixpkgs licence information: <https://github.com/NixOS/nixpkgs/blob/master/COPYING>

Nix is a build/package source and is not necessarily shipped in the final
product. Separate build-time-only inputs from libraries included in the final
installer or binary and audit only what is actually distributed, while keeping
build reproducibility records.

## Release checklist

For every commercial release:

1. Generate an SBOM and a complete dependency inventory for each artifact.
2. Record exact source versions or immutable commits, not moving branches.
3. Determine whether each library is bundled, statically linked, dynamically
   linked, supplied by a runtime, or only used during development.
4. Include all required copyright, licence, NOTICE, and source/relinking files.
5. For GPL components, perform a separate GPL compliance review before sale.
6. For LGPL components, verify dynamic-linking, relinking, and modification
   obligations.
7. Keep MetaShare's MIT notice in every artifact containing its code.
8. Update this file when v1.2.0 or any later release changes dependencies.

## Experimental v1.2.0

The experimental v1.2.0 release is not a final commercial-licence clearance.
It must retain this notice and all applicable third-party notices. Until the
release-specific dependency audit is complete, v1.2.0 should be distributed
only for testing and evaluation.

This file is a compliance draft and does not make the product legally safe by
itself. Obtain a final review from a lawyer experienced in software and
open-source licensing before commercial distribution.
