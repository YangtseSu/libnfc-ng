# libnfc-ng

`libnfc-ng` is a standalone fork of libnfc used by NFC Foundry.

This repository is at **plan 00** scope: it is a fresh clone of upstream git
with its own identity. No NFCX patches, ABI rename, driver-profile change, or
pure-library packaging change has been applied yet.

## Baseline

- Source: `https://github.com/nfc-tools/libnfc` git `master`
- Base commit: `3fa0751ad58fb0053d3de2e61bbbd4066259104c`
- Branch: `main`
- Remotes: origin only; no upstream tracking remote
- Drivers: unchanged upstream default; `--with-drivers=all` remains available
- Patches: none applied yet; Phase 01 will carry the required NFCX patches
- License: LGPL-3.0-or-later (`COPYING`); utilities/examples are BSD-2-Clause

## ABI decision (recorded, not implemented)

Phase 01 will install:

```text
/usr/lib/libnfcng.so.6
/usr/include/libnfc-ng/nfc/
/usr/lib/pkgconfig/libnfc-ng.pc
```

The current baseline still builds the upstream `libnfc` target. Do not describe
the ABI rename as done.

## Build

```sh
autoreconf -fi
./configure --prefix=/usr --disable-conffiles --enable-envvars
make
```

Keep the upstream default driver set. A consumer may select a profile with
`--with-drivers=...` explicitly; that belongs in the consumer package/CI, not
in this repository's default.

## Relationship to NFC Foundry

NFC Foundry pins a commit from this repository and links it through a narrow
CGO shim. It does not vendor the library into the application directory.

## Upstream attribution

This is an unofficial fork and is not affiliated with the upstream maintainers.
Upstream libnfc remains LGPL-3.0-or-later; its notice is preserved in `COPYING`.
