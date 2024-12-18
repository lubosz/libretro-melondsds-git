# Maintainer: Alexandre Bouvier <contact@amb.tf>
_pkgname=libretro-melondsds
pkgname=$_pkgname-git
pkgver=1.1.8.r0.g08862a8
pkgrel=1
pkgdesc="Nintendo DS core"
arch=('aarch64' 'armv7h' 'i486' 'i686' 'pentium4' 'x86_64')
url="https://github.com/JesseTG/melonds-ds"
license=('GPL-3.0-or-later')
groups=('libretro')
depends=('gcc-libs' 'glibc' 'libretro-core-info>=1.16.0.3.r2')
makedepends=('cmake>=3.19' 'git' 'libgl')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
source=(
	"$_pkgname::git+$url.git"
	'git+https://github.com/melonDS-emu/melonDS.git'
	'git+https://github.com/libretro/libretro-common.git'
	'git+https://github.com/andoalon/embed-binaries.git'
	'git+https://github.com/g-truc/glm.git'
	'git+https://gitlab.freedesktop.org/slirp/libslirp.git'
	'git+https://github.com/RobLoach/pntr.git'
	'git+https://github.com/fmtlib/fmt.git'
	'git+https://github.com/yohhoy/yamc.git'
	'git+https://github.com/martinmoene/span-lite.git'
	'git+https://github.com/HowardHinnant/date.git'
	'git+https://github.com/madler/zlib.git'
)
b2sums=(
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
)

pkgver() {
	cd $_pkgname
	git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
	# shellcheck disable=SC2154
	local cmake_args=(
		-Wno-dev
		-S "$_pkgname"
		-B build
		-D CMAKE_BUILD_TYPE=Release
		-D CMAKE_C_FLAGS_RELEASE="-DNDEBUG"
		-D CMAKE_CXX_FLAGS_RELEASE="-DNDEBUG"
		# fetch_dependency overrides
		-D DATE_REPOSITORY_URL="$srcdir"/date
		-D EMBED_BINARIES_REPOSITORY_URL="$srcdir"/embed-binaries
		-D FMT_REPOSITORY_URL="$srcdir"/fmt
		-D GLM_REPOSITORY_URL="$srcdir"/glm
		-D LIBRETRO_COMMON_REPOSITORY_URL="$srcdir"/libretro-common
		-D LIBSLIRP_REPOSITORY_URL="$srcdir"/libslirp
		-D MELONDS_REPOSITORY_URL="$srcdir"/melonDS
		-D PNTR_REPOSITORY_URL="$srcdir"/pntr
		-D SPAN_LITE_REPOSITORY_URL="$srcdir"/span-lite
		-D YAMC_REPOSITORY_URL="$srcdir"/yamc
		-D ZLIB_REPOSITORY_URL="$srcdir"/zlib
	)
	cmake "${cmake_args[@]}"
	cmake --build build
}

package() {
	depends+=('libOpenGL.so')
	# shellcheck disable=SC2154
	install -D -t "$pkgdir"/usr/lib/libretro build/src/libretro/melondsds_libretro.so
	install -Dm644 -t "$pkgdir"/usr/share/licenses/$pkgname build/melondsds-LICENSE.txt
}
