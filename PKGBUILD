# Maintainer: Alphazo <alphazo@gmail.com>

pkgname=basez
pkgver=1.6
pkgrel=1
pkgdesc='Encodes/decodes base16, base32, base32hex, base64, base64url, base64mime, base64pem PEM, base32plain, base64plain. base16 and base32 are NOT included in this package since they would conflict with the one already in coreutils.'
arch=('x86_64')
url='https://http://www.quarkline.net/basez//'
depends=('gcc-libs')
license=('GPL')

source=("http://www.quarkline.net/basez/download/${pkgname}-${pkgver}.tar.gz")
md5sums=('15c115415a51b79312d711760e60d662')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --disable-base32-command --disable-base64-command
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make DESTDIR="${pkgdir}" install
}
