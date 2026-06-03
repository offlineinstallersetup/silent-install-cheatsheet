# Silent Install Cheatsheet

> Command-line switches for unattended / silent installation of popular Windows software — for SCCM (MECM), Microsoft Intune, Group Policy (GPO), MDT, PDQ Deploy, and PowerShell deployment scripts.

Maintained by **[OfflineInstallerSetup.com](https://offlineinstallersetup.com)** — verified offline installer links for 793+ Windows, macOS, and Linux applications.

> ⚠️ **Always test in a lab first.** Installer switches change between versions and editions. Verify against the vendor's own documentation before deploying at scale. Replace `x.x.x` with the actual version in the filename.

## Contents

- [Installer framework switches (universal)](#installer-framework-switches-universal)
- [How to identify the installer framework](#how-to-identify-the-installer-framework)
- [Per-app silent install commands](#per-app-silent-install-commands)
- [MSI standard properties](#msi-standard-properties)
- [Detecting success in scripts](#detecting-success-in-scripts)

---

## Installer framework switches (universal)

Most Windows installers are built with one of a handful of frameworks. Identify the framework, and the silent switch follows:

| Framework | Silent switch | Very-silent / no UI | No restart |
|-----------|---------------|---------------------|------------|
| **Windows Installer (MSI)** | `/qb` (basic UI) | `/qn` (no UI) | `/norestart` |
| **NSIS** | `/S` (capital S) | `/S` | `/NCRC` (skip CRC) |
| **Inno Setup** | `/SILENT` | `/VERYSILENT /SUPPRESSMSGBOXES` | `/NORESTART` |
| **InstallShield** | `/s /v"/qb"` | `/s /v"/qn"` | `/v"REBOOT=ReallySuppress"` |
| **WiX Burn bundle** | `/passive` | `/quiet` | `/norestart` |
| **Squirrel** | `--silent` | `--silent` | (auto) |
| **MSIX / AppX** | `Add-AppxPackage` | (always silent) | (n/a) |
| **InstallAware** | `/s` | `/s` | `/NoRestart` |

## How to identify the installer framework

```powershell
# Inspect the installer's metadata and embedded strings
Get-Item .\setup.exe | Select-Object VersionInfo

# NSIS installers contain the string "Nullsoft"; Inno contains "Inno Setup"
Select-String -Path .\setup.exe -Pattern "Nullsoft","Inno Setup","InstallShield" -Encoding Byte -ErrorAction SilentlyContinue
```

- **Nullsoft** → NSIS → use `/S`
- **Inno Setup** → use `/VERYSILENT /NORESTART`
- **InstallShield** → use `/s /v"/qn"`
- A `.msi` file → use `msiexec /i file.msi /qn /norestart`

## Per-app silent install commands

Each row links to the app's verified download page on OfflineInstallerSetup, where the current version and official source are listed.

| Software | Installer | Silent command | Download |
|----------|-----------|----------------|----------|
| 7-Zip | NSIS | `7zXXXX-x64.exe /S /D=C:\Program Files\7-Zip` | [link](https://offlineinstallersetup.com/7-zip-offline-installer/) |
| VLC Media Player | NSIS | `vlc-x.x.x-win64.exe /S` | [link](https://offlineinstallersetup.com/vlc-media-player-offline-installer/) |
| Notepad++ | NSIS | `npp.x.x.x.Installer.x64.exe /S` | [link](https://offlineinstallersetup.com/notepad-plus-plus-offline-installer/) |
| Mozilla Firefox | NSIS | `Firefox Setup x.x.exe -ms` | [link](https://offlineinstallersetup.com/mozilla-firefox-offline-installer/) |
| Google Chrome (Enterprise MSI) | MSI | `msiexec /i googlechromestandaloneenterprise64.msi /quiet /norestart` | [link](https://offlineinstallersetup.com/google-chrome-offline-installer/) |
| Git for Windows | Inno Setup | `Git-x.x.x-64-bit.exe /VERYSILENT /NORESTART /NOCANCEL /SP-` | [link](https://offlineinstallersetup.com/git-offline-installer/) |
| Visual Studio Code | Inno Setup | `VSCodeUserSetup-x64.exe /VERYSILENT /NORESTART /MERGETASKS=!runcode` | [link](https://offlineinstallersetup.com/visual-studio-code-offline-installer/) |
| Python | Custom (PEP) | `python-3.x.x.exe /quiet InstallAllUsers=1 PrependPath=1` | [link](https://offlineinstallersetup.com/python-offline-installer/) |
| Node.js | MSI | `msiexec /i node-vx.x.x-x64.msi /quiet /norestart` | [link](https://offlineinstallersetup.com/nodejs-offline-installer/) |
| Zoom | MSI | `msiexec /i ZoomInstallerFull.msi /quiet /norestart` | [link](https://offlineinstallersetup.com/zoom-offline-installer/) |
| OBS Studio | NSIS | `OBS-Studio-x.x-Full-Installer-x64.exe /S` | [link](https://offlineinstallersetup.com/obs-studio-offline-installer/) |
| Audacity | Inno Setup | `audacity-win-x.x.x-64bit.exe /VERYSILENT /NORESTART` | [link](https://offlineinstallersetup.com/audacity-offline-installer/) |
| GIMP | Custom | `gimp-x.x.x-setup.exe /VERYSILENT /NORESTART` | [link](https://offlineinstallersetup.com/gimp-offline-installer/) |
| LibreOffice | MSI | `msiexec /i LibreOffice_x.x.x_Win_x64.msi /quiet /norestart` | [link](https://offlineinstallersetup.com/libreoffice-offline-installer/) |
| Adobe Acrobat Reader DC | Custom | `AcroRdrDCx.exe /sAll /rs /msi EULA_ACCEPT=YES` | [link](https://offlineinstallersetup.com/adobe-acrobat-reader-dc-offline-installer/) |
| PuTTY | MSI | `msiexec /i putty-64bit-x.x-installer.msi /quiet /norestart` | [link](https://offlineinstallersetup.com/putty-offline-installer/) |
| FileZilla | NSIS | `FileZilla_x.x.x_win64-setup.exe /S` | [link](https://offlineinstallersetup.com/filezilla-offline-installer/) |
| VirtualBox | MSI/Custom | `VirtualBox-x.x.x-Win.exe --silent --ignore-reboot` | [link](https://offlineinstallersetup.com/virtualbox-offline-installer/) |
| 7-Zip Zstandard | MSIX | `Add-AppxPackage -Path .\NanaZip.msixbundle` | [link](https://offlineinstallersetup.com/nanazip-offline-installer/) |
| Rufus | Portable | `rufus.exe (portable — no install; use -g for GUI)` | [link](https://offlineinstallersetup.com/rufus-offline-installer/) |

> Full catalog of 725+ Windows apps with official download links: **[offlineinstallersetup.com/category/](https://offlineinstallersetup.com/all)**

## MSI standard properties

These work with *any* `.msi` via `msiexec`:

```bat
msiexec /i "App.msi" /qn /norestart ^
  ALLUSERS=1 ^
  REBOOT=ReallySuppress ^
  /L*v "C:\Logs\App_install.log"
```

| Property | Effect |
|----------|--------|
| `/qn` | No UI (fully silent) |
| `/qb` | Basic UI (progress bar only) |
| `/norestart` | Suppress reboot |
| `REBOOT=ReallySuppress` | Harder reboot suppression |
| `ALLUSERS=1` | Per-machine install |
| `/L*v file.log` | Verbose logging |

## Detecting success in scripts

```powershell
$p = Start-Process -FilePath "msiexec.exe" `
  -ArgumentList '/i "App.msi" /qn /norestart' `
  -Wait -PassThru
# 0 = success, 3010 = success but reboot required
if ($p.ExitCode -in 0,3010) { "OK ($($p.ExitCode))" } else { "FAILED $($p.ExitCode)" }
```

| Exit code | Meaning |
|-----------|---------|
| `0` | Success |
| `1641` | Success, reboot initiated |
| `3010` | Success, reboot required |
| `1603` | Fatal error during install |
| `1618` | Another install already in progress |

---

## Contributing

Found a switch that changed, or one we're missing? [Open an issue](../../issues) or PR. Please cite the vendor's documentation.

## Related

- [awesome-offline-installers](https://github.com/offlineinstallersetup/awesome-offline-installers) — curated offline installer list
- [enterprise-software-deployment](https://github.com/offlineinstallersetup/enterprise-software-deployment) — SCCM/Intune/MDT guides
- [windows-installer-downloads](https://github.com/offlineinstallersetup/windows-installer-downloads) — direct Windows installer links

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/) — public domain.
