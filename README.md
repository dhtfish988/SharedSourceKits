# SharedSourceKits

Shared code for iOS kernel research. The repository currently contains **MGExploitation**, a static library that brings kernel exploit setup, patchfinding, memory access and privilege management into one Xcode target.

The library is intended for integration into an iOS research app. It provides a small C interface for starting the chain and reporting failures, with lower-level headers for code that needs kernel memory primitives. Its exploit implementations modify kernel memory and process privileges. Running them requires a compatible device and can crash or reboot that device.

## MGExploitation

The main components are:

| Component | Role |
| --- | --- |
| [MGExploitChain](MGExploitation/Sources/MGExploitChain.mm) | Coordinates kernel cache lookup, exploit selection, memory primitive setup and privilege changes. |
| [KFD](MGExploitation/Sources/kfd/) | Contains the kernel read/write exploit implementations, retry profiles and page-grab diagnostics. |
| [Patchfinding](MGExploitation/Sources/kernel_patchfinder.c) | Uses XPF to locate kernel symbols and offsets in the kernel image. |
| [dmaFail](MGExploitation/Sources/dmaFail/) | Implements the PPL physical read/write setup used by the chain. |
| [KernelSupport](MGExploitation/Sources/KernelSupport/) | Provides address translation, kernel and physical memory access, allocation, process and vnode helpers. |
| [Privilege management](MGExploitation/Sources/privilege_operations.c) | Contains root, platform and Mach port operations. |
| [Public interface](MGExploitation/Sources/Public/) | Exposes the chain result and the headers used by a host application. |

The implementation is written in C, Objective-C and Objective-C++. The Xcode project builds `libMGExploitation.a`; this repository does not include a standalone app.

## Build

Use macOS with Xcode and the iOS SDK installed. The project targets physical iOS devices and uses C17 and C++20. Its deployment target is iOS 15.0.

To browse the source in Xcode, open `MGExploitation/MGExploitation.xcodeproj`. If Xcode reports that it is "already open in another workspace", close the host project or workspace window that already contains MGExploitation, then open it again. You can also edit the library directly inside that host workspace. The message does not mean the project file is damaged.

From the repository root:

```sh
xcodebuild \
  -project MGExploitation/MGExploitation.xcodeproj \
  -target MGExploitation \
  -configuration Debug \
  -sdk iphoneos \
  ARCHS=arm64 \
  ONLY_ACTIVE_ARCH=NO \
  CODE_SIGNING_ALLOWED=NO \
  SYMROOT="$PWD/build/products" \
  OBJROOT="$PWD/build/objects" \
  build
```

The archive is written to `build/products/Debug-iphoneos/libMGExploitation.a`. The project also has a Release configuration. Debug enables the library's diagnostic logger; Release disables it.

## Using the library in a project

Add `MGExploitation.xcodeproj` to the host workspace, add its library target as a dependency and link the resulting archive. Keep the bundled `ThirdParty` directory alongside the project. The host app still needs its own final link settings, signing and entitlements; building the archive alone does not produce a runnable application.

The main header is [MGExploitationKit.h](MGExploitation/Sources/Public/MGExploitationKit.h). It declares two functions:

- `MGXRunExploit` runs the chain synchronously and returns its success state. `MGXRunResult` carries a failure category and page-grab counters.
- `MGXFailureReasonName` converts a failure category to a short diagnostic name.

`MGXRunOptions` currently defines `version` and `flags`, but the implementation does not read them. They do not select an exploit or change its behavior.

[MGXKernelPrimitives.h](MGExploitation/Sources/Public/MGXKernelPrimitives.h) exposes the lower-level memory and process interfaces. It includes other headers by relative path, so preserve the `Sources` directory layout when using it. The code maintains process-wide state and has no concurrency contract for running multiple chains at once.

The lower-level headers use `kernel_support_*` for primitive initialization, `system_info_*` for system metadata operations, and `runtime_info(...)` for runtime configuration. Consumers must rebuild against the current headers and library together. Direct includes must use the current filenames, including `KernelSupport/system_info.h`, `KernelSupport/kernel_primitives_types.h`, `KernelSupport/cpu_family_compat.h`, `KernelSupport/primitives_iosurface.h`, `privilege_operations.h` and `kernel_patchfinder.h`. XPC runtime keys are `runtimeInfo.usesPACBypass` and `runtimeInfo.rootPath`; producers and consumers must use the same schema. `RuntimeRootPath` and `NSRuntimeRootPath` resolve paths relative to the configured root. The optional `exec_cmd_trusted` macro requires a host-provided `kernel_support_trust_binary` declaration and implementation; this library does not provide that service.

## Compatibility and current limits

The chain selects from iOS 14–16-era exploit profiles. The shipped build target starts at iOS 15.0, and the selector has no entries above iOS 16.6.1. The `smith` implementation is present but deliberately skipped by the selector. These checks describe the current code, not a list of devices confirmed to work.

Actual behavior depends on the device, kernel build, available kernel image and later stages of the chain. A matching version check does not establish that physical memory access or privilege changes will succeed.

If a local kernel cache is unavailable, the chain can download one into the host app's Documents directory. The source also includes vnode redirection and persistence-helper routines; these are separate helpers, not steps called by the main chain entry point.

There is no device test harness or compatibility test matrix in this repository. Use it on research devices you own or are authorized to work on.

## Build check

The current source was compiled into ARM64 Debug and Release archives with Xcode 26.6 and the iOS 26.5 SDK on September 13, 2026. Compilation completed with warnings, including unused declarations and integer narrowing conversions. This check did not link a host app or execute the library on an iOS device.

## Included code

MGExploitation is integration code built around existing open-source components written by other people. The exploit implementations under `Sources/kfd/Exploit` come from [kfd](https://github.com/felix-pb/kfd) by Félix Poulin-Bélanger. The kfd wrapper, `Sources/dmaFail` and most of `Sources/KernelSupport` (upstream `libjailbreak`) are derived from [Dopamine](https://github.com/opa334/Dopamine) by Lars Fröder. `Sources/KernelSupport/vnode.*` comes from [TrollInstallerX](https://github.com/alfiecg24/TrollInstallerX) by Alfie CG. `ThirdParty` contains [XPF](https://github.com/opa334/XPF) sources, [Choma](https://github.com/opa334/Choma) and [libgrabkernel2](https://github.com/alfiecg24/libgrabkernel2) headers and prebuilt static archives, and [libarchive](https://github.com/libarchive/libarchive) headers. The bundled archives do not include the full source trees used to build them.

Original copyright notices remain in the source files. The full component list, the original authors and the applicable licenses are in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The integration and build code written for this repository is under [LICENSE](LICENSE).
