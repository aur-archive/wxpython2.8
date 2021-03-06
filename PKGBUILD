# $Id$
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=wxpython2.8
pkgver=2.8.12.1
pkgrel=1
pkgdesc="A wxWidgets GUI toolkit for Python"
arch=('i686' 'x86_64')
license=('custom:wxWindows')
url="http://www.wxpython.org"
depends=('wxgtk2.8' 'python2')
makedepends=('mesa')
source=(http://downloads.sourceforge.net/wxpython/wxPython-src-${pkgver}.tar.bz2 
        wxpython-cairo.patch wxpython-fpb_default_style.patch)
sha1sums=('05688dc03d61631750f5904273122bb40a2115f5'
          '420700b0a216b853352ffafd054f406a82a30bb3'
          'b832d628b8ff38ea598f404d133899f40d687a22')

build() {
  cd "${srcdir}/wxPython-src-${pkgver}"
  find . -type f -exec sed -i 's/env python/env python2/' {} \;
  mv wxPython/wx/tools/Editra/editra wxPython/wx/tools/Editra/Editra
  ./configure --prefix=/usr --libdir=/usr/lib --with-gtk=2 --with-opengl --enable-unicode \
    --enable-graphics_ctx --disable-optimize --enable-mediactrl \
    --with-regex=sys --with-libpng=sys --with-libxpm=sys --with-libjpeg=sys --with-libtiff=sys \
    --disable-precomp-headers
  cd "${srcdir}/wxPython-src-${pkgver}/wxPython"
  patch -p2 -i "${srcdir}/wxpython-cairo.patch"
  patch -p1 -i "${srcdir}/wxpython-fpb_default_style.patch"
  python2 setup.py WXPORT=gtk2 UNICODE=1 build
}

package() {
  cd "${srcdir}/wxPython-src-${pkgver}/wxPython"
  python2 setup.py WXPORT=gtk2 UNICODE=1 install --root="${pkgdir}"

  install -D -m644 ../docs/licence.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  #rename files to be compatible with mainline version
  cd $pkgdir/usr/bin

  local v=2.8
  for i in ./*; do mv $i $i-$v; done
  cd $pkgdir/usr/lib/python2.*/site-packages
  mv wx{,-$v}.pth
  mv wxversion{,-$v}.py
  mv wxversion{,-$v}.pyc
}
