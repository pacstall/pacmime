# Pacscript MIME integration

This repo hosts the minimal files for pacscript MIME integration on desktop environments.

Dependencies:
- `xdg-utils`
- `desktop-file-utils`
- `shared-mime-info`
- `gtk-update-icon-cache` (GTK-based environments only)

Installation:
```bash
# Create directories
MIMEDIR="/usr/share/mime"
APPDIR="/usr/share/applications"
ICONDIR="/usr/share/icons/hicolor"
sudo mkdir -p "${ICONDIR}/scalable/mimetypes" "${MIMEDIR}/packages" "${APPDIR}"

# Install files to their locations
sudo install -Dm644 "application-x-pacscript.svg" -t "${ICONDIR}/scalable/mimetypes"
sudo install -Dm644 "pacscript.xml" -t "${MIMEDIR}/packages"
sudo install -Dm644 "pacscript.desktop" -t "${APPDIR}"

# Update caches
sudo update-mime-database "${MIMEDIR}" 2>/dev/null
sudo update-desktop-database "${APPDIR}"
if command -v update-icon-caches > /dev/null; then
  sudo update-icon-caches "${ICONDIR}"
fi

# Set as default MIME type
if ! { [[ -f "${APPDIR}/mimeapps.list" ]] && \
    grep -q '^application/x-pacscript=' "${APPDIR}/mimeapps.list";
  }; then
    { ! [[ -f "${APPDIR}/mimeapps.list" ]] && echo -e '\n[Default Applications]';
      echo 'application/x-pacscript=pacscript.desktop';
    } | sudo tee -a "${APPDIR}/mimeapps.list" > /dev/null
fi
```
