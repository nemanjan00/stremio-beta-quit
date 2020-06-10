# Maintainer: Vladimir Borisov <vladimir@stremio.com>
_pkgname=stremio
pkgname=${_pkgname}
pkgver=4.4.113
pkgrel=1
pkgdesc="The next generation media center"
arch=($(uname -m))
url="https://www.stremio.com"
license=("MIT")
groups=()
depends=("nodejs" "ffmpeg" "qt5-webengine" "qt5-webchannel" "qt5-declarative" "qt5-quickcontrols" "qt5-quickcontrols2" "qt5-translations" "mpv" "openssl")
makedepends=("git" "wget" "qt5-tools" "librsvg")
provides=("${_pkgname}")
conflicts=("${_pkgname}" "stremio-legacy" "stremio-beta")
replaces=("stremio-git")
backup=()
options=()

install=stremio.install

source=("${_pkgname}::git+https://github.com/Stremio/stremio-shell.git#branch=master")
noextract=()
md5sums=("SKIP")

# Please refer to the 'USING git SOURCES' section of the PKGBUILD man page for
# a description of each element in the source array.

pkgver() {
	cd "$srcdir/${_pkgname}"
	grep -oPm1 'VERSION=\K.+' stremio.pro
	# Git, tags available
	#printf "%s" "$(git describe --long | sed 's/\([^-]*-\)g/r\1/;s/-/./g')"
}

prepare() {
	cp ../0001-Quit.patch "$srcdir/${_pkgname}/"

	cd "$srcdir/${_pkgname}"
	if [ -n "$BRANCH" ]; then
		git checkout "$BRANCH"
	fi

	git am < ./0001-Quit.patch

	git submodule update --init
	make -f release.makefile clean
}

build() {
	cd "$srcdir/${_pkgname}"
	make -f release.makefile PREFIX="$pkgdir"
}

package() {
	cd "$srcdir/${_pkgname}"
	export PREFIX="$pkgdir";
	make -f release.makefile install
}
