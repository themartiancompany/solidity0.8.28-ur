# SPDX-License-Identifier: AGPL-3.0

#    ---------------------------------------------------------------
#    Copyright © 2024, 2025, 2026
#              Pellegrino Prevete
#
#    All rights reserved
#    ---------------------------------------------------------------
#
#    This program is free software: you can redistribute it
#    and/or modify it under the terms of the GNU Affero
#    General Public License as published by the Free Software
#    Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General
#    Public License along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

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
if [[ "${_os}" == "Android" ]]; then
  _locale="C.UTF-8"
else
  _locale="$(
    locale |
      grep \
        "LANG=" |
        awk \
          -F \
            "=" \
          '{print $1}')"
fi
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

_boost_pkgver_get() {
  local \
    _modes=() \
    _pkgs=() \
    _cut_opts=()
  _modes+=(
    "Q"
    "S"
  )
  _pkgs+=(
    "boost-libs"
    "boost"
  )
  _cut_opts=(
    -d
      "-"
    -f
      "1"
    --complement
  )
  for _pkg in "${_pkgs[@]}"; do
    for _mode in "${_modes[@]}"; do
      _boost_pkgver="$(
        ( pacman \
            -"${_mode}"i \
            "${_pkg}" \
            2>"/dev/null" || \
          true ) |
          grep \
            "Version" |
            awk \
              '{print $3}' |
              rev |
                cut \
                  "${_cut_opts[@]}" |
              rev || \
        echo \
          "null")"
      if [[ "${_boost_pkgver}" != "" && \
            "${_boost_pkgver}" != "null" ]]; then
        break
      fi
    done
  done
}

_fmt_pkgver_get() {
  local \
    _modes=() \
    _pkgs=() \
    _cut_opts=()
  _modes+=(
    "Q"
    "S"
  )
  _pkgs+=(
    "fmt"
  )
  _cut_opts=(
    -d
      "-"
    -f
      "1"
    --complement
  )
  for _pkg in "${_pkgs[@]}"; do
    for _mode in "${_modes[@]}"; do
      _fmt_pkgver="$(
        ( pacman \
            -"${_mode}"i \
            "${_pkg}" \
            2>"/dev/null" || \
          true ) |
          grep \
            "Version" |
            awk \
              '{print $3}' |
              rev |
                cut \
                  "${_cut_opts[@]}" |
              rev || \
        echo \
          "null")"
      if [[ "${_fmt_pkgver}" != "" && \
            "${_fmt_pkgver}" != "null" ]]; then
        break
      fi
    done
  done
}

_verlte() {
  printf \
    '%s\n' \
    "${1}" \
    "${2}" |
    sort \
      -C \
      -V
}

_verlt() {
  ! _verlte \
      "${2}" \
      "${1}"
}

if [[ ! -v "_boost_pkgver" ]]; then
  _boost_pkgver_get
fi
_boost_majver="${_boost_pkgver%.*}"
_boost_16="$(
  ( _verlt \
      "${_boost_majver}" \
      "1.70" && \
      echo \
        "true" ) || \
    echo \
      "false")"
if [[ "${_boost_16}" == "false" ]]; then
  _boost_183="$(
    ( _verlt \
        "${_boost_majver}" \
        "1.83" && \
        echo \
          "true" ) || \
      echo \
        "false")"
fi
if [[ "${_boost_16}" == "false" && \
      "${_boost_183}" == "false" ]]; then
  _boost_latest="true"
else
  _boost_latest="false"
fi
if [[ ! -v "_fmt_pkgver" ]]; then
  _fmt_pkgver_get
fi
_fmt_majver="${_fmt_pkgver%%.*}"
_fmt_11="$(
  ( _verlt \
      "${_fmt_majver}" \
      "12" && \
      echo \
        "true" ) || \
    echo \
      "false")"
# In late 2025 or 2026, according
# to Github, Solidity publishing
# namespace seems to have been moved
# from 'ethereum' to 'argotorg'
# _ns="ethereum"
# Solidity 0.5.x requires
# a different patchset depending
# on the Boost version its built with
# With Boost lesser than 1.70
# it requires no extra patches, with boost
# up to 1.83 it requires version 0.5.16.1
# and with versions greater than 1.88
# it requires version 0.5.16.2
if [[ ! -v "_ns" ]]; then
  if [[ "${_boost_16}" == "true" ]]; then
    _ns="argotorg"
  else
    _ns="themartiancompany"
  fi
fi
if [[ ! -v "_debug" ]]; then
  _debug="true"
fi
if [[ ! -v "_git" ]]; then
  _git="true"
  # _git="${_evmfs}"
fi
if [[ ! -v "_git_service" ]]; then
  if [[ "${_boost_16}" == "true" ]]; then
    _git_service="github"
  elif [[ "${_boost_16}" == "false" ]]; then
    _git_service="gitlab"
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
_cmake_available="$(
  command \
    -v \
    "cmake" || \
  true)"
if [[ "${_cmake_available}" != "" ]]; then
  _cmake_version="$(
    cmake \
      --version |
      head \
        -n \
          1 |
        awk \
          '{print $3}')"
else
  _cmake_version="4."
fi
if [[ "${_cmake_version}" == "4."* ]]; then
  _cmake3="false"
  _cmake4="true"
elif [[ "${_cmake_version}" == "3."* ]]; then
  _cmake3="true"
  _cmake4="false"
fi
if [[ ! -v "_static" ]]; then
  _static="false"
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_boost_16}" == "true" ]]; then
        _archive_format="tar.gz"
      elif [[ "${_boost_16}" == "false" ]]; then
        _archive_format="zip"
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _archive_format="tar.gz"
    fi
  fi
fi
_pkg=solidity
_0_8_28_commit="7893614a31fbeacd1966994e310ed4f760772658"
_bundle_commit="142aa62e6805505b6a06cbeeec530f5c8bf0bfdd"
_0_8_28_1_commit="e67a5cbca580ea980920e5f01a2ac2d43a365b34"
pkgver=0.8.28
_fmtlib_pkgver=11.0.2
_fmtlib_commit="0c9fce2ffefecfdce794e1859584e25877b7b592"
_json_pkgver=3.11.3
_json_commit="9cca280a4d0ccf0c08f47a99aa71d1b0e52f8d03"
_range_v3_pkgver=0.12.0
_range_v3_commit="a81477931a8aa2ad025c6bda0609f38e09e4d7ec"
pkgbase="${_pkg}${pkgver}"
pkgname=(
  "${pkgbase}"
)
pkgrel=61
_pkgdesc=(
  "Smart contract programming language."
)
pkgdesc="${_pkgdesc[*]}"
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
if [[ "${_ns}" == "argotorg" ]]; then
  _commit="${_0_8_28_commit}"
else
  if [[ "${_boost_183}" == "true" || \
        "${_boost_latest}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _commit="${_bundle_commit}"
    elif [[ "${_evmfs}" == "false" ]]; then
      if [[ "${_boost_183}" == "true" ]]; then
        _commit="${_0_8_28_1_commit}"
      elif [[ "${_boost_latest}" == "true" ]]; then
        _commit="${_0_8_28_1_commit}"
      fi
    fi
  fi
fi
_http="https://${_git_http}.com"
url="${_http}/${_ns}/${_pkg}"
_fmtlib_url="${_http}/fmtlib/fmt"
_json_url="${_http}/nlohmann/json"
_range_v3_url="${_http}/ericniebler/range-v3"
license=(
  "GPL-3.0-or-later"
)
depends=()
_boost_makedepends=(
  "boost"
)
if [[ "${_os}" == "Android" ]]; then
  _boost_pkgname="boost"
  _boost_makedepends+=(
    "boost-headers"
    "boost-static"
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _boost_pkgname="boost-libs"
fi
if [[ "${_fmt_11}" == "true" ]]; then
  _fmt="fmt<12"
else
  _fmt="fmt11"
fi
depends=(
  "${_boost_pkgname}"
  "${_fmt}"
  "nlohmann-json"
  "range-v3"
)
if [[ "${_cmake3}" == "true" ]]; then
  _cmake_makedepends=(
    "cmake>3.10"
  )
else
  _cmake_makedepends=(
    "cmake>=4"
    "patch"
  )
fi
makedepends=(
  "${_boost_makedepends[@]}"
  "${_cmake_makedepends[@]}"
  "${_compiler}"
  "${_cmake_generator}"
  "${_fmt}"
  "nlohmann-json"
  "pkgconf"
  "range-v3"
)
if [[ "${_debug}" == "true" ]]; then
  makedepends+=(
    "tree"
  )
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
elif [[ "${_git}" == "false" ]]; then
  makedepends+=(
    "curl"
    "unzip"
  )
fi
if [[ "${_evmfs}" == "true" ]]; then
  makedepends+=(
    "evmfs"
  )
fi
checkdepends=(
  "evmone"
)
group=(
  "ethereum"
  "hip"
)
provides=(
  "${_pkg}=${pkgver}"
  "solc${pkgver}=${pkgver}"
  "solc=${pkgver}"
)
conflicts=(
  "solc${pkgver}"
  "${_pkg}-bin"
  "${_pkg}-git"
)
_cvc4_optdepends=(
  "cvc4:"
    "SMT checker"
)
_z3_optdepends=(
  "z3:"
    "SMT checker"
)
optdepends=(
  "${_cvc4_optdepends[*]}"
  "${_z3_optdepends[*]}"
)
if [[ "${_git}" == "false" ]]; then
  if [[ "${_boost_16}" == "false" ]]; then
    _tag="${_commit}"
    _tag_name="commit"
    _tarname="${_pkg}-${_tag}"
  elif [[ "${_boost_16}" == "true" ]]; then
    _tag="${pkgver}"
    _tag_name="pkgver"
    _tarname="${_pkg}_${_tag}"
  fi
elif [[ "${_git}" == "true" ]]; then
  _tag="${_commit}"
  _tag_name="commit"
  _tarname="${_pkg}-${_tag}"
fi
_0_8_28_1_tarname="${_pkg}-${_0_8_28_1_commit}"
_tarfile="${_tarname}.${_archive_format}"
_0_8_28_1_tarfile="${_0_8_28_1_tarname}.${_archive_format}"
_bundle_sum="77860b58f9d6c4a9a9cb1ceaae7ebe5d856f91f3ccd96f67d5ea6a019d79d1fb"
_bundle_sig_sum="7f737e7a88fdb8e96b428974592def4bbdf5bf24656b12ac5af76084b7fca095"
_0_8_28_1_sum="3a174458dedac6d20314d06274fb1f9d5c47ce9620a37204e79f21fa71cc38ac"
_0_8_28_1_sig_sum="1fe5518ce8693480af6461e6857651a0e8ce2a7aed104300b06d4bcaf9967658"
_fmtlib_sum="SKIP"
_fmtlib_sig_sum="SKIP"
_github_sum="SKIP"
_github_sig_sum="SKIP"
_gitlab_sum="SKIP"
_gitlab_sig_sum="SKIP"
_github_release_sum="SKIP"
_github_release_sig_sum="SKIP"
_github_release_sha512_sum='2ddce3edfc1d570fb42d19d3164f5f7316d511bd3020c711b8176410b39432b7e137806bc63e23bb6c7381ab880c7e7e667217ab4cd8d92a6ad7e2ab14'
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _sum="${_gitlab_sum}"
      _sig_sum="${_gitlab_sig_sum}"
    fi
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="SKIP"
    _sig_sum="SKIP"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _sum="${_gitlab_sum}"
      _sig_sum="${_gitlab_sig_sum}"
    fi
  fi
fi
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_kid_ns="0x926acb6aA4790ff678848A9F1C59E578B148C786"
# Gnosis
_evmfs_net="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
# Harmony
_0_8_28_1_net="1666600000"
_0_8_28_1_address="0x1f762a05cfab651d3d95778f9c89c46545913623"
_evmfs_dir="evmfs://${_evmfs_net}/${_evmfs_address}/${_evmfs_ns}"
_0_8_28_1_dir="evmfs://${_0_8_28_1_net}/${_0_8_28_1_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_bundle_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_sig_uri="${_evmfs_dir}/${_bundle_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
_0_8_28_1_uri="${_0_8_28_1_dir}/${_0_8_28_1_sum}"
_0_8_28_1_src="${_0_8_28_1_tarfile}::${_0_8_28_1_uri}"
_0_8_28_1_sig_uri="${_0_8_28_1_dir}/${_0_8_28_1_sig_sum}"
_0_8_28_1_sig_src="${_0_8_28_1_tarfile}.sig::${_0_8_28_1_sig_uri}"
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "false" ]]; then
    makedepends+=(
      "ur"
      "${_like}"
    )
    _src=""
    _sum=""
  elif [[ "${_git}" == "true" ]]; then
    _src="${_evmfs_src}"
    _sum="${_bundle_sum}"
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_bundle_sig_sum}"
    )
    if [[ "${_boost_16}" == "false" ]]; then
      source+=(
        "${_0_8_28_1_src}"
        "${_0_8_28_1_sig_src}"
      )
      sha256sums+=(
        "${_0_8_28_1_sum}"
        "${_0_8_28_1_sig_sum}"
      )
    fi
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${url}#${_tag_name}=${_tag}?signed"
    _sum="SKIP"
  elif [[ "${_git}" == false ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${url}/archive/${_commit}.${_archive_format}"
        _sum="SKIP"
      elif [[ "${_tag_name}" == "pkgver" ]]; then
        _uri="${url}/releases/download/v${pkgver}/${_tarname}.tar.gz"
        _sum="${_github_release_sum}"
        sha512sums=(
          "${_github_release_sha512_sum}"
        )
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${url}/-/archive/${_tag}/${_tag}.${_archive_format}"
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
if [[ -v "_src" ]]; then
  source+=(
    "${_src}"
  )
  sha256sums+=(
    "${_sum}"
  )
fi
validpgpkeys=(
  # Truocolo
  #   <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
  'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
  # Pellegrino Prevete (dvorak)
  #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
  '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
)

_git_unbundle() {
  local \
    _tarname="${1}" \
    _bundle \
    _repo \
    _msg=()
  _bundle="${srcdir}/${_tarname}.bundle"
  _repo="${srcdir}/${_tarname}"
  _msg=(
    "Cloning '${_bundle}' into '${_repo}'."
  )
  msg \
    "${_msg[*]}"
  git \
    clone \
      "${_bundle}" \
      "${_repo}" || \
  git \
    -C \
      "${_repo}" \
      pull || \
  true
}

_github_tarball_submodule_get() {
  local \
    _project_name="${1}" \
    _project_url="${2}" \
    _commit="${3}" \
    _submodule_path="${4}" \
    _submodule_dir \
    _oldpwd \
    _remotefile \
    _tarname \
    _tarfile \
    _url
  _submodule_dir="$(
    dirname \
      "${_submodule_path}")"
  _oldpwd="${PWD}"
  _tarname="${_project_name}-${_commit}"
  _tarfile="${_tarname}.${_archive_format}"
  _remotefile="${_commit}.${_archive_format}"
  _url="${_project_url}/archive/${_remotefile}"
  _msg=(
    "Downloading '${_url}'."
  )
  echo \
    "${_msg[*]}"
  curl \
    -L \
    -o \
      "${srcdir}/${_tarfile}" \
    "${_url}"
  mkdir \
    -p \
    "${srcdir}/${_tarname}/${_submodule_dir}"
  cd \
    "${srcdir}/${_tarname}/${_submodule_dir}"
  unzip \
    -q \
    "${srcdir}/${_tarfile}"
  mv \
    "${_tarname}" \
    "$(basename \
         "${_submodule_path}")"
  cd \
    "${_oldpwd}"
}

_git_unbundle_update() {
  local \
    _repo="${1}" \
    _bundle="${2}" \
    _repo \
    _msg=()
  _bundle_name="$(
    basename \
      "${_bundle}")"
  _msg=(
    "Updating '${_repo}' from '${_bundle}'."
  )
  msg \
    "${_msg[*]}"
  git \
    -C \
      "${_repo}" \
      remote \
        add \
          "${_bundle_name}" \
          "${_bundle}" ||
  true
  git \
    -C \
      "${_repo}" \
    pull \
      "${_bundle_name}" || \
  true
}

prepare() {
  local \
    _cmake_version \
    _commit_hash \
    _fmtlib_tarname \
    _fmtlib_tarfile
  if [[ "${_evmfs}" == "true" ]]; then
    if [[ "${_git}" == "false" ]]; then
      ur \
        "${_like}"
    elif [[ "${_git}" == "true" ]]; then
      _git_unbundle \
        "${_tarname}"
      if [[ "${_boost_183}" == "true" ]]; then
        _git_unbundle_update \
          "${srcdir}/${_tarname}" \
          "${srcdir}/${_pkg}-${_0_8_28_1_commit}"
      elif [[ "${_boost_latest}" == "true" ]]; then
        _git_unbundle_update \
          "${srcdir}/${_tarname}" \
          "${srcdir}/${_pkg}-${_0_8_28_1_commit}"
      fi
    fi
  fi
  if [[ ! -e "${srcdir}/${_tarname}/commit_hash.txt" ]]; then
    _commit_hash="${_0_8_28_commit:0:8}"    
    echo \
      "${_commit_hash}" > \
      "${srcdir}/${_tarname}/commit_hash.txt"
  fi
  # sed \
  #   -e \
  #     "/-Wsign-conversion/d" \
  #   -i \
  #   "${srcdir}/${_tarname}/cmake/EthCompilerSettings.cmake"
  # _cmake_version="$(
  #   ( cmake \
  #       --version |
  #       head \
  #         -n \
  #           1 |
  #         awk \
  #           '{print $3}' ) ||
  #   true)"
  # if [[ "${_cmake_version}" == "4."* ]]; then
  #   _msg=(
  #     "CMake version 4.x detected,"
  #     "applying patch for JSON"
  #     "preprocessor dependency."
  #   )
  #   echo \
  #     "${_msg[*]}"
  # elif [[ "${_cmake_version}" == "" ]]; then
  #   _msg=(
  #     "No CMake version detected, assuming"
  #     ">=4."
  #   )
  #   echo \
  #     "${_msg[*]}"
  # fi
  _fmtlib_available="$(
    find \
      "${srcdir}/${_tarname}/deps/fmtlib" \
      -mindepth \
        1 \
      -maxdepth \
        1 \
      -type \
        "f" || \
    true)"
  if [[ "${_fmtlib_available}" == "" ]]; then
    if [[ "${_git}" == "true" ]]; then
      git \
        -C \
          "${srcdir}/${_tarname}" \
        submodule \
          update \
          --init \
          "deps/fmtlib"
    elif [[ "${_git}" == "false" ]]; then
      if [[ "${_evmfs}" == "false" ]]; then
        if [[ "${_git_http}" == "github" ]]; then
          _github_tarball_submodule_get \
            "fmt" \
            "${_fmtlib_url}" \
            "${_fmtlib_commit}" \
            "deps/fmtlib"
          _github_tarball_submodule_get \
            "json" \
            "${_json_url}" \
            "${_json_commit}" \
            "deps/nlohmann-json"
          _github_tarball_submodule_get \
            "range-v3" \
            "${_range_v3_url}" \
            "${_range_v3_commit}" \
            "deps/range-v3"
          if [[ "${_debug}" == "true" ]]; then
            tree \
              "${srcdir}/${_tarname}/deps"
          fi
          rm \
            "${srcdir}/${_tarname}/.gitmodules"
        elif [[ "${_git_http}" == "gitlab" ]]; then
          _msg=(
            "You can take an extra effort"
            "if you want to retrieve"
            "the sources from Gitlab"
            "this time. Still it builds"
            "normally."
          )
          echo \
            "${_msg[*]}"
          exit \
            1
          tar \
            xf \
            "${srcdir}/${_tarfile}"
        fi
      elif [[ "${_evmfs}" == "true" ]]; then
        _msg=(
          "I may have already made"
          "whole 'fmt' sources undeletable but then"
          "I have packaged only 'fmt8' I'm not"
          "sure, still it seemed quite excessive"
          "bringing down 'fmt8' today."
        )
        echo \
          "${_msg[*]}"
      fi
    fi
  fi
}

_usr_get() {
  local \
    _bin \
    _cc \
    _usr
  _cc="$(
    command \
      -v \
      "cc" \
      "g++" \
      "clang++" | \
      head \
        -n \
          1)"
  _bin="$(
    dirname \
      "${_cc}")"
  _usr="$(
    dirname \
      "${_bin}")"
  echo \
    "${_usr}"
}

_compile() {
  local \
    _tests="${1}" \
    _cmake_opts=() \
    _cmake_version \
    _cc \
    _cxx \
    _cxx_compiler \
    _cxxflags=() \
    _flags=() \
    _ldflags=() \
    _cmake_static_opt \
    _libfmt
  _libfmt="$(_usr_get)/lib/libfmt.so.11"
  _cc="$(
    command \
      -v \
      "${_compiler}")"
  _ldflags+=(
    ${LDFLAGS}
    "${_libfmt}"
  )
  _cxxflags=(
    "${CXXFLAGS}"
    -I"/usr/include/fmt11"
    -Wl,"${_libfmt}"
    -Wno-unused-but-set-variable
    # -Wno-unknown-warning-option
    -Wno-deprecated-declarations
    # -Wno-dangling-reference
    # -Wno-overloaded-virtual
    # -Wno-range-loop-construct
    # -Wno-sign-conversion
    # -Wno-aggressive-loop-optimizations
  )
  if [[ "${_os}" == "Android" ]]; then
    _cxxflags+=(
      # -Wno-unqualified-std-cast-call
      # -Wno-dangling-field
    )
  elif [[ "${_os}" == "GNU/Linux" ]]; then
    _cxxflags+=(
      # -Wno-overloaded-virtual
    )
  fi
  if [[ "${_compiler}" == "gcc" ]]; then
    _cxx_compiler="g++"
  elif [[ "${_compiler}" == "clang" ]]; then
   _cxx_compiler="${_compiler}++"
  fi
  _cxx="$(
    command \
      -v \
      "${_cxx_compiler}")"
  _cmake_version="$(
    cmake \
      --version | \
      head \
        -n \
          1 |
        awk \
          '{print $3}')"
  _msg=(
    "CMake version '${_cmake_version}'."
  )
  echo \
    "${_msg[*]}"
  # if [[ "${_cmake_version}" == "4."* ]]; then
  #   _cmake_opts+=(
  #     -D CMAKE_POLICY_VERSION_MINIMUM=3.5
  #   )
  # fi
  if [[ "${_static}" == "true" ]]; then
    _cmake_static_opt="ON"
  elif [[ "${_static}" == "false" ]]; then
    _cmake_static_opt="OFF"
  else
    _msg=(
      "Unknown value '${_cmake_static_opt}'"
      "for variable '_cmake_static_opt'."
    )
    echo \
      "${_msg[*]}"
    exit \
      1
  fi
  if [[ "${_git}" == "true" ]]; then
    _vendored_deps="OFF"
  elif [[ "${_git}" == "false" ]]; then
    _vendored_deps="ON"
  fi
  _cmake_opts=(
    # --trace-expand 
    # -G
    #   "Ninja"
    -D
      CMAKE_BUILD_TYPE="Release"
    -D
      CMAKE_INSTALL_PREFIX="/usr/"
    -D
      CMAKE_EXECUTABLE_SUFFIX="${pkgver}"
    -D
      ONLY_BUILD_SOLIDITY_LIBRARIES="OFF"
    # -D
    #   CMAKE_VERBOSE_MAKEFILE:BOOL="ON"
    -D
      PEDANTIC="ON"
    -D
      PROFILE_OPTIMIZER_STEPS="OFF"
    -D
      SOLC_LINK_STATIC="${_cmake_static_opt}"
    -D
      SOLC_STATIC_STDLIBS="${_cmake_static_opt}"
    -D
      STRICT_NLOHMANN_JSON_VERSION="OFF"
    -D
      STRICT_Z3_VERSION="OFF"
    -D
      TESTS="${_tests}"
    -D
      USE_LD_GOLD="OFF"
    # -D
    #  Boost_USE_STATIC_LIBS="${_cmake_static_opt}"
    -D
      IGNORE_VENDORED_DEPENDENCIES="ON"
    -D
     USE_SYSTEM_LIBRARIES="ON"
    -D
      CMAKE_CXX_FLAGS="${_cxxflags[*]}"
    # -D
    #   CMAKE_LD_FLAGS="${_cxxflags[*]}"
    -S
      "${srcdir}/${_tarname}/"
    -Wno-dev
  )
  _flags+=(
    CC="${_cc}"
    CXX="${_cxx}"
    CXXFLAGS="${_cxxflags[*]}"
    LDFLAGS="${_ldflags[*]}"
  )
  export \
    CC="${_cc}" \
    CXX="${_cxx}" \
    CXXFLAGS="${_cxxflags[*]}" \
    LDFLAGS="${_ldflags[*]}"
  # VERBOSE=1 \
  _msg=(
    "cxxflags:"
      "${_cxxflags[*]}"
  )
  echo \
    "${_msg[*]}"
  _msg=(
    "ldflags:"
      "${_ldflags[*]}"
  )
  echo \
    "${_msg[*]}"
  _msg=(
    "CMake options:"
      "${_cmake_opts[*]}"
  )
  echo \
    "${_msg[*]}"
  CC="${_cc}" \
  CXX="${_cxx}" \
  CXXFLAGS="${_cxxflags[*]}" \
  LDFLAGS="${_ldflags[*]}" \
  cmake \
    -B \
      "${srcdir}/${_tarname}/build/" \
    "${_cmake_opts[@]}"
  # VERBOSE=1 \
  CC="${_cc}" \
  CXX="${_cxx}" \
  CXXFLAGS="${_cxxflags[*]}" \
  LDFLAGS="${_ldflags[*]}" \
  cmake \
    --build \
      "${srcdir}/${_tarname}/build/"
  echo \
    "Done."
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

package() {
  local \
    _binaries=() \
    _bin
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
      "${srcdir}/${_tarname}/build/" || \
    true
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
