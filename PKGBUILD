# Contributor: Sébastien Luttringer <seblu@seblu.net>

pkgname=bundy
pkgver=1.2.0rc1.r282.gaf90337
pkgrel=3
pkgdesc='Next generation of ISC BIND and DHCP server'
arch=('i686' 'x86_64')
url='http://bundy-dns.de/'
license=('custom')
makedepends=('boost')
provides=('bind10')
replaces=('bind10')
depends=('boost-libs' 'python' 'botan' 'python-setproctitle' 'log4cplus')
backup=('etc/bundy/cmdctl-certfile.pem'
        'etc/bundy/cmdctl-keyfile.pem'
        'etc/bundy/cmdctl-accounts.csv')
# parallel builds are broken
options=('!makeflags')
# afaict python3 API changed at some point; no new upstream release
source=("$pkgname::git+https://github.com/bundy-dns/bundy.git"
        "bundy.service")
md5sums=('SKIP'
         '209404a76dc119092533e6d13989403b')

pkgver() {
  cd $pkgname
  git describe --long --tags | sed -r 's/([^-]*-g)/r\1/;s/-/./g;s/bind10.//g;s/release.//g'
}

prepare() {
  cd $pkgname
  autoreconf --install
}

build() {
  cd $pkgname
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --libexecdir=/usr/lib \
    --with-werror=no
  
    # depends on xfr/xfrout* which was refactored/removed in
    # 14a734f7bc9b3a180e05a3381012aad39c2695ff
    #--enable-experimental-resolver
  make
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install
  # symlink license
  install -dm755 "$pkgdir/usr/share/licenses/$pkgname"
  ln -s ../../doc/$pkgname/COPYING "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  install -Dm644 ../${pkgname}.service "$pkgdir/usr/lib/systemd/system/${pkgname}.service"
}

# vim:set ts=2 sw=2 et:
