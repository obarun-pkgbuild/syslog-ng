# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/syslog-ng
# 						Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=syslog-ng
pkgver=3.10.1
pkgrel=3
pkgdesc="Next-generation syslogd with advanced networking and filtering capabilities"
arch=(x86_64)
url="http://www.balabit.com/network-security/syslog-ng/"
license=('GPL2' 'LGPL2.1')
depends=('awk' 'eventlog' 'glib2' 'libdbi')
makedepends=('python2' 'libxslt' 'json-c' 'libmongoc')
optdepends=('logrotate: for rotating log files'
	    'json-c: for json-plugin'
	    'curl: for the HTTP module'
	    'libmongoc: for the MongoDB plugin'
	    'syslog-ng-runitserv: syslog service for runit')
backup=('etc/syslog-ng/scl.conf'
        'etc/syslog-ng/syslog-ng.conf'
        'etc/logrotate.d/syslog-ng')
source=(https://github.com/balabit/syslog-ng/releases/download/syslog-ng-$pkgver/$pkgname-$pkgver.tar.gz
        syslog-ng.conf syslog-ng.logrotate)
sha1sums=('2325da17d9b6837aae8484ef3b8d5a25ee992ee9'
          '09b4ae56d083fdae3f8310f0a374dfe7f339a5bf'
          '949128fe3d7f77a7aab99048061f885bc758000c')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal


build() {
  cd ${pkgname}-$pkgver
  ./configure 	--prefix=/usr \
				--sysconfdir=/etc/syslog-ng \
				--libexecdir=/usr/lib \
				--sbindir=/usr/bin \
				--localstatedir=/var/lib/syslog-ng \
				--datadir=/usr/share/ \
				--with-pidfile-dir=/run \
				--disable-spoof-source \
				--enable-ipv6 \
				--enable-sql \
				--enable-systemd=no \
				--with-systemdsystemunitdir=no \
				--enable-manpages \
				--with-jsonc=system \
				--with-mongoc=system
  make
}

check() {
  cd ${pkgname}-$pkgver
  make check
}

package() {
  make -C ${pkgname}-$pkgver DESTDIR="$pkgdir" install
  install -dm755 "$pkgdir/var/lib/syslog-ng" "$pkgdir/etc/syslog-ng/patterndb.d"
  install -Dm644 "$srcdir/syslog-ng.conf" "$pkgdir/etc/syslog-ng/syslog-ng.conf"
  install -Dm644 "$srcdir/syslog-ng.logrotate" "$pkgdir/etc/logrotate.d/syslog-ng"
  
  # See http://lists.balabit.hu/pipermail/syslog-ng/2016-February/022667.html
  rm -r "$pkgdir"/usr/share/syslog-ng/include/scl/cim
}
