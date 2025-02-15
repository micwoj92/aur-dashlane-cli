# Maintainer: Stephan Koglin-Fischer <stephan dot koglin-fischer at funzt dot dev>
pkgname=dashlane-cli-git
pkgver=6.2447.2.r8.g4235173
pkgrel=1
pkgdesc="Dashlane CLI GitHub repository version bundled with asdf-vm to ensure using the correct node version."
arch=('x86_64')
url="https://github.com/Dashlane/dashlane-cli"
license=('MIT')
makedepends=('git' 'yarn')
source=("$pkgname::git+$url.git")
b2sums=('SKIP')
# option strip is important for dcli to work properly and avoid
# 'Pkg: Error reading from file.' error in production
options=(!strip)

pkgver() {
  cd "$srcdir/$pkgname"
  git describe --long --tags --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir/$pkgname"
#  yarn install --immutable --immutable-cache --check-cache
  yarn install --immutable --check-cache
}

build() {
  cd "$srcdir/$pkgname"
  yarn run build
  # Build linux binary
  yarn pkg:linux
}

package() {
  cd "$srcdir/$pkgname"

  # Install the binary
  install -Dm755 "$srcdir/$pkgname/bundle/dcli-linux" "$pkgdir/usr/share/dashlane-cli-git/dcli"

  # Create a symlink to the binary
  mkdir -p "$pkgdir/usr/bin"
  cd "$pkgdir/usr/bin"
  ln -sf "./../share/dashlane-cli-git/dcli" "dcli"
}
