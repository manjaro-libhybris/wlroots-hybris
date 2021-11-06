# Maintainer: Adrian Perez de Castro <aperez@igalia.com>
# Maintainer: luka177 <lukapanio@gmail.com>
pkgname=wlroots-hybris
pkgver=0.12.0.r60.g035849b0
pkgrel=1
license=(custom:MIT)
pkgdesc='Modular Wayland compositor library (hwcomposer compatible)'
url=https://github.com/droidian/wlroots
arch=(x86_64 aarch64)
provides=("libwlroots.so" "wlroots=${pkgver%%.r*}")
conflicts=(wlroots)
depends=(
	glslang
	libinput
	libxcb
	libxkbcommon
	opengl-driver
	pixman
	wayland
	xcb-util-errors
	xcb-util-renderutil
	xcb-util-wm
	seatd
	vulkan-icd-loader
	xorg-xwayland
	libhybris
	)
makedepends=(
	git
	meson
	vulkan-headers
	wayland-protocols
	xorgproto
	android-headers)
source=("${pkgname}::git+${url}")
sha512sums=('SKIP')

pkgver () {
	cd "${pkgname}"
	(
		set -o pipefail
		git describe --long 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
		printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
	)
}

build () {
	meson \
		-Dwerror=false \
		-Dexamples=false \
		"${pkgname}" build
	meson compile -C build
}

package () {
	DESTDIR="${pkgdir}" meson install -C build
	install -d ${pkgdir}/usr/lib/
	ln -s "${pkgdir}"/usr/local/lib/libwlroots.so "${pkgdir}"/usr/lib/libwlroots.so
	ln -s "${pkgdir}"/usr/local/lib/libwlroots.so "${pkgdir}"/usr/lib/libwlroots.so.7
	install -Dm644 "${pkgname}/"LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
