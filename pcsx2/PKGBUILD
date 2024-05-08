# Maintainer: éclairevoyant
# Contributor: rafaelff <rafaelff at gnome dot org>
# Contributor: WeirdBeard <obarrtimothy at gmail dot com>
# Contributor: Themaister <maister at archlinux dot us>
# Contributor: Maxime Gauduin <alucryd at archlinux dot org>
# Contributor: josephgbr <rafael dot f dot f1 at gmail dot com>
# Contributor: vEX <vex at niechift dot com>

pkgname=pcsx2
pkgver=1.7.5788
pkgrel=1
pkgdesc='A Sony PlayStation 2 emulator'
arch=(x86_64)
url=https://www.pcsx2.net
license=(
  GPL2
  GPL3
  LGPL2.1
  LGPL3
)
depends=(
  ffmpeg
  glibc
  libaio.so
  libgl
  libharfbuzz.so
  libpcap.so
  libpng
  libudev.so
  libx11
  libxcb
  libxrandr
  libzstd.so
  qt6-base
  qt6-svg
  sdl2
  shaderc-non-semantic-debug
  soundtouch
  wayland
  xz
  zlib
)
makedepends=(
  clang
  cmake
  extra-cmake-modules
  git
  libpipewire
  libpulse
  lld
  llvm
  ninja
  p7zip
  qt6-tools
  qt6-wayland
)
optdepends=('qt6-wayland: Wayland support'
            'libpipewire: Pipewire support'
            'libpulse: PulseAudio support')

_tag=7bc5427908a0e2985bd354446e4bfeeedd4cd45b
options=(!lto)
source=(
	"git+https://github.com/PCSX2/pcsx2.git#tag=${_tag}"
	git+https://github.com/PCSX2/pcsx2_patches.git
	git+https://github.com/google/googletest.git
	git+https://github.com/fmtlib/fmt.git
	git+https://github.com/biojppm/rapidyaml.git
	git+https://github.com/biojppm/cmake.git
	git+https://github.com/biojppm/c4core.git
	git+https://github.com/biojppm/debugbreak.git
	git+https://github.com/fastfloat/fast_float.git
	vulkan-headers::git+https://github.com/KhronosGroup/Vulkan-Headers.git
)
b2sums=('4e7df739987b26a0af09bbc807355eca2f7c3d7df671abbcdba50325f449dfa2c6db2bb63c093af93e934a3ff00e55407ad77918823afb60992b39ce8dfbc3c6'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP'
        'SKIP')
install=$pkgname.install

prepare() {
	cd $pkgname
	local submodule
	_pcsx2_submodules=(
		googletest::3rdparty/gtest
		fmt::3rdparty/fmt/fmt
		rapidyaml::3rdparty/rapidyaml/rapidyaml
		vulkan-headers::3rdparty/vulkan-headers
	)
	# must be done this way due to recursive submodules
	for submodule in ${_pcsx2_submodules[@]}; do
		git submodule init "${submodule#*::}"
		git submodule set-url "${submodule#*::}" "$srcdir/${submodule%::*}"
		git -c protocol.file.allow=always submodule update "${submodule#*::}"
	done

	cd 3rdparty/rapidyaml/rapidyaml
	for submodule in ext/c4core; do
		git submodule init ${submodule}
		git submodule set-url ${submodule} "$srcdir/${submodule##*/}"
		git -c protocol.file.allow=always submodule update ${submodule}
	done

	cd ext/c4core
	for submodule in cmake src/c4/ext/{debugbreak,fast_float}; do
		git submodule init ${submodule}
		git submodule set-url ${submodule} "$srcdir/${submodule##*/}"
		git -c protocol.file.allow=always submodule update ${submodule}
	done
}

pkgver() {
	git -C $pkgname describe --tags | sed 's/^v//'
}

build() {
	# 7zip the patches
	pushd pcsx2_patches
	7z a -r "$srcdir/patches.zip" patches/.
	popd

	# see .github/workflows/scripts/linux/generate-cmake-qt.sh
	cmake -S $pkgname -B build -G Ninja \
		-DCMAKE_C_COMPILER=clang \
		-DCMAKE_CXX_COMPILER=clang++ \
		-DCMAKE_EXE_LINKER_FLAGS_INIT="-fuse-ld=lld" \
		-DCMAKE_MODULE_LINKER_FLAGS_INIT="-fuse-ld=lld" \
		-DCMAKE_SHARED_LINKER_FLAGS_INIT="-fuse-ld=lld" \
		-DCMAKE_INTERPROCEDURAL_OPTIMIZATION=ON \
		-DCMAKE_BUILD_TYPE=Release \
		-DX11_API=ON \
		-DWAYLAND_API=ON \
		-DENABLE_SETCAP=OFF \
		-DDISABLE_ADVANCE_SIMD=TRUE \
		-DCMAKE_INSTALL_PREFIX=/usr

	ninja -C build -v
}

package() {
	install -Dm644 patches.zip -t "$pkgdir/opt/$pkgname/resources/"
	cp -r build/bin/* "$pkgdir/opt/$pkgname/"

	install -Dm755 /dev/stdin "$pkgdir/usr/bin/pcsx2-qt" <<eof
#!/usr/bin/env sh
/opt/pcsx2/pcsx2-qt "\$@"
eof

	cd $pkgname
	install -Dm644 .github/workflows/scripts/linux/pcsx2-qt.desktop \
		-t "$pkgdir/usr/share/applications/"
	install -Dm644 bin/resources/icons/AppIconLarge.png \
		"$pkgdir/usr/share/icons/hicolor/64x64/apps/$pkgname.png"
}
