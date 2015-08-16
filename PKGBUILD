# Maintainer: Christoph Haag <haagch@studi.informatik.uni-stuttgart.de>
pkgname=ibex-git
pkgver=610.90710a1
pkgrel=1
pkgdesc="Ibex - 3D Virtual Reality Desktop/Compositing Manager for X11/Linux, Windows and Mac OS (for e.g. Oculus Rift)"
arch=('i686' 'x86_64')
url="https://bitbucket.org/druidsbane/ibex/overview"
license=('GPL')
depends=('mesa' 'oculus-rift-sdk-jherico-git')
makedepends=('irrlicht' 'freeglut' 'glew' 'tinyxml')
#options=("!strip")

_gitname="ibex"
_builddir="build"

#temporarily my fork, until upstream fixes its stuff
source=("$_gitname::git+https://bitbucket.org/haagch/ibex.git" 'ibex-startscript')

#        'fix_install_prefix.diff'
#        'ibex-startscript'
#        'liboculusvr.diff'
#        'vlcplayer.diff'
#        'fix-segfault.diff')

#        "http://sixense.com/SDK/sixenseSDK_linux_OSX.zip")
# optionally (???) download sixenseSDK_linux_OSX.zip from http://sixense.com/linuxsdkdownload, put it in the directory, then comment in this line and the md5sum

# not in AUR I think
# optdepends=('blender-ogre-exporter: to convert models to Ogre meshes: https://code.google.com/p/blender2ogre/')

prepare() {
 msg "Patching ibex CMakeFiles.txt..."
  cd ${srcdir}/${_gitname}

  # it doesn't respect -DCMAKE_INSTALL_PREFIX, this patches CMakeFiles.txt to let "make install" install to $CMAKE_INSTALL_PREFIX/usr/share/
  # https://bitbucket.org/druidsbane/ibex/issue/3/take-cmake_install_prefix-into-account
  #git apply -vvv ${srcdir}/fix_install_prefix.diff

#  for i in ${srcdir}/*.diff; do
#        git apply -vvv --ignore-space-change --ignore-whitespace "$i"
#  done

  #https://bitbucket.org/druidsbane/ibex/issue/14/linux-glutinit-gets-called-twice-from
#  sed "/glutInit/d" -i "$srcdir/ibex/x11/x11.cpp"
}

build() {

  # maybe delete it every time for a fresh build?
  #rm -rf ${srcdir}/${_builddir}
  mkdir -p ${srcdir}/${_builddir}
  cd ${srcdir}/${_builddir}

  msg "Starting make for ibex..."

 cmake ../ibex \
         -DCMAKE_INSTALL_PREFIX=/usr \
         -DCMAKE_BUILD_TYPE=Release \
        -DOCULUS_INCLUDE_DIR=/usr/include/ovr-0.3.2/ovr #TODO

#         -DCMAKE_BUILD_TYPE=Debug \

#         -DOCULUS_LIBRARY_DIR="$srcdir/OculusSDK/LibOVR/" \
#         -DOCULUS_INCLUDE_DIR="$srcdir/OculusSDK/LibOVR/Include/" \

  make
}

package() {
  cd ${srcdir}/${_builddir}
  make DESTDIR="$pkgdir/" install
  install -m 755 -d ${pkgdir}/usr/bin/
  install -m 755 ${srcdir}/ibex-startscript ${pkgdir}/usr/bin/ibex

  install -d "$pkgdir/usr/share/ibex/lib/"
}

pkgver() {
  cd ${srcdir}/${_gitname}
  echo $(git rev-list --count master).$(git rev-parse --short master)
}


md5sums=('SKIP'
         '7b6df4c5a397928b839d978ac30ef613')
