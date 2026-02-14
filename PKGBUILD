# SPDX-License-Identifier: AGPL-3.0
      

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
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

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Matheus
#     <matheusgwdl@protonmail.com>
#   Serge K
#     <arch@phnx47.net>
#   Nicola Squartini
#     <tensor5@gmail.com>

# shellcheck disable=SC2034
# shellcheck disable=SC2154
_os="$(
  uname \
    -o)"
_evmfs_available="$(
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
_locale="$(
  locale |
    grep \
      "LANG=" |
      awk \
        -F \
          "=" \
        '{print $1}')"
if [[ ! -v "_en" ]]; then
  _en="true"
  if [[ "${_locale}" == "C.UTF-8" ]]; then
    _en="true"
  fi
fi
if [[ ! -v "_it" ]]; then
  _it="true"
  if [[ "${_locale}" == "it_IT.UTF-8" ]]; then
    _it="true"
  fi
fi
if [[ ! -v "_like" ]]; then
  if [[ "${_it}" == "true" ]]; then
    _like="mo-me-lo-segno"
  fi
  if [[ "${_en}" == "true" ]]; then
    _like="never-gonna-give-you-up"
  fi
fi
if [[ ! -v "_boost_pkgver" ]]; then
  _boost_pkgver="$(
    ( pacman \
        -Qi \
        "boost-libs" || 
     pacman \
       -Si \
       "boost-libs" || \
      pacman \
        -Qi \
        "boost" || \
      pacman \
        -Si \
	"boost" ) |
      grep \
        "Version" |
        awk \
          '{print $3}' |
          rev |
            cut \
              -d \
                "-" \
              -f \
                "1" \
              --complement |
              rev || \
    echo \
      "null")"
fi
_boost_majver="${_boost_pkgver%.*}"
_boost_oldest="$(
  printf \
    "%s\n%s" \
    "1.89" \
    "${_boost_majver}" |
    sort \
      -V |
      head \
        -n \
          1)"
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ ! -v "_git_service" ]]; then
  if [[ "${_boost_oldest}" == "1.89" ]]; then
    _git_service="gitlab"
  elif [[ "${_boost_oldest}" != "1.89" ]]; then
    _git_service="github"
  fi
fi
if [[ ! -v "_git_http" ]]; then
  if [[ "${_git_service}" == "gitlab" ]]; then
    _git_http="${_git_service}"
  elif [[ "${_git_service}" == "github" ]]; then
    _git_http="${_git_service}"
  fi
fi
if [[ "${_os}" == "Android" ]]; then
  _compiler="clang"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler="gcc"
fi
if [[ ! -v "_cmake_generator" ]]; then
  _cmake_generator="make"
  # _cmake_generator="ninja"
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    _archive_format="tar.gz"
  fi
fi
_pkg=solidity
pkgver="0.8.28"
_boost_pkgver="1.83"
_commit="7893614a31fbeacd1966994e310ed4f760772658"
pkgbase="${_pkg}${pkgver}"
pkgname+=(
  "${pkgbase}"
)
pkgrel=11
pkgdesc="Smart contract programming language."
arch=(
  "x86_64"
  "i686"
  "aarch64"
  "arm"
  "armv7l"
  "armv6l"
  "mips"
  "powerpc"
  "pentium4"
)
_http="https://github.com"
_ns="ethereum"
url="${_http}/${_ns}/${_pkg}"
license=(
  "GPL-3.0-or-later"
)
depends=()
if [[ "${_os}" == "Android" ]]; then
  depends+=(
    "boost<${_boost_pkgver}"
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  depends+=(
    "boost-libs<${_boost_pkgver}"
  )
fi
optdepends=(
  "cvc4: SMT checker"
  "z3: SMT checker"
)
makedepends=(
  "cmake"
  "${_compiler}"
)
if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    "boost-headers<${_boost_pkgver}"
    "boost-static<${_boost_pkgver}"
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  makedepends+=(
    "boost<${_boost_pkgver}"
  )
fi
checkdepends=(
  "evmone"
)
provides=(
  "${_pkg}=${pkgver}"
  "solc${pkgver}=${pkgver}"
  "solc=${pkgver}"
)
_tarname="${_pkg}_${pkgver}"
source=(
  "${_tarname}.tar.gz::${url}/releases/download/v${pkgver}/${_tarname}.tar.gz"
)
sha512sums=(
  '2ddce3edfc1d570fb42d19d3164f5f7316d511bd3020c711b8176410b39432b7e137806bc63e23bb6c7381ab880c7e7e667217ab4cd8d92a6ad7e2ab145a194f'
)

_boost_version_get() {
  pacman \
    -Qi \
    boost | \
    grep \
      "Version" | \
      awk \
        '{print $3}' | \
        rev | \
          cut \
            -d \
              "-" \
            -f \
	      2 | \
            rev
}

_verlte() {
  printf \
    '%s\n' \
    "${1}" \
    "${2}" | \
    sort \
      -C \
      -V
}

_verlt() {
  ! _verlte \
      "${2}" \
      "${1}"
}

_compile() {
  local \
    _tests="${1}" \
    _boost_version \
    _cmake_opts=() \
    _cxxflags=() \
    _cc \
    _cxx \
    _cxx_compiler
  _cc="$( \
    command \
      -v \
      "${_compiler}")"
  if [[ "${_compiler}" == "gcc" ]]; then
    _cxx_compiler="g++"
  elif [[ "${_compiler}" == "clang" ]]; then
   _cxx_compiler="${_compiler}++"
  fi
  _cxx="$( \
    command \
      -v \
      "${_cxx_compiler}")"
  _cxxflags=(
    "${CXXFLAGS}"
  )
  if [[ "${_os}" == "Android" ]]; then
    _cxxflags+=(
      -Wno-unused-but-set-variable
    )
    _boost_version="$( \
      _boost_version_get)"
    if _verlt \
	   "${_boost_version}" \
	   "1.86.0"; then
      echo \
        "Installed boost version" \
	"<=1.86.0, building with" \
        "deprecated declarations." \
        "Also be sure to use Clang" \
	"<19.x"
      _cxxflags+=(
        -Wno-deprecated-declarations
      )
    fi
  fi
  _cmake_opts=(
    -D CMAKE_BUILD_TYPE="None"
    -D CMAKE_INSTALL_PREFIX="/usr/"
    -D CMAKE_EXECUTABLE_SUFFIX="${pkgver}"
    -D ONLY_BUILD_SOLIDITY_LIBRARIES="OFF"
    -D PEDANTIC="ON"
    -D PROFILE_OPTIMIZER_STEPS="OFF"
    -D SOLC_LINK_STATIC="OFF"
    -D SOLC_STATIC_STDLIBS="OFF"
    -D STRICT_NLOHMANN_JSON_VERSION="OFF"
    -D STRICT_Z3_VERSION="OFF"
    -D TESTS="${_tests}"
    -D USE_LD_GOLD="OFF"
    -D USE_SYSTEM_LIBRARIES="OFF"
    -S "${srcdir}/${_tarname}/"
    -Wno-dev
  )
  export \
    CC="${_cc}" \
    CXX="${_cxx}" \
    CXXFLAGS="${_cxxflags[*]}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  CXXFLAGS="${_cxxflags[*]}" \
  cmake \
    -B \
      "${srcdir}/${_tarname}/build/" \
    "${_cmake_opts[@]}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  CXXFLAGS="${_cxxflags[*]}" \
  cmake \
    --build \
      "${srcdir}/${_tarname}/build/"
}

build() {
  local \
    _tests_switch=() \
    _tests_switch_status
  _tests_switch=(
    "OFF"
    # "ON"
  )
  for _tests_switch_status \
    in "${_tests_switch[@]}"; do
    _compile \
      "${_tests_switch_status}"
  done
}

check() {
  _compile \
    "ON"
  "${srcdir}/${_tarname}/build/test/soltest" \
    -p \
      true -- \
    --testpath \
      "${srcdir}/${_tarname}/test/"
  _compile \
    "OFF"
}

package_solidity0.8.28() {
  local \
    _binaries=() \
    _bin
  conflicts=(
    "${_pkg}${pkgver}-bin"
    "solc${pkgver}"
    "solc${pkgver}-bin"
  )
  _binaries=(
    "solc"
    "yul-phaser"
  )
  # Assure that the directories exist.
  mkdir \
    -p \
      "${pkgdir}/usr/share/doc/${pkgname}/"
  # Install the software.
  DESTDIR="${pkgdir}/" \
  cmake \
    --install \
      "${srcdir}/${_tarname}/build/"
  # Rename the binaries because
  # CMAKE_EXECUTABLE_SUFFIX doesn't work
  for _bin in "${_binaries[@]}"; do
    mv \
      "${pkgdir}/usr/bin/${_bin}" \
      "${pkgdir}/usr/bin/${_bin}${pkgver}" || \
      true
  done
  # Install the documentation.
  install \
    -Dm644 \
    "${srcdir}/${_tarname}/README.md" \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  cp \
    -r \
    "${srcdir}/${_tarname}/docs/"* \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  find \
    "${pkgdir}/usr/share/doc/${pkgname}/" \
    -type \
      d \
    -exec \
      chmod \
        755 \
	{} +
  find \
    "${pkgdir}/usr/share/doc/${pkgname}/" \
    -type \
      f \
    -exec \
      chmod \
      644 \
      {} +
}
