# Maintainer: Philip J. Repko <your@email.com>

pkgname=sqlch-suite
pkgver=0.1.0
pkgrel=1
pkgdesc="A modular internet radio CLI+GUI suite featuring a tray icon, TUI, and mpv controller"
arch=('any')
url="https://github.com/SW-philip/sqlch-suite"
license=('MIT')
depends=('python' 'python-gobject' 'gtk3' 'libayatana-appindicator' 'bash' 'hicolor-icon-theme' 'glib2')
optdepends=('mpv: used by sqlchctl for playback')
makedepends=('git')
source=(
  "sqlchctl::https://raw.githubusercontent.com/SW-philip/sqlch-suite/v$pkgver/sqlchctl"
  "sqlchknob::https://raw.githubusercontent.com/SW-philip/sqlch-suite/v$pkgver/sqlchknob"
  "sqlchtray::https://raw.githubusercontent.com/SW-philip/sqlch-suite/v$pkgver/sqlchtray")
md5sums=('e7df033a05f42d073e8ce3ede5baac2c'
         'ae8219cbaba49c298a859673ecf41402'
         '87a70b0c11bc530c421aca25df218c10')

package() {
  install -Dm755 sqlchctl "$pkgdir/usr/bin/sqlchctl"
  install -Dm755 sqlchknob "$pkgdir/usr/bin/sqlchknob"
  install -Dm755 sqlchtray.py "$pkgdir/usr/bin/sqlchtray"
  install -Dm644 sqlchtray.service "$pkgdir/usr/lib/systemd/user/sqlchtray.service"
  install -Dm644 sqrrlch_icon.png "$pkgdir/usr/share/icons/hicolor/64x64/apps/sqrrlch_icon.png"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
