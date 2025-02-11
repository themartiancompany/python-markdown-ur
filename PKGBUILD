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
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>
# Contributor: Kyle Keen <keenerd@gmail.com>
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Andrew Antle <andrew dot antle at gmail dot com>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: Chaiwat Suttipongsakul <cwt at bashell dot com>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg="markdown"
_Pkgname="Python-Markdown"
pkgname="${_py}-${_pkg}"
pkgver=3.7
pkgrel=2
_pkgdesc=(
  "Python implementation of"
  "John Gruber's Markdown"
)
arch=(
  'any'
)
url="https://${pkgname}.github.io"
license=(
  'BSD'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
optdepends=(
  "${_py}-yaml: parse Python in YAML metadata"
)
checkdepends=(
  "${_py}-yaml"
)
_http="https://github.com"
_ns="${_Pkgname}"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${pkgname}::git+${_url}#tag=${pkgver}"
)
sha256sums=(
  'f263f42a34d1144f77beb9beab7935837e08c1c74325416a1b6c11b88388103a'
)

build() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      build \
      --wheel \
      --no-isolation
}

check() {
  cd \
    "${pkgname}"
  [[ $("${_py}" \
         -c \
	   "import markdown; print(markdown.__version__)") == \
	   "${pkgver}" ]]
  [[ $("${_py}" \
	 -c \
	   "import markdown; print(markdown.markdown('*test*'))") == \
	   "<p><em>test</em></p>" ]]
  "${_py}" \
    -m \
      unittest \
        discover \
	  "tests"
}

package() {
  local \
    site_packages
  site_packages="$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")"
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
      --destdir="${pkgdir}" \
      "dist/"*".whl"
  # symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/Markdown-${pkgver}.dist-info/LICENSE.md" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
