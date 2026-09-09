# Lumen native relinking permission

Issued by **Baiqing Lyu (Lumen)** on **September 8, 2026**, following the publisher's approval.

## Covered release

- Application: Lumen `0.1.0`, build `1000000`, bundle `com.lumen.mediaplayer`.
- Native production baseline: `2df59fc627c18febf9b09db63405538bef0e377e`.
- Native library: `liblumencapabilities.so`, ARM64, 669,256 bytes.
- Native library SHA-256: `4ffb19a289978e51289fdc2c2ea65d5ad179bf299d37a0fc7f375e46143c0996`.
- Version-specific kit: `lumen-0.1.0-1000000-native-relink-2df59fc-issued.zip`.
- Source and relink download page: https://baiqingl.github.io/lumen-public/source.html
- Release record and detached archive checksum: https://github.com/BaiqingL/lumen-public/releases/tag/native-relink-v0.1.0-1000000

The kit's `manifest.json` identifies the supplied files and their SHA-256 hashes. The release record supplies the checksum of the complete archive. These identifiers associate this permission with the supplied original materials; they do not expand its scope.

## Recipient permission

Lumen's native relink kit contains third-party software and a limited set of original Lumen application materials. This permission applies only to the original Lumen materials expressly listed below, supplied in the version-specific kit. Third-party software remains governed by its own notices and licenses, including the GNU Lesser General Public License, version 2.1 or later. This permission does not limit those third-party rights.

The covered original Lumen materials are:

- `objects/capabilities.cpp.o`
- `objects/local_source.cpp.o`
- `objects/local_subtitles.cpp.o`
- `objects/matroska_subtitles.cpp.o`
- `objects/dolby_config.cpp.o`
- `objects/dolby_http.cpp.o`
- `native/dolby_session.cpp`
- `native/dolby_session.h`
- `native/dolby_config.h`
- `native/dolby_http.h`
- `native/exports.map`
- The kit's `rebuild.py`, `replace-native.py` and accompanying rebuild instructions.

The publisher grants recipients a non-exclusive, worldwide, royalty-free permission to copy and use these covered materials to rebuild or relink their copy of Lumen with a modified or replacement version of the LGPL-covered native library. Recipients may modify the supplied integration source, rebuild instructions and scripts for that purpose. They may modify their copy of Lumen for their own use, use the resulting modified application, and reverse engineer or debug Lumen to debug those modifications.

This permission applies to the covered materials supplied with the kit and continues for those copies. Recipients must retain the accompanying copyright, license and attribution notices. The publisher does not require access to its private repository, a publisher account, or the publisher's signing keys to exercise this permission. Platform signing and installation requirements remain separate from this permission.

This permission does not grant a general license to Lumen's unrelated application source, artwork, name or trademarks. It does not grant redistribution rights for modified Lumen application binaries or the covered original application materials beyond rights otherwise provided by applicable law or an accompanying license. Redistribution, modification and other rights in the LGPL-covered and other third-party portions remain governed by their respective licenses and are not restricted by this paragraph.

The covered materials are supplied without warranty to the extent permitted by applicable law. This does not limit rights that applicable law does not allow to be limited. Modified builds are not represented as publisher-signed, publisher-supported or officially reviewed releases.

Questions about obtaining the matching kit or using these materials may be sent to `admin@lumenapp.tv`.
