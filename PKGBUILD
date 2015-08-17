# Maintainer: artoo <artoo@manjaro.org>

# file vars for easy update
_Cgit=git-daemon.confd
_Igit=git-daemon-r1.initd
_Cmy=conf.d-2.0
_Imy=init.d-2.0
_Csvn=svnserve.confd
_Isvn=svnserve.initd2
_CPgsql=postgresql.confd
_IPgsql=postgresql.init
_Iphp=php-fpm-r4.init

_gentoo_uri="http://sources.gentoo.org/cgi-bin/viewvc.cgi/gentoo-x86"

pkgbase=openrc-devel
pkgname=('git-openrc'
	'mysql-openrc'
	'postgresql-openrc'
	'subversion-openrc'
	'php-fpm-openrc')
pkgver=20150119
pkgrel=1
pkgdesc="OpenRC init scripts"
arch=('any')
url="https://github.com/manjaro/packages-openrc"
license=('GPL2')
groups=('openrc' 'openrc-devel')
conflicts=('openrc'
	  'openrc-git'
	  'openrc-arch-services-git'
	  'initscripts'
	  'systemd-sysvcompat')
source=("${_gentoo_uri}/dev-vcs/git/files/${_Cgit}"
	"${_gentoo_uri}/dev-vcs/git/files/${_Igit}"
	"${_gentoo_uri}/dev-db/mysql-init-scripts/files/${_Cmy}"
	"${_gentoo_uri}/dev-db/mysql-init-scripts/files/${_Imy}"
	"${_gentoo_uri}/dev-vcs/subversion/files/${_Csvn}"
	"${_gentoo_uri}/dev-vcs/subversion/files/${_Isvn}"
	"${_gentoo_uri}/dev-db/postgresql/files/${_CPgsql}"
	"${_gentoo_uri}/dev-db/postgresql/files/${_IPgsql}"
	"${_gentoo_uri}/dev-lang/php/files/${_Iphp}")

pkgver() {
  date +%Y%m%d
}

_shebang='s|#!/sbin/runscript|#!/usr/bin/openrc-run|'
_runpath='s|/var/run|/run|g'
_binpath='s|/usr/sbin|/usr/bin|g'

package_git-openrc() {
  pkgdesc="OpenRC git-daemon init script"
  depends=('git' 'openrc-core')
  backup=('etc/conf.d/git-daemon')
  install=git.install

  install -Dm755 "${srcdir}/${_Cgit}" "${pkgdir}/etc/conf.d/git-daemon"
  install -Dm755 "${srcdir}/${_Igit}" "${pkgdir}/etc/init.d/git-daemon"

  local _p1='s|/var/git|/srv/git|'
  sed -e "${_p1}" -i "${pkgdir}/etc/conf.d/git-daemon"
  sed -e "${_shebang}" -e "${_binpath}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/git-daemon"
}

package_mysql-openrc() {
  pkgdesc="OpenRC mysql init script"
  depends=('mysql' 'openrc-core')
  optdepends=('bind-openrc: bind initscript')
  backup=('etc/conf.d/mysql')
  install=mysql.install

  install -Dm755 "${srcdir}/${_Cmy}" "${pkgdir}/etc/conf.d/mysql"
  install -Dm755 "${srcdir}/${_Imy}" "${pkgdir}/etc/init.d/mysql"

  local _p1='s|/sbin/mysqld|/bin/mysqld|g'
  sed -e "${_shebang}" -e "${_p1}" -i "${pkgdir}/etc/init.d/mysql"
}

package_php-fpm-openrc() {
  pkgdesc="OpenRC php-fpm init script"
  depends=('php-fpm' 'openrc-core')
  optdepends=('apache-openrc: apache initscript'
	      'lighttp-openrc: lighttp initscript'
	      'nginx-openrc: nginx initscript')
  install=php-fpm.install

  install -Dm755 "${srcdir}/${_Iphp}" "${pkgdir}/etc/init.d/php-fpm"
  
  local _p1='s|/lib/${PHPSLOT}||g'
  local _p2='s|/etc/php/fpm-${PHPSLOT}|/etc/php|'
  local _p3='s|/run/php-fpm-${PHPSLOT}|/run/php-fpm|'
  local _p4='s|PHPSLOT=${SVCNAME#php-fpm-}||'
  local _p5='s|^.*${PHPSLOT}.*||'
  sed -e "${_shebang}" \
      -e "${_runpath}" \
      -e "${_p1}" \
      -e "${_p2}" \
      -e "${_p3}" \
      -e "${_p4}" \
      -e "${_p5}" \
      -i "${pkgdir}/etc/init.d/php-fpm"
}

package_postgresql-openrc() {
  pkgdesc="OpenRC postgresql init script"
  depends=('postgresql' 'openrc-core')
  backup=('etc/conf.d/postgresql')
  install=postgresql.install

  install -Dm755 "${srcdir}/${_CPgsql}" "${pkgdir}/etc/conf.d/postgresql"
  install -Dm755 "${srcdir}/${_IPgsql}" "${pkgdir}/etc/init.d/postgresql"

  local _p1='s|/@LIBDIR@/postgresql-@SLOT@||g'

  sed -e "${_shebang}" -e "${_p1}" -i "${pkgdir}/etc/init.d/postgresql"
}

package_subversion-openrc() {
  pkgdesc="OpenRC svnserve init script"
  depends=('subversion' 'openrc-core')
  backup=('etc/conf.d/svn')
  install=subversion.install

  install -Dm755 "${srcdir}/${_Csvn}" "${pkgdir}/etc/conf.d/svn"
  install -Dm755 "${srcdir}/${_Isvn}" "${pkgdir}/etc/init.d/svn"

  local _p1='s|/var/svn|/srv/svn|g'
  sed -e "${_p1}" -i ${pkgdir}/etc/conf.d/svn
  local _p2='s|-apache|-http|g' \
	_p3='s|/run/svnserve.pid|/run/svnserve/svnserve.pid|g' \
	_p4='s/--make-pidfile//'
  sed -e "${_shebang}" -e "${_binpath}" -e "${_runpath}" -e "${_p1}" -e "${_p2}" -e "${_p3}" -e "${_p4}" -i "${pkgdir}/etc/init.d/svn"
}
            
sha256sums=('9bf02170dcf73e930a992adf44326ed7c27159d41a503ca4d9371861ee5030c9'
            '421e68201619bfbf7535d8b1a0030390b7ffab998e025f7cbd7e879c677c2819'
            '6cd8551b8ac0dded54f42c2cc9cd55fbc4776d1a541e13d7d571cefd906cb3f4'
            '1c89b70714bac5edece71ecc113a35350fc96ef977be2e0f44d6b46adbbb6c30'
            '45f2dc1a718aed885559e71d98112e670c92bd6b4f19c5cf593eced6cd2bbd97'
            '8f123253c3bfb9bbe87210a9e1facc7f52df371747dbc188396740a5cf4fa713'
            '57c1ad0b14e8458024c713dd8cc2390023b95c27ba4cbd637333b1020f11f398'
            '21825916cf5469151c1fbe546e5a76948dca53921c584882c1413b6bec3cb4f0'
            '37e34461babfb5881169f9729fbdde7d4aba533f123e2c480fe25ac3b863d3e7')
