# Maintainer: Shun Terabayashi <shunonymous@gmail.com>
# Contributor: Zhang Hai <dreaming.in.code.zh@gmail.com>
# Contributor: Alexander F Rødseth <xyproto@archlinux.org>
# Contributor: Daniel Micay <danielmicay@gmail.com>
# Contributor: Gordin <9ordin@gmail.com>

# Finding dependencies:
# ELF dependencies:
# cd /lib/ && find /opt/android-sdk/tools/ -type f -executable \
#   | LANG=C xargs readelf -d 2>/dev/null \
#   | grep -oP '(?<=Shared library: \[).*(?=\])' | LANG=C xargs pacman -Qo \
#   | cut -d' ' -f5 | sort | uniq
# .so in JAR files:
# find /opt/android-sdk/tools/ -type f -exec unzip -l {} 2>&1 \; \
#   | grep -P '^Archive:|\.so$' \
#   | awk 'BEGIN { archive = ""; } { if (NF == 2) { archive = $0; } else { \
#     if (length(archive) > 0) { print archive; archive="" } print $0; } }'
# Note that dependency on libxtst is from swt.

pkgname=android-rebuilds-sdk-bin
pkgver=26.1.1
pkgrel=1
pkgdesc='Android SDK binaries provided by Android Rebuils project'
arch=('x86_64' 'i686')
url='https://android-rebuilds.beuc.net/'
license=('custom')
provides=('android-sdk')
depends_i686=('java-environment' 'libxtst' 'fontconfig' 'freetype2' 'gcc-libs'
              'libx11' 'libxext' 'libxrender' 'zlib')
depends_x86_64=('java-environment' 'libxtst' 'fontconfig' 'freetype2'
                'lib32-gcc-libs' 'lib32-glibc' 'libx11' 'libxext' 'libxrender'
                'zlib')
optdepends=('android-emulator: emulator has become standalone since 25.3.0'
            'android-sdk-platform-tools: adb, aapt, aidl, dexdump and dx'
            'android-udev: udev rules for Android devices')
install="${pkgname}.install"
source=("https://android-rebuilds.beuc.net/dl/repository/sdk-repo-linux-tools-${pkgver}.zip"
        "${pkgname}.sh"
        "${pkgname}.csh"
        "${pkgname}.conf")
sha1sums=('c5b01d0ea1c998d4a9ca1f500fb24d3e8929bfcb'
          '30a6ed281d54f8b7be08663a18c367f79c0d8d47'
          '1bd09bf137fd09171cb426daa5748f117cfb3c25'
          '145bdf3eb41a56574b289c1577a24bc47097ec83')

package() {

  install -Dm755 "${pkgname}.sh" "${pkgdir}/etc/profile.d/${pkgname}.sh"
  install -Dm755 "${pkgname}.csh" "${pkgdir}/etc/profile.d/${pkgname}.csh"
  install -Dm644 "${pkgname}.conf" "${pkgdir}/etc/ld.so.conf.d/${pkgname}.conf"
  install -Dm644 "${srcdir}/tools/NOTICE.txt" "${pkgdir}/usr/share/licenses/${pkgname}/license.txt"
  install -d "${pkgdir}/opt/${pkgname}/platforms"
  install -d "${pkgdir}/opt/${pkgname}/add-ons"

  cp -a tools "${pkgdir}/opt/${pkgname}"

  if [[ $CARCH = i686 ]]; then
    rm -rf "${pkgdir}"/opt/android-sdk/tools/lib/{monitor-,}x86_64
  fi

  # Fix broken permissions
  chmod -R o=g "${pkgdir}/opt/${pkgname}"
  find "${pkgdir}/opt/${pkgname}" -perm 744 -exec chmod 755 {} +
}

# getver: https://developer.android.com/studio/releases/sdk-tools.html
# see https://dl.google.com/android/repository/repository2-1.xml for new versions
# vim:set ts=2 sw=2 et:
