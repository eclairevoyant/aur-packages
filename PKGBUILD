# Maintainer: Frederic Bezies < fredbezies at gmail dot com>
# Contributor: Jeffrey Clark (h0tw1r3) <dude@zaplabs.com>

pkgname=attract
pkgver=2.6.1
pkgrel=2
pkgdesc="A graphical front-end for command line emulators that hides the underlying operating system and is intended to be controlled with a joystick or gamepad."
arch=('i686' 'x86_64')
url="http://www.attractmode.org/"
license=('GPL3')
depends=('curl' 'sfml' 'ffmpeg' 'libxinerama' 'libarchive')
makedepends=('git')
provides=('attract')
conflicts=('attractmode-git')
source=("https://github.com/mickelson/${pkgname}/archive/v${pkgver}.tar.gz"
	"https://github.com/mickelson/${pkgname}/releases/download/v1.6.2/ATTRACT.MODE.intro.16-9.v6.1080p.mp4"
	"https://github.com/mickelson/${pkgname}/releases/download/v1.6.2/ATTRACT.MODE.intro.4-3.v6.1080p.mp4"
	attract.desktop)
sha256sums=('4c17641ce0a0b9ff77da5e75f0cb26cc48e4c6295ef57d86a37db18b2bd9fbe9'
            '0a21286df3bd51edd24f6eb21a246f8a6576020d34ce3f5cfe93304dbbadc23b'
            '5750cf26864a86d7625f52223ac1a4050ff4475a016bb4f94fc506b7558dbaf9'
            'd5e94d30bf6329bef87f58aad31ae9ff4632550a8576c243e41f220262c36cf6')

build() {
	cd "${pkgname}-${pkgver}"
	VERSION="$pkgver" make prefix=/usr
}

package() {
	cd "${pkgname}-${pkgver}"
	make prefix="${pkgdir}/usr" install
	install -Dm644 License.txt ${pkgdir}/usr/share/licenses/${pkgdir}/License.txt
	install -Dm644 ../ATTRACT.MODE.intro.16-9.v6.1080p.mp4 ${pkgdir}/usr/share/attract/intro/intro.mp4
	install -Dm644 ../ATTRACT.MODE.intro.4-3.v6.1080p.mp4 ${pkgdir}/usr/share/attract/intro/intro_4x3.mp4
	install -Dm644 util/icon.png ${pkgdir}/usr/share/pixmaps/attract.png
	install -Dm644 ${srcdir}/attract.desktop ${pkgdir}/usr/share/applications/attract.desktop
}

