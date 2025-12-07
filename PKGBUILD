# Maintainer: Noa Himesaka <himesaka@noa.codes>
pkgname=tiny-dfr
pkgver=0.3.2
pkgrel=1
pkgdesc="The most basic dynamic function row daemon possible"
arch=('x86_64')
license=('MIT')
depends=('linux-t2' 'pango' 'libinput' 'gdk-pixbuf2' 'ttf-ubuntu-font-family' 'librsvg')
conflicts=('touchbard')
makedepends=('git' 'cargo' 'librsvg')
source=("git+https://github.com/AsahiLinux/tiny-dfr"
        "t2-intel.conf")
sha256sums=('SKIP'
            'SKIP')

pkgver() {
  cd "$pkgname"
  git describe --long --tags --abbrev=7 | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "$pkgname"
    export RUSTUP_TOOLCHAIN=stable
    cargo fetch --locked --target "$CARCH-unknown-linux-gnu"
}

build() {
    cd "$pkgname"
    export RUSTUP_TOOLCHAIN=stable
    export CARGO_TARGET_DIR=target
    cargo build --frozen --release --all-features
}

package() {
  # Install binary
	install -Dm755 "$pkgname/target/release/tiny-dfr" "$pkgdir/usr/bin/tiny-dfr"
	# Install systemd service
  install -Dm644 "$pkgname/etc/systemd/system/tiny-dfr.service" "$pkgdir/usr/lib/systemd/system/tiny-dfr.service"
  # Install drop-in override for Intel T2 Macs (removes Apple Silicon device bindings)
  install -Dm644 "$srcdir/t2-intel.conf" "$pkgdir/usr/lib/systemd/system/tiny-dfr.service.d/t2-intel.conf"
  install -Dm644 "$pkgname/etc/systemd/system/systemd-backlight@backlight:228200000.display-pipe.0.service" "$pkgdir/usr/lib/systemd/system/systemd-backlight@backlight:228200000.display-pipe.0.service"
	install -Dm644 "$pkgname/etc/systemd/system/systemd-backlight@backlight:appletb_backlight.service" "$pkgdir/usr/lib/systemd/system/systemd-backlight@backlight:appletb_backlight.service"
	# Install udev rule
	install -Dm644 "$pkgname/etc/udev/rules.d/99-touchbar-seat.rules" "$pkgdir/usr/lib/udev/rules.d/99-touchbar-seat.rules"
  install -Dm644 "$pkgname/etc/udev/rules.d/99-touchbar-tiny-dfr.rules" "$pkgdir/usr/lib/udev/rules.d/99-touchbar-tiny-dfr.rules"
	# Install config
	mkdir -p "$pkgdir/etc/tiny-dfr"
  install -Dm644 "$pkgname/share/tiny-dfr/config.toml" "$pkgdir/etc/tiny-dfr/config.toml"
	# Install resources
	mkdir -p "$pkgdir/usr/share/"
	cp -r "$pkgname/share/tiny-dfr" "$pkgdir/usr/share/tiny-dfr"
}
