# Maintainer: Noa Himesaka <himesaka@noa.codes>
pkgname=tiny-dfr
pkgver=r45.6122386
pkgrel=1
pkgdesc="The most basic dynamic function row daemon possible"
arch=('x86_64')
license=('MIT')
depends=('linux-t2')
conflicts=('touchbard')
makedepends=('git' 'cargo')
source=("git+https://github.com/kekrby/tiny-dfr")
sha256sums=('SKIP')

pkgver() {
  cd "$pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
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
	# Install udev rule
	install -Dm644 "$pkgname/etc/udev/rules.d/99-touchbar-backlight.rules" "$pkgdir/usr/lib/udev/rules.d/99-touchbar-backlight.rules"
	install -Dm644 "$pkgname/etc/udev/rules.d/99-touchbar-seat.rules" "$pkgdir/usr/lib/udev/rules.d/99-touchbar-seat.rules"
        install -Dm644 "$pkgname/etc/udev/rules.d/99-touchbar-tiny-dfr.rules" "$pkgdir/usr/lib/udev/rules.d/99-touchbar-tiny-dfr.rules"
	# Install config
	install -Dm644 "$pkgname/etc/tiny-dfr.conf" "$pkgdir/etc/tiny-dfr.conf"
}
