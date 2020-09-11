# Maintainer: Jasper van Bourgognie <louiecaulfield@gmail.com>

pkgname=libinput-three-finger-drag
pkgver=1.16.1
pkgrel=1
pkgdesc="Input device management and event handling library"
url="https://www.freedesktop.org/wiki/Software/libinput/"
arch=(x86_64)
license=(custom:X11)
provides=('libinput')
conflicts=('libinput')
depends=('mtdev' 'systemd' 'libevdev' 'libwacom')
# upstream doesn't recommend building docs
makedepends=('gtk3' 'meson') # 'doxygen' 'graphviz' 'python-sphinx' 'python-recommonmark'
optdepends=('gtk3: libinput debug-gui'
            'python-pyudev: libinput measure'
            'python-libevdev: libinput measure')
source=("git://github.com/louiecaulfield/libinput")
md5sums=('SKIP')

build() {
  arch-meson libinput build \
    -Dudev-dir=/usr/lib/udev \
    -Dtests=false \
    -Ddocumentation=false
  ninja -C build
}

package() {
  DESTDIR="$pkgdir" ninja -C build install

  install -Dvm644 libinput/COPYING \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
