# Maintainer: Gabriel Chamon Araujo <gchamon@live.com>
# Contributor: Anton Bazhenov <anton.bazhenov at gmail>
# Contributor: Graziano Giuliani <graziano.giuliani@poste.it>
# Contributor: Antonio Cervone <ant.cervone@gmail.com>

pkgname=magics++
Pkgname=Magics
pkgver=4.6.0
_attnum=3473464
pkgrel=0
pkgdesc="Magics is the latest generation of the ECMWF's Meteorological plotting software MAGICS."
arch=('i686' 'x86_64')
url="https://software.ecmwf.int/wiki/display/MAGP"
license=('Apache')
depends=('qt5-base' 'fftw' 'pango' 'netcdf-cxx-legacy' 'eccodes' 'python' 'libgeotiff')
optdepends=('libaec' 'odb_api')
makedepends=('perl-xml-parser' 'gcc-fortran' 'swig' 'cmake' 'boost' 'emos' 'python-jinja')
source=(http://software.ecmwf.int/wiki/download/attachments/${_attnum}/${Pkgname}-${pkgver}-Source.tar.gz)
md5sums=('570d9888fc794b7c72c0a09fc9210dc3')

build() {
  cd "$srcdir/${Pkgname}-${pkgver}-Source"

  [ -x /usr/bin/odb ] && has_odb=ON || has_odb=OFF
  # force this for now. Metview does not compile
  has_odb=OFF

  mkdir -p build
  cd build

  cmake \
    -DCMAKE_LINKER_FLAGS="-pthread" \
    -DCMAKE_SHARED_LINKER_FLAGS="-pthread" \
    -DCMAKE_EXE_LINKER_FLAGS="-pthread" \
    -DENABLE_ODB=${has_odb} \
    -DGEOTIFF_PATH=/usr \
    -Dodb_api_DIR=/usr/share/odb_api/cmake \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=production \
    -DCMAKE_INSTALL_DATADIR=/usr/share \
    -DENABLE_METVIEW=1 -DENABLE_QT5=1 -DPYTHON_EXECUTABLE=/usr/bin/python3 ..
  make || return 1
}

package() {
  cd "$srcdir/${Pkgname}-${pkgver}-Source/build"
  make DESTDIR="$pkgdir" install
}

