pkgname=lib32-mingw-w64-libgcrypt
pkgver=1.5.0
pkgrel=2
pkgdesc="a general purpose crypto library based on the code used (mingw-w64, 32-bit)"
arch=(any)
url="http://www.gnupg.org"
license=("GPL")
makedepends=(mingw-w64-gcc)
depends=(mingw-w64-crt mingw-w64-libgpg-error)
options=(!libtool !strip !buildflags)
source=("ftp://ftp.gnupg.org/gcrypt/libgcrypt/libgcrypt-${pkgver}.tar.bz2")
md5sums=('693f9c64d50c908bc4d6e01da3ff76d8')

_architectures="i686-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/libgcrypt-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --enable-static \
      --enable-shared
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip -x
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
    rm -r "$pkgdir/usr/${_arch}/share"
  done
}