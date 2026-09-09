# Lumen 0.1.0 native source and relink kit

This publisher-issued kit corresponds to production baseline
`2df59fc627c18febf9b09db63405538bef0e377e`, bundle `com.lumen.mediaplayer`, version
`0.1.0`, build `1000000`. It rebuilds the editable LGPL renderer and relinks
`liblumencapabilities.so` without the private Lumen repository or ArkTS source.
`provenance.json` identifies the native library, original validation package and
matching AppGallery candidate. The approved narrow permission for supplied Lumen
materials is in `relink-permission.md`; third-party license notices are preserved.

Download this version and its detached checksum from:
https://github.com/BaiqingL/lumen-public/releases/tag/native-relink-v0.1.0-1000000

The stable source page is https://baiqingl.github.io/lumen-public/source.html.

## Contents and native link graph

The production library links nine C++ objects and the existing MIT libdovi parser
archive. This kit provides six original, unmodified application objects:

- `capabilities.cpp.o`: native module registration and capabilities.
- `local_source.cpp.o`: local media inspection.
- `local_subtitles.cpp.o` and `matroska_subtitles.cpp.o`: subtitle access.
- `dolby_config.cpp.o`: container configuration reading.
- `dolby_http.cpp.o`: native callback adapter.

The other three objects are compiled from editable source:

- `native/dolby/dv_renderer.cpp` and `dv_frame.cpp`, their headers, reviewable GLSL
  inputs, generated shader header, and shader generator retain LGPL-2.1-or-later.
- `native/dolby_session.cpp` plus three integration headers are included because
  it compiles the inline `VideoDisplayAspect` implementation in LGPL
  `dv_geometry.h`. An opaque session object alone would prevent those header
  edits from taking effect in the session. No other application implementation
  source is required for this native relink route.

`native/exports.map` preserves the production NAPI entry point and private symbol
visibility. `native/dolby/liblumen_dv_parser.a` is the original pinned MIT parser
archive. Its adapter source, Cargo lockfile, upstream revision, allocation-bounds
patch, and original build instructions are also included. Runtime and dependency
notices are preserved in `notices/` and `native/dolby/`.

The native link dependencies are HarmonyOS `ace_napi.z`, `native_media_codecbase`,
`native_media_avsource`, `native_media_avdemuxer`, `native_media_core`, `EGL`,
`GLESv3`, `native_image`, `native_window`, the C++ runtime, math, unwind and C
runtime supplied by the Harmony SDK/device. No SDK binaries or signing material
are included. The six object files and parser archive are from the original
release build, not recompilations of unrelated private application code.

## Rebuild and relink

Install Python 3.10+ and the Harmony SDK native component. The demonstrated
toolchain is native SDK **6.1.1.125 / API 24**, OHOS Clang **15.0.4**, targeting
`aarch64-linux-ohos`. The source application's minimum/target API is 20; a compiler
SDK version is not the same as the application's minimum OS requirement.

Extract this kit, open PowerShell in its directory, and run:

```powershell
python -X utf8 rebuild.py --sdk 'C:/Program Files/Huawei/DevEco Studio/sdk/default/openharmony/native' --verify-inputs
```

This checks the complete input hash manifest, regenerates shaders, compiles the
three editable C++ units, relinks the original object files and parser archive,
then produces `build/liblumencapabilities.so` and a JSON receipt. No network,
Rust installation, CMake cache, account, or private repository is needed for this
route. The unstripped library remains alongside the stripped output for debugging.

To change the renderer, edit `native/dolby/dv_renderer.cpp`, `dv_frame.cpp`, their
headers, or the GLSL input files. Run the same command without `--verify-inputs`;
the original input hashes intentionally stop matching after an edit. Generated
files should be changed through `generate_shaders.py` and the corresponding GLSL
inputs. The build always recompiles `dolby_session.cpp` so compatible edits to
the inline geometry header take effect throughout this library. Preserve the
external NAPI and parser ABI expected by the unchanged app and parser archive.

The full library can be relinked with compatible modified implementations. This
kit does not make arbitrary ABI changes compatible with the unchanged ArkTS or
Rust binary, and no such compatibility is claimed.

## Optional parser rebuild

The parser is MIT, not the LGPL component requiring this relink route. Rebuilding
it is optional. Its complete adapter source and `Cargo.lock` are supplied; the
upstream crate is pinned at quietvoid/dovi_tool
`02a368cc70490a3ce8e4ccbde1abef4a9c77d602` and version 3.4.0. To reconstruct it,
follow the original `native/dolby/build.ps1` using Rust 1.98.1 with target
`aarch64-unknown-linux-ohos`; that path downloads the pinned upstream/toolchain
and applies `libdovi-bounds.patch`. Do **not** use its `-CopyToApp` option in this
standalone kit: that option is for the original repository layout. After a parser
rebuild, copy its resulting `parser/target/aarch64-unknown-linux-ohos/release/`
`liblumen_dv_parser.a` over the supplied archive and rerun `rebuild.py`.

The preserved original renderer CMake project also builds diagnostic executables;
those diagnostics are not compiled into the production relink route. The original
source manifest distinguishes historical validation from current renderer hashes.
The kit's `manifest.json` is authoritative for the actual included bytes.

## Put a replacement library into a package

Obtain the matching original Lumen HAP separately. The helper checks its bundle,
version and original native-library hash before replacing exactly one member:

```powershell
python -X utf8 replace-native.py --input-hap 'C:/path/to/original.hap' --library 'build/liblumencapabilities.so' --output 'build/lumen-modified-unsigned.hap'
```

Every other ZIP member is retained. The old package signing block is removed;
the output is deliberately **unsigned**. Sign it with Huawei's HAP signing tools
and your own certificate/profile for this bundle and device. Developer Mode,
device trust and a matching profile are platform prerequisites. No publisher
private key is needed to rebuild or relink the library and none is provided.

A different signature cannot update the publisher-signed app while preserving its
installation identity. Use a separate test device/environment for a modified
build. This kit never installs, uninstalls, or clears application data. The
provided evidence demonstrates native compilation, relinking, and construction
of an unsigned HAP; replacement signing/installation is a separate, unverified
step. A future store package must be matched to its exact released native hash
and accompanied by instructions validated for that package.

## Release preparation limits

- The publisher supplies this version through the public release linked above.
  Access does not require the private application repository.
- Preserve LGPL and dependency license texts and attribution. The issued
  `relink-permission.md` governs the listed original application materials;
  third-party software retains its accompanying licenses.
- This is a technical contents and rebuild demonstration, not a conclusion that
  every license obligation, AppGallery requirement, or distribution-signing
  condition has been satisfied.
- The existing Rust archive carries runtime code. The preserved notices document
  attribution and known provenance limits; the exact historical compiler-builtins
  source checkout is not included. That does not prevent the demonstrated native
  rebuild, which reuses this unchanged archive.
- No private signing profile, password, account data, device identifier, ArkTS/UI
  implementation, or media sample belongs in this kit. Object/archive files are
  original compiled build inputs and may contain compiler metadata and local
  build paths; they have not been rewritten to remove that metadata.
