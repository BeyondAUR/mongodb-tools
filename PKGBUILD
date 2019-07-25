# Maintainer: James P. Harvey <jamespharvey20 at gmail dot com>
# Maintainer: Christoph Bayer <chrbayer@criby.de>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Fredy García <frealgagu at gmail dot com>

pkgname=mongodb-tools
pkgver=4.0.10
pkgrel=1
pkgdesc="The MongoDB tools provide import, export, and diagnostic capabilities."
arch=('x86_64')
url="https://github.com/mongodb/mongo-tools"
license=('Apache')
depends=('libpcap')
makedepends=('go-pie')
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/mongodb/mongo-tools/archive/r${pkgver}.tar.gz")
sha512sums=('12764b2e2016ae3ab3f0ed1f5b9be9ce10e466c53b408ad2c01b26bdf39ca41a358acd07aea5847db2b40e3e89293f77efcd2b310e4e2bf3071873abf1b20f49')

_tools=('bsondump' 'mongostat' 'mongofiles' 'mongoexport' 'mongoimport' 'mongorestore' 'mongodump' 'mongotop' 'mongoreplay')

prepare() {
  cd "${srcdir}"
  install -d build/src/github.com/mongodb/bin
  mv "mongo-tools-r${pkgver}" build/src/github.com/mongodb/mongo-tools
  sed -i 's/_Ctype_struct_/C.struct_/' build/src/github.com/mongodb/mongo-tools/vendor/github.com/google/gopacket/pcap/pcap.go
}

build() {
  cd "${srcdir}/build/src/github.com/mongodb/mongo-tools"
  GOROOT=/usr ./set_goenv.sh
  export GOPATH="$GOPATH:$srcdir/build"
  for tool in "${_tools[@]}"; do
    echo "Building ${tool}..."
    go build -o "bin/${tool}" -tags "ssl sasl" "${tool}/main/${tool}.go"
  done
}

package() {
  cd "${srcdir}/build/src/github.com/mongodb/mongo-tools"
  for tool in "${_tools[@]}"; do
    install -Dm755 "bin/${tool}" "${pkgdir}/usr/bin/${tool}"
  done
}
