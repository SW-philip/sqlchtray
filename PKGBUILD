pkgname=sqlch-suite
pkgver=0.1.0
pkgrel=1
arch=('any')
url="https://github.com/SW-philip/sqlch-suite"
license=('MIT')
depends=('bash' 'glib2' 'gtk3' 'hicolor-icon-theme' 'libayatana-appindicator' 'python' 'python-gobject' 'mpv')
makedepends=('git')
source=(
  "sqlchctl::https://raw.githubusercontent.com/SW-philip/sqlch-suite/v${pkgver}/sqlchctl"
  "sqlchknob::https://raw.githubusercontent.com/SW-philip/sqlch-suite/v${pkgver}/sqlchknob"
  "sqlchtray::https://raw.githubusercontent.com/SW-philip/sqlch-suite/v${pkgver}/sqlchtray"
  "LICENSE"
  "README.md"
)
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP') # optional: use actual sums later

package() {
  install -Dm755 sqlchctl "$pkgdir/usr/bin/sqlchctl"
  install -Dm755 sqlchknob "$pkgdir/usr/bin/sqlchknob"
  install -Dm755 sqlchtray.py "$pkgdir/usr/bin/sqlchtray"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}
