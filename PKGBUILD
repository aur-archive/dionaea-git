# Maintainer: Gagou <gagou@rez-gif.supelec.fr>
pkgname=dionaea-git
pkgver=20140623
pkgrel=1
pkgdesc="A nepenthes successor"
url="http://dionaea.carnivore.it"
license=(GPL)
arch=('i686' 'x86_64' 'armv6h')
depends=('libnl' 'libev' 'cython' 'libpcap' 'libxml2' 'libxslt' 'python-lxml' 'curl' 'udns' 'glib2' 'gc' 'libemu' 'liblcfg' 'sqlite')
makedepends=('git')
source=("dionaea::git://git.carnivore.it/dionaea.git"
        dionaea.service
	makefiles.patch)
install=$pkgname.install
md5sums=('SKIP'
         '7783964ba51bc7c3912ddf84737d55fc'
         'e821ffdd91d995c0577e08ccba502cf3')


build() {  
  cd "$srcdir/dionaea"

  echo -e "\\033[1;31m" "WARNING : libnl1 must NOT be installed. Only libnl must be installed." "\\033[0;39m"

  # Correct python modules install paths
  patch -p1 < $srcdir/makefiles.patch
  sed -i s/AM_CONFIG_HEADER/AC_CONFIG_HEADERS/g ./configure.ac  
  autoreconf -vi
  ./configure --with-python=/usr/bin/python3 --with-lcfg-lib=/usr/lib/liblcfg/ --with-lcfg-include=/usr/include/ --with-emu-include=/usr/include/ --with-emu-lib=/usr/lib/libemu/ --disable-werror || return 1
  # fix compilation with libcrypto
  sed -i "/^LIB_SSL_LIBS/s/-lssl/-lssl -lcrypto/" src/Makefile
  make || return 1
}

package() {
  cd "$srcdir/dionaea"
  make DESTDIR=$pkgdir install
  install -D -m755 $startdir/dionaea.service $pkgdir/etc/systemd/system/dionaea.service
}

