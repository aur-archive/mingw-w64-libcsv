pkgname=mingw-w64-libcsv
pkgver=3.0.3
pkgrel=3
pkgdesc="C library for csv format (mingw-w64)"
arch=(any)
url="http://sourceforge.net/projects/libcsv"
license=("GPL")
makedepends=(mingw-w64-gcc)
depends=(mingw-w64-crt)
options=(!libtool !strip !buildflags)
source=("http://downloads.sourceforge.net/libcsv/libcsv-$pkgver.tar.gz")
md5sums=('d3307a7bd41d417da798cd80c80aa42a')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/${pkgname#mingw-w64-}-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch}
    make libcsv_la_LDFLAGS="-no-undefined -version-info 3:2:0"
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' -o -name '*.bat' -o -name '*.def' -o -name '*.exp' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip -x
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
    rm -r "$pkgdir/usr/${_arch}/share"
  done
}
