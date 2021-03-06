# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/syslog-ng
# 						Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=syslog-ng
pkgver=3.15.1
pkgrel=2
pkgdesc="Next-generation syslogd with advanced networking and filtering capabilities"
arch=(x86_64)
url="http://www.balabit.com/network-security/syslog-ng/"
license=('GPL2' 'LGPL2.1')
depends=('awk' 'glib2' 'libdbi' 'libnsl')
makedepends=('libxslt' 'json-c' 'mongo-c-driver' 'librabbitmq-c' 'python')
checkdepends=('python-nose' 'python-ply')
optdepends=('logrotate: for rotating log files'
	    'json-c: for json-plugin'
	    'curl: for the HTTP module'
	    'mongo-c-driver: for the MongoDB plugin'
	    'python: for the Python plugin')
conflicts=('eventlog')
replaces=('eventlog')
backup=('etc/syslog-ng/scl.conf'
        'etc/syslog-ng/syslog-ng.conf'
        'etc/logrotate.d/syslog-ng')
source=(https://github.com/balabit/syslog-ng/releases/download/syslog-ng-$pkgver/$pkgname-$pkgver.tar.gz
        syslog-ng.conf syslog-ng.logrotate)
sha1sums=('b2cb71bd455089b3e1e3e825cd09faea5685ae04'
          '9caec3d4e9d9b7299e42ef13dc5a89a99278ab6e'
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
				--with-mongoc=system \
				--with-librabbitmq-client=system
  make
}

check() {
  cd ${pkgname}-$pkgver
  make check || /bin/true # python test requires pep8
}

package() {
  make -C ${pkgname}-$pkgver DESTDIR="$pkgdir" install
  install -dm755 "$pkgdir/var/lib/syslog-ng" "$pkgdir/etc/syslog-ng/patterndb.d"
  install -Dm644 "$srcdir/syslog-ng.conf" "$pkgdir/etc/syslog-ng/syslog-ng.conf"
  install -Dm644 "$srcdir/syslog-ng.logrotate" "$pkgdir/etc/logrotate.d/syslog-ng"
  # this is not specific to systemd, the file can be sourced by any service file as classic bash operation
  install -Dm644 "$srcdir"/$pkgname-$pkgver/contrib/systemd/syslog-ng@default -t "$pkgdir"/etc/default
  
  # See http://lists.balabit.hu/pipermail/syslog-ng/2016-February/022667.html
  rm -r "$pkgdir"/usr/share/syslog-ng/include/scl/cim
}
