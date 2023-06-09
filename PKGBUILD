# Maintainer: Julian Pollinger <julian@pollinger.dev>

pkgname=mattermost-desktop-git
_pkgname=mattermost-desktop
pkgver=5.2.2
pkgrel=1
pkgdesc="Mattermost Desktop for Linux (binary)"
arch=('x86_64')

url="https://github.com/mattermost/desktop"
license=('Apache')

makedepends=('nodejs' 'npm' 'asar' 'rpm-tools' 'git')
depends=('alsa-lib' 'gtk3' 'libnotify' 'nss' 'libxss' 'libxtst' 'xdg-utils' 'libutil-linux' 'libappindicator-gtk3' 'libsecret' 'libxcrypt-compat')
optdepends=()

conflicts=('mattermost-desktop')
provides=("${_pkgname}")

source=('git+https://github.com/mattermost/desktop.git#branch=release-5.2' ${_pkgname}.desktop)
sha256sums=('SKIP' '9e60ac9cc5a9cbebccb4180e7de947968aa49858812b5623812a1ab651a91093')

package() {
    cd 'desktop'

    npm ci

    NODE_ENV=production npm run build

    npm run package:linux-all-x64

    cd release/linux-unpacked
    install -d -m 755 "${pkgdir}"/usr/lib/mattermost

    cp -r * "$pkgdir/usr/lib/mattermost"

    cd "$pkgdir/usr/lib/mattermost"
    npx asar extract-file "$pkgdir/usr/lib/mattermost/resources/app.asar" assets/linux/icon.svg

    install -d -m 755 "$pkgdir/usr/bin"
    ln -s /usr/lib/mattermost/${_pkgname} "$pkgdir/usr/bin/$_pkgname"

    install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$_pkgname/LICENSE"

    install -Dm644 "$srcdir/$_pkgname.desktop" "$pkgdir/usr/share/applications/$_pkgname.desktop"
    install -Dm644 "$pkgdir/usr/lib/mattermost/icon.svg" "$pkgdir/usr/share/pixmaps/$_pkgname.svg"
}

pkgver() {
  # cutting off 'v' prefix that presents in the git tag
  git -C desktop describe --long --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}
