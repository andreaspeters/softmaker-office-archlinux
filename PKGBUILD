# Maintainer: Muflone http://www.muflone.com/contacts/english/

pkgname=office
pkgver=1046
pkgrel=1
pkgdesc="A complete, reliable, lightning-fast and Microsoft Office-compatible office suite with a word processor, spreadsheet, and presentation graphics software."
arch=('x86_64')
url="http://www.softmaker.de/"
license=('custom')
depends=('libxrandr' 'libgl' 'xdg-utils' 'gtk-update-icon-cache'
         'desktop-file-utils' 'curl')
makedepends=('chrpath')
install="${pkgname}.install"
source=("http://www.softmaker.net/down/softmaker-${pkgname}-2021-${pkgver}-amd64.tgz"
        "${pkgname}-textmaker"
        "${pkgname}-planmaker"
        "${pkgname}-presentations"
        "${pkgname}-textmaker.desktop"
        "${pkgname}-planmaker.desktop"
        "${pkgname}-presentations.desktop")
sha256sums=('b3bf5f37da35cf38576cfca1556f5c380a96419cf87de7584a50340d6594d9ba'
            '25ab9aa51c13253e90cd2efccb3f8802da5e0127b1710f3559c391fa75b5a048'
            'e68c53076dde488f16fd640d792042e11d2733878db81c37fa6dc254355188cf'
            '84a58920ce186e117af30b7ee0f9e4dba9641dc0e8b7b744a2da01c0cb0b728b'
            '3fed2a063d837bdc869f7450cc9381a2a278d7f7fa173b6ca1f3ff778cd9bb03'
            'dd9593eb7409ee91385f53d4e4d91b043eaee46556dccc7a2a23ba1d43e2af6b'
            '2a7aa1b08fd7d058d16aa4150841c67c5863a372be366fbcc6aca36fc76a2319')

prepare() {
  xz -d "office2021.tar.lzma"
}

build() {
  [ -d "${pkgname}" ] && rm -rf "${pkgname}"
  mkdir "${pkgname}"
  tar x -f "office2021.tar" -C "${pkgname}"
  # Remove insecure RPATH
  cd "${pkgname}"
  chrpath --delete "textmaker"
  chrpath --delete "planmaker"
  chrpath --delete "presentations"
}

package() {
  # Install package files
  install -d "${pkgdir}/usr/lib"
  cp -r "${srcdir}/${pkgname}" "${pkgdir}/usr/lib"
  # Installing license file
  install -d "${pkgdir}/usr/share/licenses/${pkgname}"
  ln -s "/usr/lib/${pkgname}/mime/copyright" "${pkgdir}/usr/share/licenses/${pkgname}/license.txt"
  # Install program executables
  install -d "${pkgdir}/usr/bin"
  install -m 755 -t "${pkgdir}/usr/bin" "${srcdir}/${pkgname}-planmaker"
  install -m 755 -t "${pkgdir}/usr/bin" "${srcdir}/${pkgname}-textmaker"
  install -m 755 -t "${pkgdir}/usr/bin" "${srcdir}/${pkgname}-presentations"
  # Installing icons
  for size in 16 32 48
  do
    install -d "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps"
    ln -s "/usr/lib/${pkgname}/icons/pml_${size}.png" "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps/${pkgname}-planmaker.png"
    ln -s "/usr/lib/${pkgname}/icons/prl_${size}.png" "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps/${pkgname}-presentations.png"
    ln -s "/usr/lib/${pkgname}/icons/tml_${size}.png" "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps/${pkgname}-textmaker.png"

    install -d "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/mimetypes"
    ln -s "/usr/lib/${pkgname}/icons/pmd_${size}.png" "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/mimetypes/application-x-pmd.png"
    ln -s "/usr/lib/${pkgname}/icons/prd_${size}.png" "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/mimetypes/application-x-prd.png"
    ln -s "/usr/lib/${pkgname}/icons/tmd_${size}.png" "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/mimetypes/application-x-tmd.png"
  done
  # Install desktop files
  install -d "${pkgdir}/usr/share/applications"
  install -m 755 -t "${pkgdir}/usr/share/applications" "${pkgname}-textmaker.desktop"
  install -m 755 -t "${pkgdir}/usr/share/applications" "${pkgname}-planmaker.desktop"
  install -m 755 -t "${pkgdir}/usr/share/applications" "${pkgname}-presentations.desktop"
  # Installing mime file
  install -d "${pkgdir}/usr/share/mime/packages"
  install -m 644 -t "${pkgdir}/usr/share/mime/packages" "${srcdir}/${pkgname}/mime/softmaker-office-2021.xml"
}
