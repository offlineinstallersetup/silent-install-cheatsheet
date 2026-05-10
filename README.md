# Silent Install Cheatsheet

> Practical reference of silent-install command-line switches for popular Windows software — for IT admins, MDT, SCCM, Intune, GPO scripts, and unattended deployment.

Maintained by [**OfflineInstallerSetup.com**](https://offlineinstallersetup.com/) — verified offline installers from official sources.

[![Reference](https://img.shields.io/badge/Reference-offlineinstallersetup.com-2563eb?style=flat-square&logo=googlechrome&logoColor=white)](https://offlineinstallersetup.com/)

> 📘 **See also:** [Silent install reference at offlineinstallersetup.com](https://offlineinstallersetup.com/resources/silent-install-reference/)

---

## Installer Type Detection

| Technology | Detection | Common Switches |
|-----------|-----------|----------------|
| **Windows Installer (MSI)** | `.msi` extension | `/qn` `/quiet` `/norestart` |
| **InstallShield (EXE wrapping MSI)** | Often `setup.exe` | `/s /v"/qn"` |
| **NSIS (Nullsoft)** | Small EXE | `/S` (case-sensitive) |
| **Inno Setup** | EXE with "innosetup" metadata | `/SILENT` or `/VERYSILENT` |
| **WiX Bundle** | EXE bootstrapper | `/quiet` `/passive` |

---

## Quick Reference — Popular Apps

### Web Browsers

| Software | Silent Command |
|----------|----------------|
| Google Chrome (Enterprise MSI) | `msiexec /i GoogleChromeStandaloneEnterprise64.msi /qn` |
| Mozilla Firefox | `Firefox\ Setup.exe -ms` |
| Microsoft Edge | `MicrosoftEdgeEnterpriseX64.msi /quiet /norestart` |
| Brave Browser | `BraveBrowserStandaloneSilentSetup.exe` |
| Opera | `Opera_Setup.exe /silent` |

### Media & Office

| Software | Silent Command |
|----------|----------------|
| VLC Media Player | `vlc-x.x.x-win64.exe /S` |
| LibreOffice | `msiexec /i LibreOffice_x.x.x_Win_x86-64.msi /qn` |
| 7-Zip | `7z-x.exe /S` |
| Notepad++ | `npp.x.x.x.Installer.x64.exe /S` |
| Audacity | `audacity-win-x.x.x-64bit.exe /SILENT` |

### Developer

| Software | Silent Command |
|----------|----------------|
| VS Code (System) | `VSCodeSetup-x64.exe /VERYSILENT /MERGETASKS=!runcode` |
| Git for Windows | `Git-x.x.x-64-bit.exe /VERYSILENT /NORESTART` |
| Node.js (MSI) | `msiexec /i node-vx.x.x-x64.msi /qn` |
| Python | `python-x.x.x-amd64.exe /quiet InstallAllUsers=1 PrependPath=1` |

### Communication

| Software | Silent Command |
|----------|----------------|
| Zoom | `msiexec /i ZoomInstallerFull.msi /qn` |
| Microsoft Teams | `msiexec /i Teams_windows_x64.msi /qn ALLUSER=1` |
| Slack | `msiexec /i SlackSetup.msi /qn` |

---

## Switch Reference by Technology

### MSI (Windows Installer)

```cmd
msiexec /i installer.msi /qn /norestart /l*v install.log
```

| Switch | Meaning |
|--------|---------|
| `/i` | Install |
| `/x` | Uninstall |
| `/qn` | No UI |
| `/qb` | Basic UI |
| `/quiet` | Same as /qn |
| `/norestart` | Suppress reboot |
| `/l*v file` | Verbose log |
| `PROPERTY=val` | Set MSI property |
| `TRANSFORMS=f.mst` | Apply transform |

### InstallShield

```cmd
setup.exe /s /v"/qn /norestart"
```

The `/v` passes everything in quotes to the embedded MSI.

### NSIS

```cmd
installer.exe /S
```

⚠️ Switch is **uppercase** `/S`.

### Inno Setup

```cmd
installer.exe /VERYSILENT /SUPPRESSMSGBOXES /NORESTART
```

| Switch | Meaning |
|--------|---------|
| `/SILENT` | No wizard, but progress shown |
| `/VERYSILENT` | No UI |
| `/SUPPRESSMSGBOXES` | Auto-dismiss prompts |
| `/NORESTART` | Skip reboot |
| `/DIR="path"` | Custom install dir |
| `/LOG="file"` | Write log |

---

## PowerShell Examples

```powershell
Start-Process -FilePath "installer.exe" -ArgumentList "/S" -Wait -NoNewWindow
```

---

## Resources

- 📦 [Verified offline installers — offlineinstallersetup.com](https://offlineinstallersetup.com/)
- 🛡️ [Verify installer safety](https://offlineinstallersetup.com/resources/verify-installer-safety/)
- 📘 [Offline installer availability reference](https://offlineinstallersetup.com/resources/offline-installer-availability/)
- ⚙️ [Full silent install reference](https://offlineinstallersetup.com/resources/silent-install-reference/)

---

## Contributing

Found a switch that's not documented? Open a PR or contact via [offlineinstallersetup.com/contact](https://offlineinstallersetup.com/contact/).

## License

[CC0 — Public domain](https://creativecommons.org/publicdomain/zero/1.0/).

---

🌐 Maintained by [**OfflineInstallerSetup.com**](https://offlineinstallersetup.com/)
