# Linux Logbook — Timeline (October – November 2026)

Chronological log reconstructed from `/var/log/pacman.log`, `/etc/` configuration files, `~/.local/bin/` scripts, and recorded work sessions.

---

## October 2026

### October 21 — Base EndeavourOS Installation
- **17:56 – 18:03:** Clean installation of **EndeavourOS** (`BUILD_ID=2026.08.15`, `+1320` base packages, `~50` updated).
- Initial environment: KDE Plasma, `plasmalogin.service`, `firewalld.service`, `NetworkManager`.
- **18:42 – 23:33:** Setup of work tools and initial environment:
  - `~/Aplicaciones/Antigravity-x64` and `~/.antigravity-ide`.
  - Initial scripts in `~/.local/bin/`: `entrar-a-bios`, `vc`/`vercel` symlinks.

### October 22 — BIOS, +4 GB RAM, AMD GPU & Gaming (WoW)
- **02:05 – 02:08:** BIOS tuning and RAM upgrade (+4 GB). Configured `/etc/modprobe.d/amdgpu.conf` (`options amdgpu ppfeaturemask=0xffffffff`) to unlock full power/clock control on Radeon 740M.
- Optimization of **World of Warcraft (ChromieCraft 3.3.5a)** in `~/Juegos/ChromieCraft_3.3.5a`:
  - Created `~/.local/bin/wow-addons-manager` and `~/.local/bin/convert-to-instagram`.
  - Installed `discord` (`pacman.log`: +1 package).

### October 23 — Web Development, Node & GPU Control
- Installed **13 packages** (`pacman.log`): `nodejs`, `npm`, `node-gyp`, `lact` (AMD GPU control), `opencode-beta`, `gst-plugins-good`, `qt6-multimedia-gstreamer`.
- Enabled `systemd-resolved` (`dbus-org.freedesktop.resolve1.service`) and created symlink `~/.local/bin/actualizar-sistema`.

### October 24 — Gaming Scripts & Addon Backups
- Created `~/.local/bin/modo-gamer` and `~/.local/bin/wow`.
- Addon backups stored in `~/optimizacion/backup-addons-20261024-1334` and `~/optimizacion/backup-blizzard-addons`.

### October 25 — USB Wi-Fi, Audio, Webcam, ZRAM & Docker
Key infrastructure day (`+38` packages installed in `pacman.log`):
- **USB Wi-Fi (`8188eu`):**
  - `/etc/modprobe.d/blacklist-rtl8xxxu.conf` (blacklisted conflicting generic `rtl8xxxu` driver).
  - `/etc/modprobe.d/8188eu.conf` (`rtw_power_mgnt=0 rtw_ips_mode=0 rtw_enusbss=0` to prevent USB adapter suspend/dropouts).
  - `/etc/sysctl.d/99-ip-forward.conf` (`net.ipv4.ip_forward = 1` for internet sharing/hotspot).
  - `/etc/NetworkManager/conf.d/dns.conf` (`dns=systemd-resolved`).
  - Created `airforge` and `hotspot` symlinks pointing to `baliza`.
- **Hardware & Performance:**
  - `/etc/udev/rules.d/60-nvme-performance.rules` (`scheduler="none"`, `read_ahead_kb="256"` for NVMe).
  - `/etc/udev/rules.d/99-logitech-c310.rules` + `guvcview-qt` for Logitech C310 webcam.
  - `/etc/modprobe.d/zswap.conf` (`enabled=0`) + `zram-generator` for compressed swap in RAM (`zstd`).
  - `piper` / `ratbagd.service` for mouse configuration.
- **Audio & Apps:**
  - `easyeffects`, `lsp-plugins-lv2`, `calf`, `webrtc-audio-processing`.
  - `whatsie-git` (WhatsApp Desktop client), `android-tools`, `android-udev`, `docker`, `docker-compose`.

### October 26 – 27 — Hyprland (testing), Debugging & CLI Tools
- **Oct 26:** Installed `valgrind`, `debuginfod`, `opencode-desktop-bin`, `electron44`; `agy` binary in `~/.local/bin/`.
- **Oct 27:** Installed `sbctl`, `hyprland`, `aquamarine`, `vswallpaper-effect`, `plasma6-wallpapers-smart-video-wallpaper-reborn` (`+53` packages). `antigravity` wrapper in `~/.local/bin/`.

### October 28 – 30 — KDE Power Management & Wi-Fi Hotspot Stabilization
- **Oct 28:** Tuned `~/.config/powerdevilrc` to delay sleep and screen-off to **120 minutes (7200 s)** to prevent Wi-Fi hotspot dropouts.
- **Oct 29 – 30:** Diagnosed and tuned `Red-Exeq-WiFi` on `wlan0` (channel 6, 2.4 GHz) for sharing internet to smartphone (POCO C65).

### October 31 — Initial WoW Audio Troubleshooting
- Updated `~/Juegos/ChromieCraft_3.3.5a/WTF/Config.wtf` (`Sound_EnableHardware "0"` due to FMOD incompatibility) and updated `~91` packages.

---

## November 2026

### November 1 — Kernel/GRUB Cleanup, Conflicting Node Binary & USB Wi-Fi
- `+58` packages installed (including `dkms`, `qemu-*`, `pass`, `tree`), `-7` removed (including `linux-lts`).
- **Critical environment fix:** Detected and removed a stale `node` binary (v20.18 in `~/.local/bin/node`, moved to `old_node_backup`) that was shadowing the system's `/usr/bin/node`.
- **USB Wi-Fi & Boot:** Recovered USB Wi-Fi adapter after hot-unplug and pruned stale bootloader entries (keeping primary kernel + fallback).

### November 2 — Permanent WoW Audio Fix
- Updated `~/.local/bin/jugar-wow`: pinned output to **System Default** and tuned latency buffer to prevent audio routing to secondary HDMI/DisplayPort sinks.

### November 3 — Wi-Fi Hotspot Watchdog & Android/MTK Tools
- Updated `~/.local/bin/baliza` and `~/.local/bin/compartir_internet.py`.
- Enabled `baliza-watchdog.service` (background user service protecting the hotspot link).
- Worked with Android/MediaTek tooling (`~/android_audit`, `~/lineageos_poco_c65`, `~/mtkclient`).

### November 4 — Space Desktop Wallpaper, ClamAV & Cleanup
- Created `~/.local/bin/cambiar-fondo-espacio` and desktop shortcut `~/Escritorio/Cambiar-Fondo-Espacial.desktop`.
- Installed `clamav`, `bleachbit`, and enabled `/etc/systemd/system/paccache.timer` and `paccache.service` for automated `pacman` cache pruning.

### November 5 – 8 — System Maintenance & Project Work
- **Nov 5:** `~37` packages updated.
- **Nov 7:** `~10` packages updated; created project directories under `~/Proyectos/`.

### November 9 — "TITAN" Security, Optimization & Control Suite
Major standardization and system hardening session (`02:47 – 03:03`):
- Created/updated suite scripts in `~/.local/bin/`:
  - `actualizar-todo` (with symlink `~/actualizar-todo.sh`)
  - `blindaje-seguridad` (with symlinks `~/aplicar-seguridad-root.sh` and `~/optimizacion/aplicar-optimizacion-sistema.sh`) — see full 8-layer breakdown in [SEGURIDAD.md](SEGURIDAD.md)
  - `instalar-seguridad-y-limpieza` (with symlink `~/instalar-seguridad-y-limpieza.sh`)
  - `limpiar-sistema`, `optimizar-ryzen`, `respaldo-usb`, `titan-doctor`, `titan-hardware-root`, `titan-oneclick`, `centro_de_control.py`
- English → Spanish user directory symlinks: `~/Documents -> ~/Documentos`, `~/Downloads -> ~/Descargas`, `~/Videos -> ~/Vídeos`.
- Verified AMD microcode and A620M H + Ryzen 5 8500G chipset configuration.

### November 10 – 11 — Mass Update, ChatGPT Desktop & Git
- **Nov 11:** `~150` packages updated (`openssl-3`, etc.).
- Diagnosed `chatgpt-desktop` / `~/.config/Codex` (stale `Singleton*` locks and gateway errors).
- Updated Git credentials in `~/.gitconfig` and `~/.git-credentials`.

### November 12 — Cursor, Persistent Hardening & Central Logbook
- **14:02 – 18:19:** Steam, `pm2`, and package updates (`~31` packages).
- **22:35 – 22:37:** Installed `cursor-bin` (`electron43`) and deployed/updated hardening files in `/etc/`:
  - `/etc/sysctl.d/50-cursor.conf`
  - `/etc/sysctl.d/98-network-security.conf` (TITAN Security: ICMP stealth mode and network hardening)
  - `/etc/sysctl.d/99-ryzen-gaming.conf` (TITAN Performance: Ryzen 5 8500G + TCP BBR + Wine/Proton)
  - `/etc/udev/rules.d/51-microcontrollers-mtk.rules` (ESP32, CP210x, MediaTek)
  - `/etc/NetworkManager/conf.d/00-mac-randomization.conf`
  - `/etc/systemd/system/blindaje-seguridad.service`
- **23:04 – 23:12:** Created this central workspace `/home/exeq/Proyectos/linux-escritorio-correcciones-programas` and overhauled the TITAN suite:
  - **Security (`blindaje-seguridad` / `titan-doctor`):** added `yama.ptrace_scope=1`, `randomize_va_space=2`, `sysrq=0`, `dev.tty.ldisc_autoload=0`, plus checks for `~/.seguridad` and `.pacnew` files.
  - **Performance (`blindaje-seguridad` / `titan-hardware-root`):** set `vm.swappiness=100` for `zstd` ZRAM and removed `drop_caches` to preserve useful page cache.
  - **Updates (`actualizar-todo`):** removed `--ignore` on `steam`/`wine`/`gamemode` (avoids partial upgrades on Arch) and added `.pacnew` detector in `/etc/`.
  - **TITAN Panel (`centro_de_control.py`):** moved subprocess telemetry (`systemctl`, `nmcli`, `docker`) into `TelemetryWorker(QThread)` to keep the UI responsive, filtered physical NIC traffic (excluding `lo`/`docker0`), added `amdgpu` GPU temperature, asynchronous `run_repair`, and `🔍 Ver Updates` button.
- **23:27:** Fixed **Antigravity** shortcuts and wrapper: resolved conflict between two installed packages (`antigravity-ide 2.6.0-1` from Oct 28 vs `antigravity-appimage 2.31.2-1` from Nov 12 22:36) plus a stale wrapper in `~/.local/bin/antigravity`; repointed `~/.local/bin/antigravity` and local `.desktop` entries to `/usr/bin/antigravity`.
- **23:29 – 23:50:** Comprehensive fixes for **TITAN Control Center**, hardware care, game optimization, and personal directory organization:
  - **PolKit loop fix (`centro_de_control.py` & `titan-doctor`):** replaced `firewall-cmd --list-all --zone=public` calls (which triggered `org.fedoraproject.FirewallD1.config.info` every 2 s) with direct reads of `/etc/firewalld/zones/public.xml`.
  - **Hardware care & state (`titan-oneclick`, `titan-hardware-root`, `blindaje-seguridad`, `centro_de_control.py`):** set `balanced` (`balance_performance`) profile for daily use (reserving pure `performance` for `modo-gamer`), `vm.compaction_proactiveness=0`, `transparent_hugepage=madvise` (`/etc/tmpfiles.d/titan-thp.conf`), **Aplicar Blindaje Root** button with live output in `konsole`, and fixed `is_gamer_mode_active()` path (`/run/user/1000/modo-gamer`).
  - **WoW 3.3.5a (`Warlogz` + `Config.wtf` + `jugar-wow`):** restored `Warlogz/macros-cache.txt` from `macros-cache.old` with `SET synchronizeMacros "0"`, removed global `INSTA` macro from `CANALLA01/macros-cache.txt`, 25-man raid optimizations (`processAffinityMask="4095"`, `M2Faster="3"`, `extShadowQuality="0"`, `gxTextureCacheSize="1024"`, `ffxGlow="0"`, `maxFPS="120"`, `maxFPSBk="30"`, `Sound_EnableSoftwareHRTF="0"`), and automatic `powerprofilesctl` restoration upon exiting `jugar-wow`.
  - **Dota 2 (`autoexec.cfg` & `video.txt`):** `fps_max="120"`, `fps_max_ui="60"`, disabled `r_depth_of_field`, `r_dota_bloom_compute_shader`, `r_dota_allow_wind_on_trees`, `r_dota_allow_parallax_mapping`, `r_dashboard_render_quality`, and `dsp_enhance_stereo`.
  - **Cleanup & path organization:** sorted `~/Descargas` into `~/Documentos/Descargas_Docs`, `~/Vídeos/Descargas_Videos`, `~/Imágenes/WhatsApp`, `~/Música/WhatsApp_Audios`, organized 98 screenshots into `~/Imágenes/Screenshots/{2026-10,2026-11}`, secured `Mozilla-Recovery-Key` in `~/.seguridad/documentos/` (`chmod 700/600`), and freed `~3.17 GB` of junk/duplicates.

### November 14 — TITAN Panel v2.2 (Linux Hacker / Matrix 60-30-10) & CachyOS-style Gaming Stack
- **TITAN Panel (`~/.local/bin/centro_de_control.py`):**
  - Full UI overhaul (`TitanControlCenter`, `LogViewerDialog`, `DoctorDialog`) to a **CRT Terminal / Retro Linux Hacker** aesthetic using a **60 / 30 / 10** color palette (`#040604` dark base, `#163616`/`#7bc47b` terminal green structure, `#ffb000` amber + `#00ff41` neon accents).
  - Added `MatrixHeaderFrame` (subtle ~12 FPS Matrix rain in header background) + blinking CRT cursor `TITAN://CORE_v2.2 █`.
  - Added **`:: GAMING_STACK [CACHYOS-STYLE / RADEON-740M]`** module with live telemetry for `ananicy-cpp`, `mangohud`, `gamescope`, and quick launch for `ProtonUp-Qt` and `Steam`.
- **Gaming stack installed & verified (without altering EndeavourOS repositories):**
  - Installed packages: `ananicy-cpp 1.2.0-1`, `cachyos-ananicy-rules-git 20260908.r2762.g03ef03fb-1`, `mangohud 0.8.4-1`, `lib32-mangohud 0.8.4-1`, `gamescope 3.17.0-1`, `protonup-qt-bin 2.15.1-1`.
  - `ananicy-cpp.service` enabled and active: detected Ryzen 5 8500G hybrid topology (`12 CPUs`, `big.LITTLE=yes`, P/E cores) and loaded `15812` CachyOS rules.
  - **`Proton-CachyOS` installed in Steam:** `proton-cachyos-11.0-20261015-slr-x86_64_v3` deployed to `~/.steam/root/compatibilitytools.d/`.
  - **`MangoHud`:** verified active `~/.config/MangoHud/MangoHud.conf` (toggle `Shift_R+F12`, `top-right`, `fps`, `frametime`, `cpu/gpu temp`, `ram`, `vram`, `gamemode`).
- **WoW 3.3.5a Optimization (`jugar-wow` + `dxvk.conf` + Zen 4 P-core affinity):**
  - Pinned live `Wow.exe` to the 4 Zen 4 Performance threads (`taskset -pc 0,4,6,10`).
  - Updated `~/.local/bin/jugar-wow` to enforce `SET processAffinityMask "1105"` in `Config.wtf` before each launch and export `MANGOHUD=1`.
  - Set `d3d9.samplerAnisotropy = 8` in `/home/exeq/Juegos/ChromieCraft_3.3.5a/dxvk.conf` to reduce DDR5 memory bandwidth pressure on the Radeon 740M.
- **New Animated Space Wallpaper ("Quantum Anomaly & Alien Monolith"):**
  - Generated 16:9 base image at `~/Imágenes/cosmic_anomaly_base.jpg` and rendered 1080p orbital loop at `~/Vídeos/wallpapers/espacio/cosmic_anomaly_loop.mp4`.
  - Integrated as option `5` in `~/.local/bin/cambiar-fondo-espacio` and activated in KDE Plasma 6 (`smart.video.wallpaper.reborn`).

### November 16 — DNS-over-TLS + BBR Timestamps & Path of Exile Stack
- **Network Optimization (`enp5s0` + `sysctl`):**
  - Enabled **strict DNS-over-TLS** (`connection.dns-over-tls 2`) and disabled **LLMNR** (`connection.llmnr 0`) on `Conexión cableada 1` (`systemd-resolved` with `1.1.1.1`).
  - Enabled `net.ipv4.tcp_timestamps = 1` in `/etc/sysctl.d/98-network-security.conf` and `~/.local/bin/blindaje-seguridad` for `TCP BBR` RTT accuracy and PAWS protection (using randomized per-connection offsets in modern kernels).
- **Path of Exile (Standalone) + Overlays + Radeon 740M Performance:**
  - **Path diagnosis & fix:** `PathOfExile.exe` had downloaded `Content.ggpk` (70 GB) into `~/` due to a missing `cd "$(dirname "$EXE")"` in the launcher; relocated to the `Standalone/pfx/.../Path of Exile/` prefix and fixed the working directory.
  - **Clean launcher separation (`~/.local/bin/` & `.desktop`):** `poe-jugar` (installed game + `poe-standalone` alias), `poe-instalar` (installer/repair), and `poe-overlays` / `poe-tools` (`Awakened-PoE-Trade`, `ExileCompass`, `Exilence-CE` without duplicate processes).
  - **Performance automation in `poe-jugar`:** `powerprofilesctl performance` with automatic restore to `balanced` on exit, env vars `AMD_VULKAN_ICD=RADV`, `RADV_PERFTEST=sam`, `MESA_SHADER_CACHE_MAX_SIZE=12G`, `WINEFSYNC=1`, `WINEESYNC=1`, `gamemoderun`, `MANGOHUD=1`, `--nologo`, and `production_Config.ini` auto-tuning (`Vulkan`, `texture_quality=Medium`, `shadow_type=Low`, `vsync_mode=Off`, `use_dynamic_particle_culling=true`, `framerate_limit=120`).
