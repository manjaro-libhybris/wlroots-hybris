# Maintainer: Bardia Moshiri <fakeshell@bardia.tech>
# Maintainer: Adrian Perez de Castro <aperez@igalia.com>
# Maintainer: luka177 <lukapanio@gmail.com>
pkgname=wlroots-hybris
pkgver=0.12.0.r142.g599e4aa4
pkgrel=1
license=(custom:MIT)
pkgdesc='Modular Wayland compositor library (hwcomposer compatible)'
url=https://github.com/droidian/wlroots
arch=(x86_64 aarch64)
provides=("libwlroots.so" "wlroots=${pkgver%%.r*}" "wlroots")
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
	install -d ${pkgdir}/usr/lib/pkgconfig/
	ln -s /usr/local/lib/libwlroots.so "${pkgdir}"/usr/lib/libwlroots.so
	ln -s /usr/local/lib/libwlroots.so "${pkgdir}"/usr/lib/libwlroots.so.7
	install -d ${pkgdir}/usr/include/
	ln -s /usr/local/include/wlr "${pkgdir}"/usr/include/wlr
	ln -s /usr/local/lib/pkgconfig/wlroots.pc "${pkgdir}"/usr/lib/pkgconfig/wlroots.pc
	install -Dm644 "${pkgname}/"LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
