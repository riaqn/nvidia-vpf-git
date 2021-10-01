# Maintainer: Daniel Bermond <dbermond@archlinux.org>

pkgname=nvidia-vpf-git
pkgver=r232.gfad2753
pkgrel=1
pkgdesc='NVIDIA Video Processing Framework (git version)'
arch=('x86_64')
url='https://github.com/NVIDIA/VideoProcessingFramework/'
license=('Apache')
depends=('cuda' 'nvidia-utils' 'ffmpeg' 'python')
makedepends=('git' 'cmake' 'nvidia-sdk')
provides=('nvidia-vpf')
conflicts=('nvidia-vpf')
source=('git+https://github.com/NVIDIA/VideoProcessingFramework.git'
        '010-nvidia-vpf-git-fix-install.patch')
sha256sums=('SKIP'
            '568c79e802ce442369a57a21f79d8d8a31680054e71140d178e6106c1af00f9b')

prepare() {
    patch -d VideoProcessingFramework -Np1 -i "${srcdir}/010-nvidia-vpf-git-fix-install.patch"
}

pkgver() {
    cd VideoProcessingFramework
    printf 'r%s.g%s' "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    export CXXFLAGS+=' -I/opt/cuda/include'
    export  LDFLAGS+=' -L/opt/cuda/lib64'
    
    cmake -B build -S VideoProcessingFramework \
        -DCMAKE_BUILD_TYPE:STRING='None' \
        -DCMAKE_SKIP_RPATH:BOOL='YES' \
        -DCMAKE_INSTALL_PREFIX:PATH='/usr' \
        -DGENERATE_PYTHON_BINDINGS:BOOL='TRUE' \
        -DVIDEO_CODEC_SDK_INCLUDE_DIR:PATH='/usr/include/nvidia-sdk' \
        -Wno-dev
    make -C build
}

package() {
    make -C build DESTDIR="$pkgdir" install
}
