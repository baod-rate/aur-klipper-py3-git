# Maintainer: loredan13
# Contributor: lf <packages at lfcode dot ca>
pkgname=klipper-py3-git
_pkgbase=klipper
pkgver=r3436.0878807e
pkgrel=1
pkgdesc="3D printer firmware with motion planning on the host"
arch=('x86_64' 'i686' 'arm' 'armv6h' 'armv7h' 'aarch64')
url="https://github.com/KevinOConnor/klipper"
license=('GPLv3')
groups=()
depends=(
    python-cffi
    python-pyserial
    python-greenlet
    python-jinja
    ncurses
    libusb
    avrdude
    avr-gcc
    avr-binutils
    avr-libc
)
optdepends_x86_64=(
    'arm-none-eabi-gcc: for ARM MCU firmware compilation'
    'arm-none-eabi-newlib: for ARM MCU firmware compilation'
    'stm32flash: for flashing firmware on STM MCU'
)
makedepends=('git')
provides=("$_pkgbase")
conflicts=("$_pkgbase")
replaces=()
backup=("etc/${_pkgbase}/klipper.cfg")
options=()
install=
source=("${_pkgbase}::git+https://github.com/KevinOConnor/klipper#branch=work-python3-20200612"
        'klipper.service'
        'sysusers.conf'
        'tmpfiles.conf')
noextract=()
md5sums=('SKIP'
         '6a2713f46f58bab015c367d937030031'
         'cf3715af9f53cc1660e412abe3697342'
         '94100ed3da74a98bdaed27f395621511')

pkgver() {
    cd "$srcdir/$_pkgbase"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "$srcdir/$_pkgbase"
    GIT_COMMITTER_NAME=aur \
    GIT_COMMITTER_EMAIL=aur@archlinux.org \
    git rebase master
}

build() {
    cd "$srcdir/$_pkgbase"
    python -m compileall klippy
    python klippy/chelper/__init__.py
}

check() {
    cd "$srcdir/$_pkgbase"
    # TODO: compile configs and run tests
    # python scripts/test_klippy.py -d
}

package() {
    cd "$srcdir/$_pkgbase"
    install -Dm644 "$srcdir/klipper.service" "$pkgdir/usr/lib/systemd/system/${_pkgbase}.service"
    install -Dm644 "$srcdir/sysusers.conf" "$pkgdir/usr/lib/sysusers.d/${_pkgbase}.conf"
    install -Dm644 "$srcdir/tmpfiles.conf" "$pkgdir/usr/lib/tmpfiles.d/${_pkgbase}.conf"

    install -m755 -d "$pkgdir/usr/share/doc/${_pkgbase}"
    install -m755 -d "$pkgdir/usr/share/${_pkgbase}/scripts"
    install -m755 -d "$pkgdir/usr/share/${_pkgbase}/examples"
    install -m755 -d "$pkgdir/usr/lib/${_pkgbase}/klippy"
    install -m755 -d "$pkgdir/usr/lib/${_pkgbase}/lib"
    install -m755 -d "$pkgdir/etc/${_pkgbase}"
    install -m755 -d "$pkgdir/var/lib/${_pkgbase}"

    cp -r "$srcdir/${_pkgbase}/docs"/* "$pkgdir/usr/share/doc/${_pkgbase}"/
    cp -r "$srcdir/${_pkgbase}/scripts/"* "$pkgdir/usr/share/${_pkgbase}/scripts"/
    cp -r "$srcdir/${_pkgbase}/config/"* "$pkgdir/usr/share/${_pkgbase}/examples"/
    cp -r "$srcdir/${_pkgbase}/klippy/"* "$pkgdir/usr/lib/${_pkgbase}/klippy"/
    cp -r "$srcdir/${_pkgbase}/lib/"* "$pkgdir/usr/lib/${_pkgbase}/lib"/

    python scripts/make_version.py ARCHLINUX > "$pkgdir/usr/lib/${_pkgbase}/klippy/.version"
}
