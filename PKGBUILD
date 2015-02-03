pkgname=wesnoth-data
pkgver=1.12.1
pkgrel=1
pkgdesc="A turn-based strategy game on a fantasy world (data files)"
arch=('x86_64')
license=('GPL')
url="http://www.wesnoth.org/"
makedepends=('boost' 'cmake' 'sdl_image' 'sdl1_ttf' 'sdl_mixer' 'sdl_net' 'lua' 'pango' 'git')
options=(!emptydirs)
source=("git://github.com/wesnoth/wesnoth.git#tag=${pkgver}"
        "wesnoth-boost.patch")
md5sums=('SKIP'
         '3f531eb34e6a2a0d5aea68c68dafcf7f')

build() {
  cd "${srcdir}/wesnoth"

  # Try this again in a new version when they fix their linking to boost
  patch -Np1 < ${srcdir}/wesnoth-boost.patch

  mkdir build && cd build
  cmake .. \
      -DCMAKE_INSTALL_PREFIX=/usr \
      -DENABLE_OMP=ON \
      -DENABLE_TOOLS=ON \
      -DMANDIR=share/man \
      -DFIFO_DIR=/var/run/wesnothd
  make
}

package() {
  cd "${srcdir}/wesnoth"

  cd build
  make DESTDIR="$pkgdir" install

  rm -r $pkgdir/usr/bin
  rm -r $pkgdir/usr/share/man

  for file in $(grep -l "python" $pkgdir/usr/share/wesnoth/data/tools/*); do
    sed -i "s|#!/usr/bi#n/env python|#!/usr/bin/env python2|" $file;
  done
}