# Lumen 0.1.0 candidate6 native source and relink kit

This kit matches `liblumencapabilities.so`, ARM64, **676,744 bytes**, SHA-256
`d5dd5d01006e570faa90463763bcf7abce513dd6791ec955c0861f7c08942601`,
in Lumen `0.1.0` / `1000000`, bundle `com.lumen.mediaplayer`, candidate6.
The implementation is commit `d626e534e97efbb0c9771396fdbc40b5d81d6971`.

Use the binary hash and candidate number to select the kit: earlier candidates
used the same app version. The version-specific release is
https://github.com/BaiqingL/lumen-public/releases/tag/native-relink-v0.1.0-1000000-candidate6
and the stable source page is https://lumenapp.tv/source.html.
Historical kits remain associated with their original native hashes.

`manifest.json` identifies every supplied input. `provenance.json` records exact
source/object origins and the development-signed package used for local native
reproduction. Final distribution-signed APP/HAP hashes are recorded separately
with the public release; this kit contains no signed application package.

## Contents and permissions

Nine C++ objects and the pinned MIT libdovi parser archive form the library.
Six exact application objects are supplied: `capabilities.cpp.o`,
`local_source.cpp.o`, `local_subtitles.cpp.o`, `matroska_subtitles.cpp.o`,
`dolby_config.cpp.o`, and `dolby_http.cpp.o`. The other three are rebuilt from
editable `native/dolby_session.cpp`, `native/dolby/dv_frame.cpp`, and
`native/dolby/dv_renderer.cpp`.

Headers, GLSL inputs, shader generator, parser adapter/lockfile/patch, upstream
references and license notices are supplied. The renderer and LGPL-derived
materials retain LGPL-2.1-or-later. `native/dolby/MODIFICATIONS.md` and `NOTICE.md`
record dated Lumen modifications. The session source allows inline geometry
changes in `dv_geometry.h` to affect both consumers. `native/exports.map`
preserves the NAPI ABI. `reference/application-CMakeLists.txt` is a reference;
the standalone rebuild does not need the private CMake checkout.

`relink-permission.md` retains the exact previously approved recipient grant
and covered material scope, with candidate6 technical identifiers. It covers
the six objects, supplied integration files, scripts and rebuild instructions
listed there. It grants no new rights in unrelated app code, artwork, name or
trademarks and does not restrict accompanying third-party licenses.

## Rebuild

Use Python **3.10+** and Harmony native SDK **6.1.1.125 / API 24**, OHOS Clang
**15.0.4**, targeting `aarch64-linux-ohos`. The app's minimum and target API
are **23**. NAPI, media, EGL/GLES, NativeImage, NativeWindow, C++, math, unwind
and C runtime dependencies are provided by the SDK/device. SDK binaries are
not redistributed in this kit.

In PowerShell, from the extracted kit directory:

```powershell
python -X utf8 rebuild.py --sdk 'C:/Program Files/Huawei/DevEco Studio/sdk/default/openharmony/native' --verify-inputs
```

The helper verifies paths, sizes and SHA-256 hashes, regenerates shaders,
recompiles the three editable units, relinks the six supplied objects and parser
archive, and writes `build/liblumencapabilities.so`. The unstripped library and
JSON rebuild receipt remain alongside it. This route requires no network, Rust
installation, private repository, CMake cache or publisher credentials.

Edit renderer/frame code, headers or GLSL inputs, then rerun without
`--verify-inputs`. Original hashes intentionally fail after edits. Generated
shaders should be changed through the GLSL sources and generator. Both consumers
of inline geometry are rebuilt. Preserve the NAPI and parser ABI expected by
the unchanged application and parser; arbitrary ABI changes are not made
compatible by the kit.

The standalone command retains production optimization, hardening, link order,
libraries and stripping. It omits the unused HMS include search path. Three
generated shader files are normalized to LF after regeneration. The external
validation receipt records whether pristine stripped/unstripped outputs are
byte-identical; source inclusion alone is not reproduction evidence.

The distribution overlay prepends dated modification notices to LGPL source
files. The kit generator omits only that exact first notice from shader input
before embedding it, and retains notices in generated source and outside C++
shader strings. It retains all other shader edits. The pre-overlay source hashes
are recorded in provenance; executable tokens and shader strings are unchanged.

## Optional parser rebuild

The MIT parser archive is reused unchanged for normal relinking. Its adapter,
`Cargo.lock` and upstream patch are included. Libdovi is pinned to quietvoid's
dovi_tool revision `02a368cc70490a3ce8e4ccbde1abef4a9c77d602`, version 3.4.0.
`native/dolby/build.ps1` documents Rust **1.98.1** and target
`aarch64-unknown-linux-ohos`, downloads pinned dependencies, and applies
`libdovi-bounds.patch`. That optional route requires network access and Rust.
Do not use `-CopyToApp` from this standalone kit; it assumes the original
repository layout. Replace the supplied parser archive with the resulting
`parser/target/aarch64-unknown-linux-ohos/release/liblumen_dv_parser.a` and rerun
`rebuild.py`. Local candidate6 validation does not repeat Rust compilation.

The preserved renderer CMake project includes diagnostic executables that are
not linked into production. Its `source-manifest.json` keeps historical source
and validation records under explicit historical fields; the candidate6 entry
identifies pinned repository source. Top-level `manifest.json` is authoritative
for all included bytes, including the documented dated-notice overlay.

## Replace the library in a matching package

Obtain a matching original HAP separately, then run:

```powershell
python -X utf8 replace-native.py --input-hap 'C:/path/to/original.hap' --library 'build/liblumencapabilities.so' --output 'build/lumen-modified-unsigned.hap'
```

The helper verifies the bundle, version and original native hash and replaces
exactly one ZIP member, retaining the remaining members. Repacking removes the
old HAP signing block. The result is **unsigned**; the script does not install it.

Use Huawei's signing tools and your own eligible certificate/profile for this
bundle and device. Developer Mode, device trust and a matching profile remain
platform requirements. No publisher signing key is needed to rebuild/relink and
none is supplied. A different signature cannot update a publisher-signed install
while retaining its identity; use a separate test environment. The helper never
installs, uninstalls or clears application data.

## Validation scope

The accompanying candidate6 validation covers pristine/repeated native
reproduction, a modified geometry header rebuilt into both consumers, and
unsigned replacement preserving all other HAP members. Native rebuilding and
repacking do not establish recipient signing/installation, visual correctness,
playback performance, AppGallery approval or a complete legal-compliance result.

The unchanged Rust archive includes runtime code with the provenance limits in
its notices, including the missing exact historical compiler-builtins source
checkout. Preserve all notices. No SDK, signing profile, credentials, account or
device data, unrelated ArkTS/UI source, artwork or media sample is included.
Immutable object/archive files may contain compiler metadata and build paths;
these exact reproduction inputs are not rewritten.
