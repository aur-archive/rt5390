# Maintainer: Zuyi Hu <hzy068808@gmail.com>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

module=rt5390sta
pkgname=rt5390
pkgver=2.6.0.0_r1
_patchrel=${pkgver/*_r/}
pkgrel=1
pkgdesc="Ralink RT5390 PCI WLAN adaptors kernel module"
arch=(i686 x86_64)
url="http://www.mediatek.com/en/products/connectivity/wifi/pc/pcie/rt5390/"
license=('GPL')
depends=('linux>=3.14')
makedepends=('linux-headers')
install=$module.install
source=($module-$pkgver.zip::https://github.com/hzy199411/rt5390sta-linux/archive/r${_patchrel}.tar.gz)

build() {
	_kernver=$(pacman -Q linux | sed -r 's#.* ([0-9]+\.[0-9]+).*#\1#')
	KERNEL_RELEASE=$(cat /usr/lib/modules/extramodules-$_kernver-ARCH/version)

	cd "$srcdir"/rt5390sta-linux-r${_patchrel}

	EXTRA_CFLAGS="-DVERSION=$pkgver" \
	LINUX_SRC="/usr/lib/modules/$KERNEL_RELEASE/build" \
		make
}

package() {
	_kernver=$(pacman -Q linux | sed -r 's#.* ([0-9]+\.[0-9]+).*#\1#')
	depends=("linux>=3.14")
	KERNEL_VERSION=$(cat /usr/lib/modules/extramodules-$_kernver-ARCH/version)
	msg "Kernel = $KERNEL_VERSION"

	cd "$srcdir"/rt5390sta-linux-r${_patchrel}

	install -Dm 0640 RT5390STA.dat "$pkgdir/etc/Wireless/RT5390STA/RT5390STA.dat"
	install -Dm 0644 os/linux/$module.ko "$pkgdir/usr/lib/modules/extramodules-$_kernver-ARCH/$module.ko"
	install -dm 0755 "$pkgdir/usr/share/doc/$module"
	install -m 0644 iwpriv_usage.txt README* RT5390STA* sta_ate_iwpriv_usage.txt "$pkgdir/usr/share/doc/$module"

	find "$pkgdir" -name '*.ko' -exec gzip -9 {} \;
	sed -i "s|extramodules-.*-ARCH|extramodules-$_kernver-ARCH|" "$startdir/$module.install"
}

md5sums=('130ea336cba561ac68b8dfbfd531227d')
