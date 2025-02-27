# Pacscript MIME integration

This repo hosts the minimal files for pacscript MIME integration on desktop environments.

Dependencies:
- `xdg-utils`
- `desktop-file-utils`
- `shared-mime-info`
- `gtk-update-icon-cache` (GTK-based environments only)

Installation:
```bash
MIMEDIR="/usr/share/mime"
APPDIR="/usr/share/applications"
ICONDIR="/usr/share/icons/hicolor"
sudo mkdir -p "${ICONDIR}/scalable/mimetypes" "${MIMEDIR}/packages" "${APPDIR}"
sudo install -Dm644 "application-x-pacscript.svg" -t "${ICONDIR}/scalable/mimetypes"
sudo install -Dm644 "pacscript.xml" -t "${MIMEDIR}/packages"
sudo install -Dm644 "pacscript.desktop" -t "${APPDIR}"
sudo update-mime-database "${MIMEDIR}" 2>/dev/null
sudo update-desktop-database "${APPDIR}"
if command -v update-icon-caches > /dev/null; then
  sudo update-icon-caches "${ICONDIR}"
fi
{ ! [[ -f "${APPDIR}/mimeapps.list" ]] && echo -e "\n[Default Applications]";
  echo "application/x-pacscript=pacscript.desktop"; } | sudo tee -a "${APPDIR}/mimeapps.list"
```
