# System Inventory & Cleanup Plan

Verified map of custom scripts, symlinks, system configurations, and unorganized files as of **2026-11-12**.

> **Safety rule:** Do not move or delete scripts in `~/.local/bin/` without first updating any symlinks and `systemd` units pointing to them (e.g., `blindaje-seguridad.service` or `baliza-watchdog.service`).

---

## 1. Active Custom Scripts (`~/.local/bin/`)

### Maintenance, Security & Hardware ("TITAN")
- `~/.local/bin/actualizar-todo` (symlinked from `~/actualizar-todo.sh` and `~/.local/bin/actualizar-sistema`)
- `~/.local/bin/blindaje-seguridad` (symlinked from `~/aplicar-seguridad-root.sh` and `~/optimizacion/aplicar-optimizacion-sistema.sh`)
- `~/.local/bin/instalar-seguridad-y-limpieza` (symlinked from `~/instalar-seguridad-y-limpieza.sh`)
- `~/.local/bin/limpiar-sistema`
- `~/.local/bin/optimizar-ryzen`
- `~/.local/bin/titan-doctor`
- `~/.local/bin/titan-hardware-root`
- `~/.local/bin/titan-oneclick`
- `~/.local/bin/respaldo-usb`
- `~/.local/bin/centro_de_control.py`

### Networking & Wi-Fi Hotspot
- `~/.local/bin/baliza` (aliased via symlinks as `airforge` and `hotspot`)
- `~/.local/bin/compartir_internet.py`

### Gaming, Desktop & Utilities
- `~/.local/bin/jugar-wow`, `wow`, `modo-gamer`, `wow-addons-manager`
- `~/.local/bin/cambiar-fondo-espacio` (invoked by `~/Escritorio/Cambiar-Fondo-Espacial.desktop`)
- `~/.local/bin/convert-to-instagram`
- `~/.local/bin/entrar-a-bios`
- `~/.local/bin/agy`, `antigravity`, `gh`, `vc`, `vercel`

---

## 2. Custom `/etc/` Configurations

- **Kernel / Sysctl (`/etc/sysctl.d/`):**
  - `50-cursor.conf` — unprivileged user namespaces support for Cursor
  - `98-network-security.conf` — TITAN Security (ICMP/network hardening)
  - `99-ip-forward.conf` — `net.ipv4.ip_forward = 1` (Wi-Fi hotspot routing)
  - `99-ryzen-gaming.conf` — TITAN Performance (Ryzen 5 8500G + TCP BBR + Wine/Proton)
- **Kernel Modules (`/etc/modprobe.d/`):**
  - `amdgpu.conf` — `ppfeaturemask=0xffffffff`
  - `8188eu.conf` + `blacklist-rtl8xxxu.conf` — stable USB Wi-Fi driver with power saving disabled
  - `zswap.conf` — `enabled=0` (in favor of `zstd` ZRAM)
- **udev Rules (`/etc/udev/rules.d/`):**
  - `51-microcontrollers-mtk.rules` — ESP32 / CP210x / MediaTek
  - `60-nvme-performance.rules` — `scheduler=none`, `read_ahead_kb=256`
  - `99-logitech-c310.rules` — automatic configuration for Logitech C310 webcam
- **Networking (`/etc/NetworkManager/conf.d/`):**
  - `00-mac-randomization.conf`
  - `dns.conf` — `dns=systemd-resolved`

---

## 3. Detected Clutter (Pending Gradual Cleanup)

1. **Duplicate symlinks in `~/` (`$HOME`):**
   - `~/actualizar-todo.sh`, `~/aplicar-seguridad-root.sh`, `~/instalar-seguridad-y-limpieza.sh`, and `~/optimizacion/aplicar-optimizacion-sistema.sh` point to scripts already located in `~/.local/bin/` (in `$PATH`).
2. **`~/optimizacion/` directory:**
   - Contains `backups/`, `obsoleto/`, `backup-addons-20261024-1334/`, and `backup-blizzard-addons/` mixed in the user's home directory.
3. **`~/Descargas/` directory (68 items, ~3.4 GB):**
   - Already-applied or heavy installers: `docker-desktop-x86_64.pkg.tar.zst` (644 MB), `Antigravity.tar.gz` (175 MB), and `Antigravity (1).tar.gz` (175 MB).
   - Duplicate files: `MATEMATICA-Cursillo-Ingreso-Nivel-Superior-2027.pdf` and `(1).pdf`; `B2B_digital_strategy_outreach_guide_4c577bfa_1080p.mp4` and `(1).mp4`.
   - Already-extracted archives (`addon-file-24159-6ac28f23cee06.zip`, `AddOns(1).rar`, `ExequielEchevarria.zip`).
   - Personal/sensitive documents mixed with temporary downloads (`dni_frente.jpg`, `dni_dorso.jpg`, `Mozilla-Recovery-Key_...txt`).
4. **Loose scripts in `~/Proyectos/`:**
   - Multiple push/automation scripts at the root of `~/Proyectos/` (`auto_pusher_*.py`, `push_*.py`, `runner_30_pushes.py`, `GUIA_LEVANTAR_SERVIDORES.md`) outside a dedicated directory.
5. **Working directories in `~/` outside `~/Proyectos/`:**
   - `~/android_audit`, `~/lineageos_poco_c65`, `~/mtkclient`, `~/c-practica`.
