# Maintainer: Tom Brown <tom@CarlsonSpeed.com>
pkgname='videokit-kde'
pkgver=0.1.0
pkgrel=1
pkgdesc="User-level KDE video utility suite for transcoding, metadata, etc."
arch=('any')
url="https://github.com/TomB16/VideoKit-KDE"
license=('MIT')
depends=('ffmpeg' 'mediainfo' 'bash' 'crudini')  # Add any runtime deps here
makedepends=('git')
source=("videokit-config"
        "videokit-file2title"
        "videokit-noforcedsubs"
        "videokit-queue"
        "videokit-s2hms"
        "videokit-title2file"
        "videokit-transcodefile"
        "videokit-transcodeprocess"
        "videokit-transcodequeue"
        "videokit.desktop"
        "videokit.conf"
        "LICENSE")
#source=("git+https://github.com/TomB16/VideoKit-KDE.git#commit=master")
#source=("$pkgname-r$pkgver::git://github.com/TomB16/VideoKit-KDE.git)
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')  # Don't need this when using Git as source

#pkgver() {
#  cd "$srcdir/$pkgname"
#  git describe --tags --always | sed 's/^v//;s/-/./g'
#}


package() {

  cd "$srcdir" || return 1
  echo "$srcdir" > output

  # Install all the Bash scripts into /usr/bin/
  install -Dm755 videokit-config            "$pkgdir/usr/bin/videokit-config"
  install -Dm755 videokit-file2title        "$pkgdir/usr/bin/videokit-file2title"
  install -Dm755 videokit-noforcedsubs      "$pkgdir/usr/bin/videokit-noforcedsubs"
  install -Dm755 videokit-queue             "$pkgdir/usr/bin/videokit-queue"
  install -Dm755 videokit-s2hms             "$pkgdir/usr/bin/videokit-s2hms"
  install -Dm755 videokit-title2file        "$pkgdir/usr/bin/videokit-title2file"
  install -Dm755 videokit-transcodefile     "$pkgdir/usr/bin/videokit-transcodefile"
  install -Dm755 videokit-transcodeprocess  "$pkgdir/usr/bin/videokit-transcodeprocess"
  install -Dm755 videokit-transcodequeue    "$pkgdir/usr/bin/videokit-transcodequeue"

  # Install .desktop file (for KDE integration)
  install -Dm644 videokit.desktop                   "$pkgdir/usr/share/kio/servicemenus/videokit.desktop"

  # Install configuration file (if it's part of the app)
  install -Dm644 videokit.conf                      "$pkgdir/usr/share/videokit/videokit.conf"

  # Install LICENSE file
  install -Dm644 LICENSE                            "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Rebuild KDE service cache
  if command -v kbuildsycoca5 &> /dev/null; then kbuildsycoca5; fi
  if command -v kbuildsycoca6 &> /dev/null; then kbuildsycoca6; fi

}
