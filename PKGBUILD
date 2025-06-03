# Maintainer: Philip J. Repko <your@email.com>

pkgname=sqlch-suite
pkgver=3.4.1
pkgrel=1
pkgdesc="A modular internet radio CLI+GUI suite featuring a tray icon, TUI, and mpv controller"
arch=('any')
url="https://github.com/SW-philip/sqlch-suite"
license=('MIT')
depends=('python' 'python-gobject' 'gtk3' 'libappindicator-gtk3' 'bash' 'hicolor-icon-theme' 'glib2')
optdepends=('mpv: used by sqlchctl for playback')
source=('sqlch'
        'sqlchctl'
        'sqlchknob'
        'sqlchtray.py'
        'sqlchtray.service'
        'sqlchtray-icon.png'
        'LICENSE')
md5sums=('SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP')

package() {
  install -Dm755 sqlch "$pkgdir/usr/bin/sqlch"
  install -Dm755 sqlchctl "$pkgdir/usr/bin/sqlchctl"
  install -Dm755 sqlchknob "$pkgdir/usr/bin/sqlchknob"
  install -Dm755 sqlchtray.py "$pkgdir/usr/bin/sqlchtray"
  install -Dm644 sqlchtray.service "$pkgdir/usr/lib/systemd/user/sqlchtray.service"
  install -Dm644 sqlchtray-icon.png "$pkgdir/usr/share/icons/hicolor/64x64/apps/sqlchtray-icon.png"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
