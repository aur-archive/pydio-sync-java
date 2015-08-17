#Maintainer: max-k <max-k AT post DOT com>

if [ $(uname -m) = "x86_64" ]; then
  _arch="x86_64"
else
  _arch="x86"
fi

pkgname=pydio-sync-java
_sdkname=pydio-sdk-java
pkgver=0.8.4
pkgrel=1
pkgdesc="Synchronization Client of the AjaXplorer project. Based on the Java Client."
arch=('i686' 'x86_64')
url="https://github.com/pydio/${pkgname}"
license=(GPL3)
depends=('java-environment')
makedepends=('git' 'maven')
source=("git://github.com/pydio/${pkgname}.git"
        "git://github.com/pydio/${_sdkname}.git"
        "pom.xml.patch"
        "pydio-sync"
        "pydio-sync-java.desktop")
md5sums=('SKIP'
         'SKIP'
         '425563c6baba34602fade72acc821fed'
         'eb8d67c3ca07a7e2798f874c9380d254'
         '7a0e2efcdf6012c231c256fb01ed8173')

build() {
  mkdir -p ${srcdir}/.m2/repository
  local _MVNOPTS="-X -Dmaven.repo.local=${srcdir}/.m2/repository"
  cd ${srcdir}/${_sdkname}
  git checkout sync-${pkgver}
  mvn ${_MVNOPTS} clean install -U || return 1
  cd ${srcdir}/${pkgname}/java
  git checkout ${pkgver}
  patch -p0 -i ${srcdir}/pom.xml.patch pom.xml
  mvn ${_MVNOPTS} clean install -P linux_${_arch} -U || return 1
}

package() {
  cd ${srcdir}/${pkgname}/java
  install -Dm644 res/images/AjxpLogo16-Bi.png ${pkgdir}/usr/share/icons/pydio-sync-java.png
  cd ${srcdir}/${pkgname}/java/target/linux_${_arch}
  install -d ${pkgdir}/usr/share/applications
  install -Dm644 pydio-sync-${pkgver}-SNAPSHOT.jar ${pkgdir}/usr/share/${pkgname}/pydio-sync.jar 
  install -Dm755 ${srcdir}/pydio-sync ${pkgdir}/usr/bin/${pkgname}
  install -Dm644 ${srcdir}/${pkgname}.desktop ${pkgdir}/usr/share/applications/
  cp -r PydioSync_lib ${pkgdir}/usr/share/${pkgname}/lib
  cp -r classes ${pkgdir}/usr/share/${pkgname}/
}
