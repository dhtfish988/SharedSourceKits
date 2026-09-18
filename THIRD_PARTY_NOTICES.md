# Third-Party Notices

MGExploitation is integration code built around existing open-source kernel
research components. Those components were written by other people, keep their
original copyright holders, and are redistributed here under their own licenses.
This file lists what is bundled, where it came from and which license applies.

The repository's own integration and build code is covered by [LICENSE](LICENSE)
(MIT). That license covers only that integration and build code — it does not cover
the bundled upstream components listed below, which keep their own copyright holders
and license terms. Nothing in this file is claimed as original work of this
repository.

## Bundled components

| Component | Paths in this repository | Original author | Upstream | License |
| --- | --- | --- | --- | --- |
| kfd | `MGExploitation/Sources/kfd/Exploit/` | Félix Poulin-Bélanger | [felix-pb/kfd](https://github.com/felix-pb/kfd) | MIT |
| weightBufs | IOSurface kread/kwrite in `MGExploitation/Sources/kfd/Exploit/libkfd/krkw/` | 0x36 | [0x36/weightBufs](https://github.com/0x36/weightBufs) | MIT |
| Dopamine | `MGExploitation/Sources/kfd/kfd.h`, `kfd.m`, `MGExploitation/Sources/dmaFail/`, most of `MGExploitation/Sources/KernelSupport/` | Lars Fröder (opa334) and Dopamine contributors | [opa334/Dopamine](https://github.com/opa334/Dopamine) | MIT |
| TrollInstallerX | `MGExploitation/Sources/KernelSupport/vnode.h`, `vnode.m` | Alfie CG (alfiecg24) | [alfiecg24/TrollInstallerX](https://github.com/alfiecg24/TrollInstallerX) | MIT |
| XPF | `MGExploitation/ThirdParty/include/xpf/` | opa334 | [opa334/XPF](https://github.com/opa334/XPF) | MIT |
| golb | `MGExploitation/ThirdParty/include/xpf/decompress.c` | 0x7ff | [0x7ff/golb](https://github.com/0x7ff/golb) | Apache-2.0 |
| Choma | `MGExploitation/ThirdParty/include/choma/`, `MGExploitation/ThirdParty/lib/libchoma.a` | opa334 | [opa334/Choma](https://github.com/opa334/Choma) | MIT |
| libgrabkernel2 | `MGExploitation/ThirdParty/include/libgrabkernel2/`, `MGExploitation/ThirdParty/lib/libgrabkernel2.a` | Alfie CG (alfiecg24) | [alfiecg24/libgrabkernel2](https://github.com/alfiecg24/libgrabkernel2) | MIT |
| libarchive | `MGExploitation/ThirdParty/include/libarchive/` | Tim Kientzle, Martin Matuska and contributors | [libarchive/libarchive](https://github.com/libarchive/libarchive) | BSD-2-Clause |
| Apple dyld formats | `dyld_cache_format.h`, `fixup-chains.h`, `CachePatching.h` inside `MGExploitation/ThirdParty/include/choma/` | Apple Inc. | Apple dyld | APSL-2.0 |

Notes on this table:

- `Sources/KernelSupport/` was named `Sources/libjailbreak/` upstream in Dopamine.
  It was renamed in this repository; the code is still Dopamine-derived.
- `Sources/kfd/Exploit/libkfd/krkw/IOSurface_shared.h` records in its own header
  that the technique is taken from weightBufs.
- `xpf/decompress.c` records in its own header that it is taken from golb and is
  Apache-2.0. Its upstream `decompress.LICENSE` file is not bundled here; the
  full Apache License 2.0 text is included at [`licenses/Apache-2.0.txt`](licenses/Apache-2.0.txt).
- `libchoma.a` and `libgrabkernel2.a` are prebuilt static archives. The full
  source trees used to build them are not included in this repository; get them
  from the upstream projects listed above.
- Per-file copyright and license headers in the source are the authoritative
  record. This table is a summary, not a replacement for them.

## License texts

### MIT License

Applies to kfd, weightBufs, Dopamine, TrollInstallerX, XPF, Choma and
libgrabkernel2, each with its own copyright holder:

- Copyright (c) 2023 Félix Poulin-Bélanger (kfd)
- Copyright (c) 0x36 (weightBufs)
- Copyright (c) Lars Fröder / opa334 (Dopamine, XPF, Choma)
- Copyright (c) Alfie CG / alfiecg24 (TrollInstallerX, libgrabkernel2)

```
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

### BSD 2-Clause License

Applies to the libarchive headers.

- Copyright (c) 2003-2010 Tim Kientzle
- Copyright (c) 2016 Martin Matuska

```
Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:
1. Redistributions of source code must retain the above copyright
   notice, this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright
   notice, this list of conditions and the following disclaimer in the
   documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE AUTHOR(S) "AS IS" AND ANY EXPRESS OR
IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES
OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED.
IN NO EVENT SHALL THE AUTHOR(S) BE LIABLE FOR ANY DIRECT, INDIRECT,
INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT
NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF
THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```

### Apache License 2.0

Applies to `xpf/decompress.c`, taken from golb. Full text:
[`licenses/Apache-2.0.txt`](licenses/Apache-2.0.txt).

### Apple Public Source License 2.0

Applies to the Apple dyld format headers vendored inside Choma. Those files
carry Apple's `@APPLE_LICENSE_HEADER_START@` block, which is the authoritative
notice. The license is available at <https://opensource.apple.com/apsl/>.

- Copyright (c) 2003-2010 Apple Inc. All rights reserved.
- Copyright (c) 2006-2015 Apple Inc. All rights reserved.
- Copyright (c) 2018 Apple Inc. All rights reserved.
