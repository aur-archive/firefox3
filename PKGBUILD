# Maintainer: Ng Oon-Ee <ngoonee.talk@gmail.com>
# Source: firefox build in [extra]
# Contributor: Benjamin Hedrich <kiwisauce (a) pagenotfound (dot) de>

pkgname=firefox3
pkgver=3.6.28
pkgrel=1
pkgdesc="Standalone web browser from mozilla.org. This build installs in /opt and does not interfere with your system firefox (allowing for installing of a different version as your main browser)."
arch=('i686' 'x86_64')
license=('MPL' 'GPL' 'LGPL')
depends=('desktop-file-utils' 'libevent' 'hunspell' 'startup-notification' 'nspr' 'gtk2')
makedepends=('zip' 'pkg-config' 'diffutils' 'libgnomeui>=2.24.1' 'python2' 'wireless_tools' 'autoconf2.13' 'mesa')
install=firefox3.install
url="http://www.mozilla.org/projects/firefox"

source=(http://releases.mozilla.org/pub/mozilla.org/firefox/releases/${pkgver}/source/firefox-${pkgver}.source.tar.bz2
        mozconfig
        firefox3.desktop
        firefox3-safe.desktop
        mozilla-firefox-1.0-lang.patch
        browser-defaulturls.patch
        firefox-version.patch
        firefox-agent.patch
        python2.7.patch
        enable-x86_64-tracemonkey.patch
        mozilla-gcc46.patch
        xulrunner-version.patch
        xulrunner-png14.patch
        xulrunner-png15.patch)

md5sums=('175fea06e1af7c76992e23865e4456eb'
         'fb9cb2867c1a1efd6030e71b5167a3d5'
         '7918f4aa616cce0c2a51522eb8724a4e'
         '79c2b227d1874d9f54abd94dbd9e5f34'
         'bd5db57c23c72a02a489592644f18995'
         '1807651225b021e043154f8bba715a19'
         '92c11c66dd69b03f214002fededd1fc8'
         'f437e94acff8f810991271ef4677d859'
         'ab3dc9aecae7f08b9492fb3c00a5fd28'
         'cbd938cd1fb8210cd8a2c41833489af9'
         '7016f2fc8522a3730bc4e6c232d6f735'
         '371303c5bdc4fa0d955d14521b93b69d'
         'cf93e665cd54edeb98efaae73717bfc3'
         '909064cdbec1ab927918e6a1d4253d81')

build() {
  cd "${srcdir}/mozilla-1.9.2"
  patch -Np1 -i ${srcdir}/mozilla-firefox-1.0-lang.patch
  patch -Np1 -i ${srcdir}/browser-defaulturls.patch
  patch -Np1 -i ${srcdir}/firefox-version.patch
  patch -Np1 -i ${srcdir}/firefox-agent.patch
  patch -Np0 -i ${srcdir}/python2.7.patch
  patch -Np0 -i ${srcdir}/enable-x86_64-tracemonkey.patch
  patch -Np1 -i ${srcdir}/xulrunner-version.patch
  patch -Np0 -i ${srcdir}/xulrunner-png14.patch
  patch -Np0 -i ${srcdir}/xulrunner-png15.patch
  patch -Np2 -i ${srcdir}/mozilla-gcc46.patch

  cp "${srcdir}/mozconfig" .mozconfig
  unset CFLAGS
  unset CXXFLAGS

  export LDFLAGS="-Wl,-rpath,/usr/lib/firefox-3.6"
  export CXXFLAGS="-O2 -pipe -fpermissive"
  cp ${srcdir}/mozilla-1.9.2/security/coreconf/Linux{2.6,3.0}.mk

  autoconf-2.13

  make -j1 -f client.mk build MOZ_MAKE_FLAGS="${MAKEFLAGS}"
  make -j1 DESTDIR="${pkgdir}/opt" install

  rm -f ${pkgdir}/opt/usr/lib/firefox-3.6/libjemalloc.so
  rm -Rf ${pkgdir}/opt/usr/bin

  install -m755 -d ${pkgdir}/usr/share/{applications,pixmaps}
  install -m644 ${srcdir}/mozilla-1.9.2/browser/branding/unofficial/default48.png ${pkgdir}/usr/share/pixmaps/firefox3.png
  install -m644 ${srcdir}/firefox3.desktop ${pkgdir}/usr/share/applications/firefox3.desktop
  install -m644 ${srcdir}/firefox3-safe.desktop ${pkgdir}/usr/share/applications/firefox3-safe.desktop

  install -m755 -d ${pkgdir}/usr/bin
  ln -s /opt/usr/lib/firefox-3.6/firefox ${pkgdir}/usr/bin/${pkgname}
}
