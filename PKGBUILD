# Maintainer: Tobias Powalowski <tpowa@archlinux.org>

pkgname=ndiswrapper-lts
_kernver=2.6.32-lts
pkgver=1.56
pkgrel=5
pkgdesc="Module for NDIS (Windows Network Drivers) drivers supplied by vendors. For LTS 2.6 kernel."
license=('GPL')
arch=(i686 x86_64)
url="http://ndiswrapper.sourceforge.net"
install="ndiswrapper.install"
depends=("ndiswrapper-utils=$pkgver" 'kernel26-lts>=2.6.32' 'kernel26-lts<2.6.33')
makedepends=('kernel26-lts-headers>=2.6.32' 'kernel26-lts-headers<2.6.33')
source=(http://downloads.sourceforge.net/sourceforge/ndiswrapper/ndiswrapper-$pkgver.tar.gz)
options=(!strip)
md5sums=('1431f7ed5f8e92e752d330bbb3aed333')

build()
{
  cd $srcdir/ndiswrapper-$pkgver/driver
  make KVERS=$_kernver
}

package() {
  cd $srcdir/ndiswrapper-$pkgver/driver
  make DESTDIR=$pkgdir KVERS=$_kernver install
  rm $pkgdir/lib/modules/$_kernver/modules.* #wtf?

  sed -i -e "s/KERNEL_VERSION='.*'/KERNEL_VERSION='${_kernver}'/" $startdir/*.install
  # move it to correct kernel directory
  mkdir -p $pkgdir/lib/modules/$_kernver/kernel/drivers/net/wireless/ndiswrapper
  mv $pkgdir/lib/modules/$_kernver/misc/* $pkgdir/lib/modules/$_kernver/kernel/drivers/net/wireless/ndiswrapper/
  rm -r $pkgdir/lib/modules/$_kernver/misc/
  # gzip -9 modules
  find "$pkgdir" -name '*.ko' -exec gzip -9 {} \;
}

