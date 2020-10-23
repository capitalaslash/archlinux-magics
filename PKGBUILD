# Contributor: Anton Bazhenov <anton.bazhenov at gmail>
# Contributor: Graziano Giuliani <graziano.giuliani@poste.it>
# Contributor: Antonio Cervone <ant.cervone@gmail.com>

pkgname=magics++
Pkgname=Magics
pkgver=4.5.0
_attnum=3473464
pkgrel=1
pkgdesc="Magics is the latest generation of the ECMWF's Meteorological plotting software MAGICS."
arch=('i686' 'x86_64')
url="https://software.ecmwf.int/wiki/display/MAGP"
license=('Apache')
depends=('eccodes>=2.19.0' qt5-base pango proj python)
optdepends=(libaec odb_api)
makedepends=(cmake gcc-fortran perl-xml-parser python-jinja swig)
source=(http://software.ecmwf.int/wiki/download/attachments/${_attnum}/${Pkgname}-${pkgver}-Source.tar.gz)
sha256sums=('d6f5682772ba7012f845b0c37f7bbaf5e12e6e9cf36da7524b234d8dc073ad2c')

build() {
  [ -x /usr/bin/odb ] && has_odb=ON || has_odb=OFF
  # force this for now. Metview does not compile
  has_odb=OFF

  cmake \
    -B build \
    -S "${Pkgname}-${pkgver}-Source" \
    -DCMAKE_LINKER_FLAGS="-pthread" \
    -DCMAKE_SHARED_LINKER_FLAGS="-pthread" \
    -DCMAKE_EXE_LINKER_FLAGS="-pthread" \
    -DENABLE_ODB=${has_odb} \
    -DGEOTIFF_PATH=/usr \
    -Dodb_api_DIR=/usr/share/odb_api/cmake \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=production \
    -DCMAKE_INSTALL_DATADIR=/usr/share \
    -DENABLE_METVIEW=1 \
    -DENABLE_QT5=1 \
    -DPYTHON_EXECUTABLE=/usr/bin/python3 \
    ..

  # sed -i magics.pc-pkg-config-build.cmake -e '/REPLACE/s/++/\\\\+\\\\+/'

  make -C build
}

package() {
  make -C build DESTDIR="$pkgdir" install
}

