# System Inventory & Cleanup Plan

Verified map of custom scripts, symlinks, system configurations, and unorganized files (updated **2026-11-25**).

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
- `~/.local/bin/titan-game-run` (`v3.1` CachyOS-style gaming wrapper: holds `amd-pstate-epp=performance` via `powerprofilesctl launch`, unlocks Radeon 740M DPM to `2799 MHz` via Polkit `gpuclockctl`, routes Dota 2 to `<1s` NVMe RADV shader cache `~/.cache/dota2_radv_cache`, and read-only locks `video.txt` `chmod 444` with `r_low_latency=0` against the `Mesa 26.3.1` `VK_AMD_anti_lag` sleep bug)
- `~/.local/bin/jugar-wow`, `wow`, `modo-gamer`, `wow-addons-manager`
- `~/.local/bin/cambiar-fondo-espacio` (invoked by `~/Escritorio/Cambiar-Fondo-Espacial.desktop`)
- `~/.local/bin/cambiar-fondo-matrix` (activates 1080p Matrix Digital Rain wallpaper loop with `PauseMode=3` for zero gaming overhead)
- `~/.local/bin/matrixctl` (master orchestrator for `~/Proyectos/matrix-os`: `apply`, `profile`, `wallpaper`, `hub`, `cockpit`, `doctor`, `install-deps`, `rollback`)
- `~/.local/bin/matrix-hud-probe` (low-overhead JSON `/proc` + `/sys` + Dev/Ops/Audio telemetry probe for `org.matrix.hud` v2)
- `~/.local/bin/matrix-deploy-hub` (PyQt6 CRT GUI + `--tui` monitor for dev ports, PM2, Docker, Git radar, Vercel/GH)
- `~/.local/bin/matrix-cockpit` (4-pane Operator Cockpit launcher via Zellij or native Kitty 0.50 splits)
- `~/.local/bin/convert-to-instagram`
- `~/.local/bin/entrar-a-bios`
- `~/.local/bin/reparar-linux` (one-shot root repair: validates UKI `.cmdline` with `objcopy`, syncs idempotent `blindaje-seguridad`, masks non-UKI `dracut` hooks, sets `WIRELESS_REGDOM="AR"`, disables `irqbalance`/`usbguard`)
- `~/.local/bin/limpiar-disco-root` (system cleanup: orphan purge, `paccache -rk0`, orphaned system Flatpak `ostree prune`, journal vacuum, `fstrim -a`)
- `~/.local/bin/agy`, `antigravity`, `gh`, `vc`, `vercel`
- `~/.config/systemd/user/docker-desktop.service.d/timeout.conf` (`TimeoutStopSec=15s` shutdown hang fix)
- `~/Documents/MANUAL_LINUX_ENDEAVOUROS_EXEQ.md` (offline host-specific Linux & UKI reference manual)
- `~/Desktop/LEEME_INICIAR_DESDE_BASH.txt` & `~/Documents/LEEME_INICIAR_DESDE_BASH.txt` (emergency low-resource bash / TTY rescue guide)

### Desktop Aesthetics, Terminal & Telemetry Paths (XDG Standard)
- `~/.config/kitty/kitty.conf` (`JetBrainsMono Nerd Font`, `#000000` / `#00ff41`, `0.90` opacity, `zsh`)
- `~/.config/starship.toml`, `~/.zshrc`, `~/.cache/zsh/` (`zsh-autosuggestions` + `zsh-syntax-highlighting`)
- `~/.config/fastfetch/{config.jsonc,matrix-logo.txt}`
- `~/.config/btop/{btop.conf,themes/matrix.theme}`, `~/.config/cava/config`, `~/.config/yakuakerc`
- `~/.config/autostart/org.kde.yakuake.desktop` (Quake-style `F12` dropdown terminal on login)
- `~/.config/kscreenlockerrc` (`matrix_digital_rain_static.png`)
- `~/.local/share/color-schemes/MatrixPhosphor.colors` & `~/.local/share/plasma/desktoptheme/Matrix/`
- `~/.local/share/icons/Matrix-Green/` (linked at `~/.icons/Matrix-Green` for Plasma cursor discovery) & `Papirus-Dark` (`green` folders)
- `~/.local/share/konsole/Matrix.{colorscheme,profile}`
- `~/.local/share/plasma/plasmoids/org.matrix.hud/`
- `~/Vídeos/wallpapers/matrix/matrix_digital_rain_loop.mp4` & `~/Imágenes/matrix_digital_rain_static.png`
- `~/backup-pre-rice/` (pre-rice dotfiles backup + `rollback.sh`)

---

## 2. Custom `/etc/` Configurations

- **Kernel / Sysctl (`/etc/sysctl.d/`):**
  - `50-cursor.conf` — unprivileged user namespaces support for Cursor
  - `98-network-security.conf` — TITAN Security (ICMP/network hardening)
  - `99-ip-forward.conf` — `net.ipv4.ip_forward = 1` (Wi-Fi hotspot routing)
  - `99-ryzen-gaming.conf` — TITAN Performance (Ryzen 5 8500G + TCP BBR + Wine/Proton)
- **Boot / UKI / Dracut (`/etc/kernel/`, `/etc/dracut.conf.d/`):**
  - `/etc/kernel/cmdline` & `/etc/dracut.conf.d/99-matrix-cmdline.conf` — synchronized full UKI kernel command line including `rw root=UUID=...`, Plymouth `quiet splash`, and AppArmor LSM stack
  - `/etc/kernel/install.conf` (`0644`, `layout=uki`)
  - `/etc/kernel/install.d/{50-dracut-kifd,50-dracut-fallback,90-loaderentry-kifd,90-loaderentry-fallback}.install -> /dev/null` — masked duplicate non-UKI hooks
- **Kernel Modules (`/etc/modprobe.d/`, `/etc/modules-load.d/`):**
  - `amdgpu.conf` — `ppfeaturemask=0xffffffff`
  - `8188eu.conf` + `blacklist-rtl8xxxu.conf` — stable USB Wi-Fi driver with power saving disabled
  - `zswap.conf` — `enabled=0` (in favor of `zstd` ZRAM)
  - `snd-hda-intel-ultra.conf` (`power_save=0`) + `usbhid-1000hz.conf` (`1ms` USB polling)
  - `/etc/modules-load.d/bbr.conf` — `tcp_bbr` + `sch_fq` (fixes early-boot `systemd-sysctl` `default_qdisc` error)
  - `/etc/conf.d/wireless-regdom` — `WIRELESS_REGDOM="AR"`
- **udev Rules (`/etc/udev/rules.d/`):**
  - `51-microcontrollers-mtk.rules` — ESP32 / CP210x / MediaTek
  - `60-nvme-performance.rules` — `ENV{DEVTYPE}=="disk"`, `scheduler=none`, `read_ahead_kb=256`, `rq_affinity=2`
  - `65-usb-peripherals-lowlatency.rules` — disables USB autosuspend on mouse, keyboard, and webcam
  - `70-poe-overlays-uinput.rules` — non-root `/dev/uinput` access for Wayland overlays
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
