# Maintainer: Frank Siegert <frank.siegert@googlemail.com>
# Contributor: bartus <arch-user-repoᘓbartus.33mail.com>
pkgname=openboard-develop-git
pkgver=1.5.3
pkgrel=3
pkgdesc="Interactive whiteboard software for schools and universities"
arch=('x86_64' 'i686')
url="http://openboard.ch/index.en.html"
install=openboard.install
license=('GPL3')
depends=('qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-script' 'qt5-webkit' 'qt5-tools' 'qt5-xmlpatterns' 'libpaper' 'bzip2' 'openssl' 'libfdk-aac' 'sdl' 'ffmpeg')
depends+=(quazip)  #drop internal quazip and use system one.
depends+=(poppler) #replace internal xpdf with poppler and drop freetype/xpdf from deps
source=("https://github.com/OpenBoard-org/OpenBoard/archive/v$pkgver.tar.gz"
        qchar.patch
        qwebkit.patch
        https://github.com/OpenBoard-org/OpenBoard/pull/218.diff
        https://github.com/OpenBoard-org/OpenBoard/pull/223.diff
        openboard.desktop)
source+=(quazip.diff)
source+=(poppler.patch)
source+=(drop_ThirdParty_repo.patch)
md5sums=('fe3644033dccfd16c80b683210e4ac57'
         'bf2c524f3897cfcfb4315bcd92d4206e'
         '60f64db6bf627015f4747879c4b30fd3'
         'f484614cc48181287607afb5a45ef644'
         '04c421c140e983d41975943ede5fe61a'
         '21d1749400802f8fc0669feaf77de683'
         '30a7928f696f958d5e8f06e02c49639f'
         '8b774d204501bb8515ee224651a7d624'
         '879116c683374b2dde291014e44a29fe')

prepare() {
  cd $srcdir/OpenBoard-$pkgver
  patch -p1 < $srcdir/qchar.patch
  patch -p1 < $srcdir/qwebkit.patch
  patch -p1 < $srcdir/218.diff
  patch -p1 < $srcdir/223.diff
  patch -p1 < $srcdir/quazip.diff
  patch -p1 < $srcdir/poppler.patch
  patch -p1 < $srcdir/drop_ThirdParty_repo.patch
}

build() {
  cd "$srcdir/OpenBoard-$pkgver"
  qmake OpenBoard.pro -spec linux-g++
  make
}

package() {
  cd "$srcdir/OpenBoard-$pkgver"

  mkdir -p $pkgdir/opt/openboard

  for i in customizations etc i18n library; do
    cp -rp $srcdir/OpenBoard-$pkgver/resources/$i $pkgdir/opt/openboard;
  done

  cp -rp $srcdir/OpenBoard-$pkgver/resources/images/OpenBoard.png $pkgdir/opt/openboard/
  cp -rp build/linux/release/product/OpenBoard $pkgdir/opt/openboard/

  install -D -m 644 $srcdir/openboard.desktop $pkgdir/usr/share/applications/openboard.desktop
  install -d -m 755 $pkgdir/usr/bin
  ln -s /opt/openboard/OpenBoard $pkgdir/usr/bin/openboard
}
