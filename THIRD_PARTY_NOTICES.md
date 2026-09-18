# Third-Party Notices

TOUCHa includes, incorporates, or is derived in part from third-party software.
Those components are not relicensed by the proprietary TOUCHa licence.
Customers must receive the applicable notices and licence texts with every
binary, Flatpak, APK, installer, or source distribution.

## MetaShare

Parts of TOUCHa are based on or derived from MetaShare:

<https://github.com/makemake-kbo/metashare>

Copyright (c) 2026 makemake

MetaShare is used under the MIT License:

```text
MIT License

Copyright (c) 2026 makemake

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Components requiring release-specific verification

The exact versions, build options, linking method, and distribution terms must
be recorded for every release. The current project documentation identifies or
mentions components including:

- FFmpeg/libav*;
- Opus;
- PipeWire;
- GTK/gtkmm;
- SDL2;
- libdrm;
- sdbus-cpp and XDG desktop portals;
- Qt/PyQt6, where used by the TOUCHa GUI;
- Android SDK, NDK, Gradle, and Android framework components; and
- KDE/Flatpak runtime components.

Before selling a release, generate a complete dependency inventory and add the
corresponding copyright and licence texts here. Do not label a release as
"fully proprietary" when it contains third-party code.

## Experimental v1.2.0

The experimental v1.2.0 release is not a final commercial-licence clearance.
It must retain this notice and all applicable third-party notices. Until the
release-specific dependency audit is complete, v1.2.0 should be distributed
only for testing and evaluation.

This file is a compliance draft, not legal advice.
