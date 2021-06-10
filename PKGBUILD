# Maintainer : bakayuuko <33942350+bakayuuko@users.noreply.github.com>

pkgname=ncmpcpp-yt-dl-git
pkgver=0.9.2.r9.g35f9262a
pkgrel=1
epoch=2
pkgdesc='An almost exact clone of ncmpc with some new features (git version) with youtube-dl patch'
arch=('x86_64')
url='https://rybczak.net/ncmpcpp/'
license=('GPL')
depends=('curl' 'libmpdclient' 'taglib' 'ncurses' 'fftw' 'boost-libs' 'youtube-dl')
makedepends=('git' 'boost')
provides=('ncmpcpp')
conflicts=('ncmpcpp')
source=('git+https://github.com/ncmpcpp/ncmpcpp.git'
        '010-ncmpcpp-use-arch-flags.patch'
        'yt-dl.diff')
sha256sums=('SKIP'
            'SKIP'
            'SKIP')

prepare() {
    patch -d ncmpcpp -Np1 -i "${srcdir}/010-ncmpcpp-use-arch-flags.patch"
    patch -d ncmpcpp -Np1 -i "${srcdir}/yt-dl.diff"
    ncmpcpp/autogen.sh
}

pkgver() {
    git -C ncmpcpp describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^v//'
}

build() {
    cd ncmpcpp
    export BOOST_LIB_SUFFIX=''
    
    # http://site.icu-project.org/download/61#TOC-Migration-Issues
    export CPPFLAGS+=' -DU_USING_ICU_NAMESPACE=1'
    
    ./configure \
        --prefix='/usr' \
        --enable-clock \
        --enable-outputs \
        --enable-unicode \
        --enable-visualizer \
        --with-curl \
        --with-fftw \
        --with-taglib
    make
    make -C extras
}

package() {
    make -C ncmpcpp DESTDIR="$pkgdir" install
    install -D -m755 ncmpcpp/extras/artist_to_albumartist -t "${pkgdir}/usr/bin"
}
