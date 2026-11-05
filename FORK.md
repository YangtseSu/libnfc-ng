# libnfc-ng

`libnfc-ng` is a standalone fork of libnfc used by NFC Foundry.

This repository is at **plan 01** scope: it is a Linux library package with a
unique ABI identity and no installed command-line utilities. Card protocols and
application integration belong to later phases.

## Identity

Phase 01 installs:

```text
/usr/lib/libnfcng.so.6
/usr/include/libnfc-ng/nfc/
/usr/lib/pkgconfig/libnfc-ng.pc
```

The C API and exported symbol names are unchanged. Consumers compile with
`pkg-config --cflags --libs libnfc-ng`; the header prefix is not on the default
include path. The separate soname lets the fork coexist with Arch's `libnfc`
package.

## Baseline

- Upstream source: `https://github.com/nfc-tools/libnfc` git `master`
- Upstream base commit: `3fa0751ad58fb0053d3de2e61bbbd4066259104c`
- Branch: `main`
- Phase 01 baseline tag: `v1.8.0-ng.1`
- License: LGPL-3.0-or-later (`COPYING`); utilities/examples are BSD-2-Clause
- Fork patches: the `LIBNFC_DEVICE`/conffiles fix is committed directly, not
  kept as a patch series

## Drivers

The upstream default driver set in `m4/libnfc_drivers.m4` is unchanged, and
`--with-drivers=all` still builds every available driver. The NFC Foundry
package profile is selected explicitly by the consumer:

```text
--with-drivers=pn532_uart,pn53x_usb,acr122_usb,acr122_pcsc,pcsc
```

Only `pn532_uart` is validated. Compiling a driver does not advertise support
for it.

## Build

```sh
autoreconf -fi
./configure --prefix=/usr --sysconfdir=/etc \
  --disable-static --disable-conffiles --enable-envvars
make
```

`--disable-conffiles` avoids `/etc/nfc`; environment variables such as
`LIBNFC_DEVICE` remain available. Upstream utilities and examples are
diagnostic-only, disabled by default, and never installed:

```sh
./configure --enable-utils --enable-example
```

`make install` installs the shared library, the public headers and the
pkg-config file.

## Packaging

The Arch package is `libnfc-ng-git`. It builds the NFC Foundry profile above
and must not install `libnfc.so.6` or any `nfc-*` executable. System `libusb`
and `pcsclite` are used; nothing is bundled.

## Relationship to NFC Foundry

NFC Foundry pins a commit from this repository and links it through a narrow
CGO shim. It does not vendor the library into the application directory.

## Upstream attribution

This is an unofficial fork and is not affiliated with the upstream maintainers.
Upstream libnfc remains LGPL-3.0-or-later; its notice is preserved in `COPYING`.
