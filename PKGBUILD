pkgname=sqlch-suite
pkgver=1.1
pkgrel=1
pkgdesc="A minimal analog suite for internet radio control and display (squelch, sqlchknob, sqlchtray)"
arch=('any')
url="https://github.com/SW-philip/sqlch-suite"
license=('MIT')
depends=('mpv' 'gtk3' 'libappindicator-gtk3' 'jq' 'curl' 'coreutils' 'bash' 'glib2' 'pavucontrol')
makedepends=('git')
source=("$pkgname-$pkgver.tar.gz::https://github.com/SW-philip/sqlch-suite/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('649a7d09d0cf80088b58931fc678f884abe14d2f1c63787eff69e2ba46e79eff')

package() {
  cd "$srcdir/sqlch-suite-$pkgver"
  install -Dm755 sqlchctl "$pkgdir/usr/bin/sqlchctl"
  install -Dm755 sqlchtray "$pkgdir/usr/bin/sqlchtray"
  install -Dm755 sqlchknob "$pkgdir/usr/bin/sqlchknob"
}
