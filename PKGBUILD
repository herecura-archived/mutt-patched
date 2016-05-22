# Maintainer: Oscar Morante <oscar@morante.eu>
# Contributor: tobias [tobias [at] archlinux.org]
# Contributor: Gaetan Bisson <bisson@archlinux.org>

pkgname=mutt-patched
pkgver=1.6.1
pkgrel=2
pkgdesc='Small but very powerful text-based mail client (using -patched from debian)'
url='http://www.mutt.org/'
license=('GPL')
backup=('etc/Muttrc')
arch=('i686' 'x86_64')
optdepends=('smtp-forwarder: to send mail')
depends=('gpgme' 'ncurses' 'gnutls' 'libsasl' 'libidn' 'mime-types'
         'krb5' 'tokyocabinet')
makedepends=( 'w3m' 'docbook-xsl' )
conflicts=('mutt')
provides=('mutt')
source=(
  "https://bitbucket.org/mutt/mutt/downloads/mutt-${pkgver}.tar.gz"
  "http://http.debian.net/debian/pool/main/m/mutt/mutt_1.6.1-1.debian.tar.xz"
)
sha256sums=('98b26cecc6b1713082fc880344fa345c20bd7ded6459abe18c84429c7cf8ed20'
            '0d610b4fa5f6512cb68c80c70041c291df3ecfe5cacd0b455269be444cef14a3')

prepare() {
  cd "${srcdir}/mutt-$pkgver"

  IFS=$'\n'
  for patch in $(cat "$srcdir/debian/patches/series"); do
      patch -p1 -i "$srcdir/debian/patches/$patch"
  done

  autoreconf -vfi
}

build() {
  cd "${srcdir}/mutt-${pkgver}"

  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --mandir=/usr/share/man \
    --with-docdir=/usr/share/doc \
    --with-mailpath=/var/mail \
    --disable-dependency-tracking \
    --enable-compressed \
    --enable-debug \
    --enable-fcntl \
    --enable-gpgme \
    --enable-hcache \
    --enable-imap \
    --enable-pop \
    --enable-smtp \
    --with-curses \
    --with-gss \
    --with-idn \
    --with-mixmaster \
    --without-bdb \
    --without-gdbm \
    --without-qdbm \
    --with-regex \
    --with-sasl \
    --with-gnutls \
    --enable-sidebar \
    --enable-nntp

  make
}

package() {
  cd "${srcdir}/mutt-${pkgver}"
  make DESTDIR="${pkgdir}" install

  rm "${pkgdir}"/etc/mime.types{,.dist}
  rm "${pkgdir}"/usr/bin/{flea,muttbug}
  rm "${pkgdir}"/usr/share/man/man1/{flea,muttbug}.1
  install -D -m 644 contrib/gpg.rc "$pkgdir"/etc/Muttrc.gpg.dist
}
