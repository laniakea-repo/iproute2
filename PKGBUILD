# Maintainer: Christian Hesse <mail@eworm.de>
# Maintainer: Ronald van Haren <ronald.archlinux.org>
# Contributor: Judd Vinet <jvinet@zeroflux.org>

pkgname=iproute2
pkgver=5.17.0
pkgrel=1
pkgdesc='IP Routing Utilities'
arch=('x86_64')
license=('GPL2')
url='https://git.kernel.org/pub/scm/network/iproute2/iproute2.git'
depends=('glibc' 'iptables' 'libelf')
optdepends=('db: userspace arp daemon'
            'libcap: tipc'
            'linux-atm: ATM support')
provides=('iproute')
backup=('etc/iproute2/ematch_map'
        'etc/iproute2/rt_dsfield'
        'etc/iproute2/rt_protos'
        'etc/iproute2/rt_realms'
        'etc/iproute2/rt_scopes'
        'etc/iproute2/rt_tables')
makedepends=('linux-atm')
options=('staticlibs')
validpgpkeys=('9F6FC345B05BE7E766B83C8F80A77F6095CDE47E') # Stephen Hemminger
source=("https://www.kernel.org/pub/linux/utils/net/${pkgname}/${pkgname}-${pkgver}.tar."{xz,sign}
        '0001-make-iproute2-fhs-compliant.patch')
sha256sums=('6e384f1b42c75e1a9daac57866da37dcff909090ba86eb25a6e764da7893660e'
            'SKIP'
            '758b82bd61ed7512d215efafd5fab5ae7a28fbfa6161b85e2ce7373285e56a5d')

prepare() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  # set correct fhs structure
  patch -Np1 -i "${srcdir}"/0001-make-iproute2-fhs-compliant.patch

  # do not treat warnings as errors
  sed -i 's/-Werror//' Makefile

}

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  export CFLAGS+=' -ffat-lto-objects'

  ./configure
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  make DESTDIR="${pkgdir}" SBINDIR="/usr/bin" install

  # libnetlink isn't installed, install it FS#19385
  install -Dm0644 include/libnetlink.h "${pkgdir}/usr/include/libnetlink.h"
  install -Dm0644 lib/libnetlink.a "${pkgdir}/usr/lib/libnetlink.a"
}

