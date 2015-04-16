# Maintainer: Scott Dose <sdose27@gmail.com>
pkgname=arch-automated-docker-git
_gitname=arch-automated-docker
pkgver=0.1
pkgrel=1
pkgdesc="Utility to automate the process of starting, stopping and removing docker containers"
arch=('any')
url="http://github.com/halosman/arch-automated-docker"
license=('GPL')
depends=('docker' 'sed')
makedepends=('git')
backup=(etc/automated-docker.conf)
source=('git+https://github.com/halosman/arch-automated-docker')
md5sums=('SKIP')

pkgver() {
	cd "$pkgname"
	git show -s --format="%ci" HEAD | sed -e 's/-//g' -e 's/ .*//'
}

package() {
	cd "$pkgname"
	install -D -m 775 start-container "$pkgdir/usr/bin/start-container"
	install -D -m 775 rm-container "$pkgdir/usr/bin/rm-container"
	install -D -m 775 stop-container "$pkgdir/usr/bin/stop-container"
	install -D -m 644 automated-docker@.service "$pkgdir/usr/lib/systemd/system/automated-docker.service"
	install -D -m 644 automated-docker.conf "$pgkdir/etc/automated-docker.conf"
}