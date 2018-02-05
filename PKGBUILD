# Maintainer: Benjamin Chrétien <chretien dot b plus aur at gmail dot com>
# Contributor: Fabio Loli <loli_fabio@protonmail.com>
# Contributor: Yuxin Wu <ppwwyyxxc@gmail.com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: hauptmech
# Contributor: figo.zhang
# Contributor: lubosz

pkgname=pcl-git
pkgver=1.8.1
pkgrel=2
pkgdesc="A comprehensive open source library for n-D Point Clouds and 3D geometry processing"
arch=('x86_64' 'i686')
url='http://www.pointclouds.org'
license=('BSD')
depends=('boost' 'eigen' 'flann' 'vtk' 'qhull' 'qt5-base' 'glu' 'qt5-webkit'
  'openmpi' 'python2' 'libxt' 'libharu' 'proj' 'glew')
makedepends=('cmake' 'gl2ps')
optdepends=('cuda' 'openni2' 'python2-sphinx')
source=(git+https://github.com/PointCloudLibrary/pcl)
sha256sums=(SKIP)
conflicts=(pcl)
provides=(pcl)

prepare() {
  cd "${srcdir}/pcl"

  # Patch to support boost 1.5.6:
  # 1. Api change. See http://www.pcl-users.org/PCL-compilation-errors-Please-help-me-td4035209.html
  # This patch is not necessary for now.
  # sed -i "s/boost::property_tree::xml_writer_settings.*/boost::property_tree::xml_writer_settings<std::string> settings = boost::property_tree::xml_writer_make_settings<std::string>('\\\t', 1);/g" io/src/lzf_image_io.cpp
  # 2. Qt moc parser fails on some boost headers files. Try to get around.
  grep -rl '#include <boost/date_time/posix_time/posix_time.hpp>' . \
    | xargs sed -i "s/#include <boost.*posix_time.hpp>/#ifndef Q_MOC_RUN\n\r#include <boost\\/date_time\\/posix_time\\/posix_time.hpp>\n\r#endif/g"

  # [[ -d build ]] && rm -r build
}

build() {
# [[ -d build ]] && rm -r build
#  mkdir -p build && cd build
 # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  cmake ${srcdir}/pcl \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DBUILD_apps=ON \
    -DBUILD_apps_cloud_composer=ON \
    -DBUILD_apps_in_hand_scanner=ON \
    -DBUILD_apps_modeler=ON \
    -DBUILD_apps_point_cloud_editor=ON \
    -DBUILD_examples=ON \
    -DBUILD_global_tests=OFF \
    -DBUILD_surface_on_nurbs=ON \
    -DBUILD_CUDA=ON \
    -DBUILD_cuda_io=ON \
    -DBUILD_cuda_apps=ON \
    -DBUILD_GPU=ON \
    -DCUDA_HOST_COMPILER=/usr/bin/gcc

#  cd "${srcdir}/pcl-pcl-${pkgver}/build"
  make -j1
}

package() {
  cd "${srcdir}/build"
  make DESTDIR=${pkgdir} install

  install -Dm644 ${srcdir}/pcl/LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
