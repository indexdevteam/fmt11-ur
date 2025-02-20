# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.


# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Jeremy Kescher <jeremy@kescher.at>
# Based on Arch `fmt` package
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Mihai Bişog <mihai.bisog@gmail.com>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="gcc-libs"
elif [[ "${_os}" == "Android" ]]; then
  # This is probably libc++ actually
  _libc="ndk-sysroot"
fi
_pkg=fmt
_majver="10"
pkgname="${_pkg}${_majver}"
pkgver="${_majver}.2.1"
_commit="e69e5f977d458f2650bb346dadf2ad30c5320281"
pkgrel=1
pkgdesc='Open-source formatting library for C++'
arch=(
  'x86_64'
  'i686'
  'arm'
  'armv7l'
  'aarch64'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
url="https://${_pkg}.dev"
license=(
  "MIT"
)
depends=(
  "${_libc}"
)
makedepends=(
  'cmake'
  'doxygen'
  'git'
  'ninja'
  'npm'
  'python-breathe'
  'python-docutils'
  'python-jinja'
  'python-six'
  'python-sphinx'
  'python-wheel'
)
provides=(
  "lib${_pkg}.so=${_majver}-64"
  "${_pkg}"
)
_tag_name="commit"
_tag="${_commit}"
_http="https://github.com"
_ns="${_pkg}lib"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "git+${_url}.git#${_tag_name}=${_tag}"
  "fmt-no-pip-no-virtualenv.patch"
  "${_pkg}-${_majver}.0.0-sphinx.patch"
)
b2sums=(
  'e79699a46afe099007f9ad4b5ba063fbd8a90575535cd006452c7e648d4a6131460407ecfeb824c7e4d64d793f9e09ff2953afd7f997cd4a273b9422b832e46b'
  '0bc421afdc4c2527525ce2e21740c9f72e05431394fb4710c1a8fa6d3bb2ee20d0630e2a76ddbac3c0ba27c1ab08f0c8e27d060def1370721b1c94246cbbf0ff'
  '4eabdf38317e22e6b650b91821f1fab50bb3641e4f9a63847cb9b823becd3a4106fe47df37c8dc886f5fe1d1d3e529136c867459105df07c359582214d6fa01f'
)

prepare() {
  cd \
    "${_pkg}"
  patch \
    -Np1 \
    -i \
    "../fmt-no-pip-no-virtualenv.patch"
  patch \
    -Np1 \
    -i \
    "../fmt-${_majver}.0.0-sphinx.patch"
}

pkgver() {
  cd \
    "${_pkg}"
  git \
    describe \
      --tags
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts+=(
    -S
      "${_pkg}"
    -B
      "build"
    -G
      "Ninja"
    -DCMAKE_BUILD_TYPE="None"
    -DCMAKE_INSTALL_PREFIX="/usr"
    -DCMAKE_INSTALL_LIBDIR="/usr/lib"
    -DFMT_CMAKE_DIR="lib/cmake/${pkgname}"
    -DFMT_DOC="OFF"
    -DFMT_INC_DIR="include/${pkgname}" \
    -DFMT_PKGCONFIG_DIR="lib/$pkgname/pkgconfig"
    -DFMT_TEST="$CHECKFUNC"
    -DBUILD_SHARED_LIBS="ON"
    -Wno-dev
  )
  cmake \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      "build"
}

check() {
  cmake \
    --build \
      "build" \
    --target 
      "test"
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
      "build"
  install \
    -Dm644 \
    "${_pkg}/LICENSE" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  cd \
    "${pkgdir}"
  ln \
    -sf \
    "../lib${_pkg}.so.10" \
    "usr/lib/${pkgname}/libfmt.so"
  rm \
    "usr/lib/lib${_pkg}.so"
  sed \
    -i \
    "/libdir/s/\/lib/&\/${pkgname}/" \
    "usr/lib/${pkgname}/pkgconfig/"*".pc"
}

# vim: ts=2 sw=2 et:
