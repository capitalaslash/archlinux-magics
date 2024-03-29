# Maintainer: Antonio Cervone <ant.cervone@gmail.com>
# Contributor: Gabriel Chamon Araujo <gchamon@live.com>
# Contributor: Anton Bazhenov <anton.bazhenov at gmail>
# Contributor: Graziano Giuliani <graziano.giuliani@poste.it>

pkgname=magics++
Pkgname=Magics
pkgver=4.9.4
_attnum=3473464
pkgrel=0
pkgdesc="Magics is the latest generation of the ECMWF's Meteorological plotting software MAGICS."
arch=('i686' 'x86_64')
url="https://software.ecmwf.int/wiki/display/MAGP"
license=('Apache')
depends=('eccodes>=2.19.0' libgeotiff qt6-base pango python)
optdepends=(ksh libaec odb_api)
makedepends=(cmake gcc-fortran perl-xml-parser python-jinja swig)
source=(http://software.ecmwf.int/wiki/download/attachments/${_attnum}/${Pkgname}-${pkgver}-Source.tar.gz
        gcc11.patch)
sha256sums=('2559c2a6d6aabb8c219e7039b08f8a8108c360969c61e703d5558729f8834cdc'
            'c0250ac473b4703e79c8f879f6271d1409e800a0f8a7a77b3793cc9ce9a85951')

prepare() {
  cd "${Pkgname}-${pkgver}-Source"
  patch --forward --strip=1 --input=$srcdir/gcc11.patch
}

build() {
  [ -x /usr/bin/odb ] && has_odb=ON || has_odb=OFF
  # force this for now. Metview does not compile
  has_odb=OFF

  cmake \
    -B build \
    -S "${Pkgname}-${pkgver}-Source" \
    -DCMAKE_SHARED_LINKER_FLAGS="-pthread" \
    -DCMAKE_EXE_LINKER_FLAGS="-pthread" \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=production \
    -DCMAKE_INSTALL_DATADIR=/usr/share \
    -DENABLE_METVIEW=ON \
    -DENABLE_GEOTIFF=ON \
    -DENABLE_ODB=${has_odb}

  make -C build
}

package() {
  make -C build DESTDIR="$pkgdir" install
}

