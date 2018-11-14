# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux-vfio packages will only work with the default linux-vfio package! To have a single PKGBUILD target many
# kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
pkgbase="spl-linux-vfio"
pkgname=("spl-linux-vfio" "spl-linux-vfio-headers")
_splver="0.7.12"
_kernelver="4.18.5.arch1-1"
_extramodules="${_kernelver/.arch/-arch}-vfio"

pkgver="${_splver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-vfio-headers=${_kernelver}")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${_splver}/spl-${_splver}.tar.gz")
sha256sums=("4709a06e913bbbeb634161a8b68c3f879e5b6040c6c0c09b1f51042b3178c274")
license=("GPL")
depends=("kmod" "linux-vfio=${_kernelver}")

build() {
    cd "${srcdir}/spl-${_splver}"
    ./autogen.sh
    ./configure --prefix=/usr --libdir=/usr/lib --sbindir=/usr/bin \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build \
                --with-config=kernel
    make
}

package_spl-linux-vfio() {
    pkgdesc="Solaris Porting Layer kernel modules."
    provides=("spl")
    groups=("archzfs-linux-vfio")
    conflicts=("spl-dkms" "spl-dkms-git" 'spl-linux-vfio-git')
    cd "${srcdir}/spl-${_splver}"
    make DESTDIR="${pkgdir}" install
    mv "${pkgdir}/lib" "${pkgdir}/usr/"
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_spl-linux-vfio-headers() {
    pkgdesc="Solaris Porting Layer kernel headers."
    provides=("spl-headers")
    conflicts=("spl-dkms" "spl-dkms-git" "spl-dkms-rc" "spl-headers")
    cd "${srcdir}/spl-${_splver}"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/spl-*/${_extramodules}/Module.symvers
}
