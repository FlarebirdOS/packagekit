pkgname=(
    packagekit
    libpackagekit-glib
)
pkgbase=packagekit
pkgver=1.3.2
pkgrel=1
pkgdesc="A system designed to make installation and updates of packages easier"
arch=('x86_64')
url="https://www.freedesktop.org/software/PackageKit/"
license=('GPL-2.0-or-later')
makedepends=(
    'git'
    'bash-completion'
    'docbook-xsl'
    'glib2-devel'
    'gobject-introspection'
    'intltool'
    'libxslt'
    'meson'
    'polkit'
    'python-setuptools'
    'sqlite'
    'vala'
)
options=('!emptydirs')
source=(git+ssh://git@github.com/PackageKit/PackageKit.git#tag=v${pkgver})
sha256sums=(3a4674c2eb55b0bc9760bfbefdf0e6f6340cae72f4b20cd592d1396dc3086cde)

build() {
    cd PackageKit

    local meson_args=(
        -D cron=false
        -D gstreamer_plugin=false
        -D gtk_doc=false
        -D gtk_module=false
        -D packaging_backend=alpm
        -D systemd=true
    )

    ${meson_options} "${meson_args[@]}"

    ${meson_build}
}

package_packagekit() {
    depends=(
        'libpackagekit-glib'
        'pacman'
        'polkit'
        'sqlite'
    )
    backup=(
        var/lib/PackageKit/transactions.db
        etc/PackageKit/alpm.d/pacman.conf
        etc/PackageKit/alpm.d/repos.list
    )

    cd PackageKit

    ${meson_install} ${pkgdir}

    _pick libpackagekit ${pkgdir}/usr/include
    _pick libpackagekit ${pkgdir}/usr/lib64/{girepository-1.0,libpackagekit-glib2.so*,pkgconfig}
    _pick libpackagekit ${pkgdir}/usr/share/{gir-1.0,vala}
}

package_libpackagekit-glib() {
    pkgdesc="GLib library for accessing PackageKit"
    depends=('glib2')

    mv libpackagekit/* ${pkgdir}
}
