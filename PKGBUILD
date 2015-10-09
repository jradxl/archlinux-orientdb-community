# Maintainer:  Neng Xu <neng2.xu2@gmail.com>
# Contributor: Tobias Quinn <tobias@tobiasquinn.com>
# Contributor: John Radley <jradxl [at] gmail [dot] com>

##
## This is an install from Orient's tar.gz, and does not build Java sources
##

pkgname=orientdb-community

## PKGBUILD:pkgver is not allowed to contain colons, hyphens or whitespace
pkgver=2.1.3
_pkgsuffix=
_pkgtmp=
pkgrel=1
#epoch=1
pkgdesc="The OrientDB Graph-Document NoSQL - Community Edition 2.1.3 GA 4th Oct 2015"
arch=('any')
license=('Apache')
url="http://www.orientdb.org"
depends=('sh' 'java-environment')
makedepends=('unzip')
conflicts=('orientdb' 'orientdb-git' 'orientdb-graphed-git' 'orientdb-graphed')
install=$pkgname.install
groups=()
checkdepends=()
optdepends=()
provides=()
replaces=()
backup=()
options=()
noextract=()
changelog=""

#
# Using Orient's explict tar.gz download for this version.
# server2.sh and shutdown2.sh are versions more suited to systemd usage.
#
source=(
  "http://orientdb.com/download.php?email=unknown@unknown.com&file=orientdb-community-2.1.3.tar.gz" 
  'orientdb.service'
  'server2.sh'
  'shutdown2.sh'
)

md5sums=('4c90eec15ae214addd19d8aa8812e99d'
         'bfc3672cffd97b2f13d65009b8f1b232'
         '4220a44d3456840d6df8a9954728e3ed'
         'd5cb0292f84abc048062c8b4585b6204')

#prepare() {}

#No Build required
#build() {}

#check() {}

package() 
{    

  cd "${srcdir}"/${pkgname}-${pkgver}
  
  # Create directories with permissions
  install -dm755 "${pkgdir}"/opt/orientdb
  install -dm700 "${pkgdir}"/opt/orientdb/benchmarks
  install -dm755 "${pkgdir}"/opt/orientdb/bin
  install -dm700 "${pkgdir}"/opt/orientdb/config
  install -dm700 "${pkgdir}"/opt/orientdb/databases
  install -dm755 "${pkgdir}"/opt/orientdb/lib
  install -dm755 "${pkgdir}"/opt/orientdb/log
  install -dm755 "${pkgdir}"/opt/orientdb/plugins
  install -dm700 "${pkgdir}"/opt/orientdb/www
  
  # Recursively copy files
  cp -r . "${pkgdir}"/opt/orientdb

  # Add the improved systemd server management scripts
  install -m700 "${srcdir}"/server2.sh "${pkgdir}"/opt/orientdb/bin
  install -m700 "${srcdir}"/shutdown2.sh "${pkgdir}"/opt/orientdb/bin

  # Set permissions on the executables
  # --no World permissions are intended, so namcap errors can be ignored.
  install -m700 bin/*.sh "${pkgdir}"/opt/orientdb/bin/
  install -m755 bin/console.sh "${pkgdir}"/opt/orientdb/bin/
    
  # Remove DOS bat files
  find "${pkgdir}"/opt/orientdb -type f -name "*.bat" -exec rm -f {} \;
  
  # Empty dirs for Install - namcap errors can be ignored.
  install -d "${pkgdir}"/usr/bin
  install -d "${pkgdir}"/var/log/orientdb
  install -d "${pkgdir}"/usr/lib/systemd/system

  # Instead of a patch
  sed -i 's/cd `dirname $0`/#cd `dirname $0`/' "${pkgdir}"/opt/orientdb/bin/console.sh
  
  sed -i 's|\.\./log|/opt/orientdb/log|' "${pkgdir}"/opt/orientdb/config/orientdb-server-log.properties
  sed -i 's|YOUR_ORIENTDB_INSTALLATION_PATH|/opt/orientdb|' "${pkgdir}"/opt/orientdb/bin/orientdb.sh
  sed -i 's|USER_YOU_WANT_ORIENTDB_RUN_WITH|orient|' "${pkgdir}"/opt/orientdb/bin/orientdb.sh

  #Prevent server.sh being run from root
  sed -i '/PRG="$0"/a if [[ $(id -u) -eq 0 ]] ; then echo "" ; echo "Please do not try to start Orientdb Server as root." ; exit 1 ; fi' "${pkgdir}"/opt/orientdb/bin/server.sh
  
  install -m644 "${srcdir}"/orientdb.service "${pkgdir}"/usr/lib/systemd/system/
}

