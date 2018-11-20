_pkgname=benchmark
pkgname=mingw-w64-benchmark
pkgver=1.4.1
pkgrel=1
pkgdesc='A microbenchmark support library, by Google (mingw-w64)'
arch=(any)
url='https://github.com/google/benchmark'
license=('APACHE')
depends=("mingw-w64-gcc")
makedepends=("mingw-w64-gcc"
             "mingw-w64-cmake"
             "mingw-w64-gtest")
options=(!strip !buildflags staticlibs)
_sourcename=${_pkgname}-${pkgver}
source=("${_pkgname}-${pkgver}.tar.gz::https://github.com/google/benchmark/archive/v${pkgver}.tar.gz")
sha256sums=('f8e525db3c42efc9c7f3bc5176a8fa893a9a9920bbd08cef30fb56a51854d60d')
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"
prepare() {
  find ${_sourcename} -name *.cc -exec sed -i -e "s/Windows.h/windows.h/g" -e "s/Shlwapi.h/shlwapi.h/g" -e "s/VersionHelpers.h/versionhelpers.h/g" {} \;
}
build() {
  cd ${_sourcename}

  for _arch in ${_architectures}; do
    mkdir -p build-${_arch}
    pushd build-${_arch}
    ${_arch}-cmake .. \
      -DCMAKE_BUILD_TYPE=Release \
      -DREGISTER_INSTALL_PREFIX=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DBUILD_STATIC_LIBS=ON \
      -DBUILD_TESTING=ON \
      -DBENCHMARK_ENABLE_GTEST_TESTS=OFF \
      -DCMAKE_SHARED_LIBRARY_LINK_C_FLAGS="" \
      -DCMAKE_SHARED_LIBRARY_LINK_CXX_FLAGS="" \
      -DHAVE_STD_REGEX=0
    find ../.. -exec sed -i "s/Shlwapi/shlwapi/g" {} \; -print 2>/dev/null
    make
    popd
  done
}
package() {
  cd ${_sourcename}
  for _arch in ${_architectures}; do
    pushd build-${_arch}
    make DESTDIR=${pkgdir} install
    popd
  done
}

