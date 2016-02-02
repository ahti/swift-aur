_gitbranch='swift-2.2-branch'
pkgname='swiftc'
pkgver="2.2.20151222a"
pkgver() {
  cd "$srcdir/swift"
  git describe --long --tags | sed -r 's/swift-([0-9]+\.[0-9]+)-SNAPSHOT-([0-9]+)-([0-9]+)-([0-9]+)-([a-z]+)-([0-9]+)/\1.\2\3\4\5.r\6/g;s/-/./g'
}
pkgrel='1'
arch=('x86_64')
license=('Apache')
pkgdesc='Swift Programming Language'
url='https://swift.org/'

depends=(
  'libxml2'
  'python'
  'python2'
  'libutil-linux'
  'libedit'
  'icu'
  'libbsd'
)

makedepends=(
  'git'
  'cmake'
  'ninja'
  'clang'
  'sqlite'
  'swig'
  'ncurses'
  'rsync'
)

# By default makepkg runs strip on binaries. This seems to cause issues with the Swift REPL.
# Disable it in the PKGBUILD with:
options=(!strip)

# /etc/makepkg.conf has this defined as "-Wl,-01,--sort-common,--as-needed,-z,relro"
# I think this is an issue not sure why though
LDFLAGS=""

source=(
  'fix-lldb-build.patch'
  '0001-Provide-a-custom-preset-for-Arch-Linux.patch'
  "swift::git+http://github.com/apple/swift.git#branch=${_gitbranch}"
  "llvm::git+http://github.com/apple/swift-llvm.git#branch=${_gitbranch}"
  "clang::git+http://github.com/apple/swift-clang.git#branch=${_gitbranch}"
  "lldb::git+http://github.com/apple/swift-lldb.git#branch=${_gitbranch}"
  "cmark::git+http://github.com/apple/swift-cmark.git#branch=${_gitbranch}"
  "llbuild::git+http://github.com/apple/swift-llbuild.git#branch=master"
  "swiftpm::git+http://github.com/apple/swift-package-manager.git#branch=master"
  "swift-corelibs-xctest::git+http://github.com/apple/swift-corelibs-xctest.git#branch=master"
  "swift-corelibs-foundation::git+http://github.com/apple/swift-corelibs-foundation.git#branch=master"
  "swift-integration-tests::git+http://github.com/apple/swift-integration-tests.git#branch=${_gitbranch}"
)

sha256sums=(
  'c62a1a903a9849be53f5bb9cc6f701f0a2409e188a38b9df5cc565e9b8f3f9ba'
  '6c876a071616abe0d79ac64c5520662e3f0a68bf1fb9aac98cad7c9003077331'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
)

prepare() {
  cd "$srcdir/swift"
  git apply "$srcdir/0001-Provide-a-custom-preset-for-Arch-Linux.patch"

  cd "$srcdir/lldb"
  # This is a proposed patch provided by the community. This patches Python 3 compatibility.
  # https://bugs.swift.org/browse/SR-14
  git apply "$srcdir/fix-lldb-build.patch"
}

package() {
  export LC_CTYPE=en_US.UTF-8
  export LANG=en_US.UTF-8
  installable_package="$(readlink -f ${srcdir}/../swift-${pkgver}.tar.gz)"
  cd "$srcdir/swift"
  utils/build-script --preset=buildbot_arch_linux installable_package="${installable_package}" install_destdir="$pkgdir/"
  tar xf "${installable_package}" -C "$pkgdir/"
}
