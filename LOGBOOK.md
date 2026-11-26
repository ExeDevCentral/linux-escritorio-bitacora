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
  - `blindaje-seguridad` (with symlinks `~/aplicar-seguridad-root.sh` and `~/optimizacion/aplicar-optimizacion-sistema.sh`) — see full 8-layer breakdown in [SECURITY.md](SECURITY.md)
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

### November 17 — Full Matrix Phosphor Desktop Rice (`#00ff41`) & XDG Standardization
- **Pre-Rice Backup & Rollback (`~/backup-pre-rice/`):**
  - Snapshotted `~/.config` (excluding heavy browser/Electron caches documented in `EXCLUDED_DIRS.txt`), `$HOME` shell/GTK dotfiles, and `~/.local/share/{icons,konsole,plasma}` (`11 MB` total), with one-command rollback script `~/backup-pre-rice/rollback.sh`.
- **Plasma 6 Wayland, GTK & Cursor Theme (preserving `AllowTearing=true`, G203 `PointerAcceleration=0.000`, and `/etc/sysctl.d/*`):**
  - Created and applied `MatrixPhosphor` color scheme (`#000000` / `#00ff41`, `AccentColor=0,255,65`), `Matrix` Plasma desktop theme, synchronized GTK 2/3/4 CSS (`colors.css` + `gtk.css`), custom multi-size `Matrix-Green` Xcursor theme (`~/.local/share/icons/Matrix-Green` + `~/.icons/Matrix-Green`), and `Papirus-Dark` with green folders (`papirus-folders` AUR).
  - Rendered seamless 1080p/30fps Matrix Digital Rain loop (`~/Vídeos/wallpapers/matrix/matrix_digital_rain_loop.mp4`) and static frame (`~/Imágenes/matrix_digital_rain_static.png`), activated via `~/.local/bin/cambiar-fondo-matrix` with `PauseMode=3` (0% GPU/CPU overhead during fullscreen games) and applied to `kscreenlockerrc`.
- **Terminal-First Stack & Panel Telemetry (XDG paths):**
  - Installed `kitty`, `ttf-jetbrains-mono-nerd`, `zsh`, `zsh-autosuggestions`, `zsh-syntax-highlighting`, `starship`, `fastfetch`, `btop`, `cava`, `yakuake`, `papirus-icon-theme`, and `papirus-folders` (AUR), switching login shell to `/usr/bin/zsh` and setting `kitty` as KDE default terminal (`TerminalApplication=kitty`).
  - Configured `~/.config/kitty/kitty.conf`, `~/.local/share/konsole/Matrix.{colorscheme,profile}`, `~/.zshrc` (preserving all TITAN/Git/Dev aliases and `~/.config/env/secrets.env`), `~/.config/starship.toml`, `~/.config/fastfetch/`, `~/.config/btop/`, `~/.config/cava/`, `~/.config/yakuakerc` + autostart (`F12` dropdown), and custom Plasma 6 panel widget `org.matrix.hud` backed by `~/.local/bin/matrix-hud-probe`.

### November 18 — PROJECT ZION // MATRIX OS v3.0 (Living Obsidian Desktop, GLSL Shader, HUD v2 & Declarative Routing)
- **Single-Source-of-Truth Routing (`~/Proyectos/matrix-os` + `matrixctl`):**
  - Created pre-v3 atomic snapshot in `~/.local/state/matrix-os/snapshots/pre-v3-*` and deployed declarative repo `~/Proyectos/matrix-os/` managed by `~/.local/bin/matrixctl` (`apply`, `profile cyber|focus|gamer`, `wallpaper shader|video`, `hub`, `cockpit`, `doctor`, `install-deps`, `rollback`).
  - Symlinked dotfiles (`~/.bashrc`, `~/.zshrc`, `~/.config/shell/matrix-core.sh`, `starship.toml`, `kitty.conf`, `kwinrulesrc`, `MatrixObsidian.colors`, `panel-colorizer` preset, `.desktop` launchers) and synced Plasma 6 KPackages (`org.matrix.hud`, `org.matrix.shaderwall`, `MatrixObsidian` desktop theme) complying with Plasma 6 canonicalPath sandboxing.
- **Obsidian Phosphor Optics, KWin 6.8.3 Motion & 2×2 Virtual Workspaces:**
  - Created and applied `MatrixObsidian` color scheme & Plasma theme (`AdaptiveTransparency=true`, layered obsidian tones + `#00ff41` / `#00e5ff` / `#ffb000` / `#ff2a55` telemetry accents).
  - Configured 4 virtual desktops in a `2×2` grid (`01:CODE`, `02:OPS`, `03:NET`, `04:GAME`), live KWin effects (`blur`, `diminactive`, `glide`, `magiclamp`, `slide`, `translucency`, `wobblywindows`), and tactical `kwinrulesrc` (borderless Kitty/Yakuake, translucent Deploy Hub/TITAN, 100% opaque zero-overhead gaming bypass) while strictly preserving `AllowTearing=true`.
- **Native Qt6 GLSL Shader Wallpaper (`org.matrix.shaderwall`) & Floating Panel + HUD v2:**
  - Authored and compiled 3-layer parallax GLSL 440 Matrix rain fragment shader (`matrix_rain.frag` → `matrix_rain.frag.qsb` via `/usr/lib/qt6/bin/qsb`) with CRT scanlines, radial obsidian vignette, and native `TaskManager.TasksModel` 0%-GPU auto-pause on fullscreen/maximized windows or `modo-gamer`.
  - Mounted `luisbocanegra.panel.colorizer` with custom `Matrix Obsidian Floating` preset and upgraded `org.matrix.hud` to v2 (live CPU sparkline canvas, pulsing status LED, animated 5-bar PipeWire/activity equalizer, Dev/Ops badge, and expandable Plasma 6 tactical drawer with 1-click profile switching).
- **Deploy & Dev Hub (`matrix-deploy-hub`), TITAN Integration & Unified Living Terminals:**
  - Built `~/.local/bin/matrix-deploy-hub` (PyQt6 CRT GUI + `--tui` live terminal mode) scanning local dev ports, PM2, Docker, dirty Git repos in `~/Proyectos`, and on-demand Vercel (`vc`) / GitHub (`gh`) runs; wired quick launchers `[kitty]`, `[deploy-hub]`, and `[cockpit]` into `centro_de_control.py` and linked `modo-gamer` to auto-toggle `matrixctl profile gamer`.
  - Upgraded Kitty `0.50.0` (`cursor_trail 3`, `background_blur 32`, `background_opacity 0.84`, `allow_remote_control yes`, `cockpit.session` 4-pane split layout), unified `~/.bashrc` and `~/.zshrc` via `~/.config/shell/matrix-core.sh` + enhanced `starship.toml`, and upgraded `btop`/`cava`/`yakuake` themes.
- **Audio Hi-Res, Display/Wayland, Low-Latency Network, Input (1000 Hz) & System Stability Tuning (21:15 – 21:21):**
  - **Audio (`PipeWire` + `WirePlumber` + `ALC897` + `EasyEffects`):** upgraded ALSA hardware output from default 16-bit / 1024 period to `S32_LE` (24-bit native DAC precision), `48000 Hz`, `period-size 256` (`4` periods, `headroom 128`), and `session.suspend-timeout-seconds = 0` (`~/.config/wireplumber/wireplumber.conf.d/50-alsa-hi-res-low-latency.conf`); configured PipeWire studio mixing (`quantum 256`, `resample.quality 10`, `wannamaker3` noise-shaped dithering in `~/.config/pipewire/pipewire.conf.d/99-studio-quality.conf` and `pipewire-pulse.conf.d/99-low-latency-pulse.conf`), disabled `snd_hda_intel` power-saving (`power_save=0`, `power_save_controller=N` in `/etc/modprobe.d/snd-hda-intel-ultra.conf`), added realtime privileges (`/etc/security/limits.d/95-pipewire-realtime.conf`), and created the `TITAN-Studio-Mastering` preset in `~/.config/easyeffects/output/TITAN-Studio-Mastering.json`.
  - **Display & Graphics (`KWin 6` + `Radeon 740M` + `Fontconfig` + `VA-API`):** calibrated `HDMI-A-1` (1080p@75Hz, 10-bit bpc, `Full RGB`, `prefer_accuracy`), set KWin `LatencyPolicy=ExtremelyLow` and `AnimationDurationFactor=1.0`, configured LCD RGB subpixel font rendering (`~/.config/fontconfig/fonts.conf` & `kcmfonts`), exported Radeon 740M VA-API/VDPAU (`radeonsi`) + `RADV` + `12G` Mesa shader cache (`~/.config/plasma-workspace/env/10-amd-radeon-media.sh`), and enabled zero-copy GPU rasterization + VA-API + QUIC in `~/.config/chromium-flags.conf` and `~/.config/electron-flags.conf`.
  - **Internet & Ethernet (`enp5s0` + `TCP BBR` + `Anti-Bufferbloat`):** enabled hardware GRO/GSO/TSO and UDP GRO forwarding (`rx-gro-list on`, `rx-udp-gro-forwarding on`, `txqueuelen 2000`) on `enp5s0`, added `net.ipv4.tcp_notsent_lowat = 131072`, `net.ipv4.tcp_ecn = 2`, `tcp_tw_reuse = 1`, and high-speed socket buffers in `/etc/sysctl.d/99-ryzen-gaming.conf`, disabled mDNS and WoL on `Conexión cableada 1`, and enabled local DNS caching in `systemd-resolved`.
  - **Keyboard & Mouse (`G203` + `Sino Wealth` + `usbhid`):** enforced kernel-level 1000 Hz (`1 ms`) USB polling (`mousepoll=1 kbpoll=1 jspoll=1` in `/etc/modprobe.d/usbhid-1000hz.conf`), disabled USB autosuspend on mouse, keyboard, and webcam (`/etc/udev/rules.d/65-usb-peripherals-lowlatency.rules`), tuned keyboard repeat (`RepeatDelay=200`, `RepeatRate=45` in `~/.config/kcminputrc`) preserving G203 `PointerAcceleration=0.000` flat profile, and added `blockcompositing=true` + `tearing=true` to the KWin gaming rule.
  - **System Stability (`udev` + `firewalld` + `VM`):** fixed `/etc/udev/rules.d/60-nvme-performance.rules` (`SUBSYSTEM=="block", KERNEL=="nvme[0-9]*n[0-9]*"` with `rq_affinity=2`) eliminating partition `udev-worker` errors in `journalctl`, resolved `firewalld` `ZONE_CONFLICT` on `docker0`, and replaced percentage-based `dirty_ratio` with fixed `vm.dirty_bytes = 268435456` (256 MB) / `vm.dirty_background_bytes = 67108864` (64 MB), `vm.watermark_scale_factor = 125`, and `kernel.sched_autogroup_enabled = 1`.
- **Path of Exile Addons & Overlays Comprehensive Repair (21:45 – 21:51):**
  - **Broken AppImage Symlink & Auto-Updater Protection (`awakened-poe-trade`, `exile-compass`, `exilence-ce`):** `Awakened-PoE-Trade` had auto-updated to `3.30.101` and deleted `3.29.108`, leaving `Awakened-PoE-Trade.AppImage` as a broken symlink (`No such file or directory`); repointed the symlink and updated all three launchers in `~/.local/bin/` to dynamically detect and relink the latest versioned `.AppImage` on startup.
  - **Pre-configured Paths & Account (`Client.txt`, `production_Config.ini`, `Filtros`, `Builds`):** populated `clientLog` (`/home/exeq/Juegos/PathOfExile/Logs/Client.txt`), `gameConfig` (`/home/exeq/Juegos/PathOfExile/Filtros/production_Config.ini`), and `accountName` (`maniatic02#4455`) in `~/.config/awakened-poe-trade/apt-data/config.json`; populated `EXILECOMPASS_POE1_LOG_FILE_PATH_V1`, `EXILECOMPASS_POE2_LOG_FILE_PATH_V1`, and `EXILECOMPASS_LOG_FILE_PATH_V1` in `~/.config/com.juddisjudd.exilecompass/settings.json`; created standard symlink `~/Documents/My Games/Path of Exile -> ~/Juegos/PathOfExile/Filtros`.
  - **Path of Building Flatpak Sandbox & Builds Link (`community.pathofbuilding.PathOfBuilding`):** granted `--filesystem=/home/exeq/Juegos/PathOfExile:rw`, `--filesystem=xdg-documents:rw`, and `--filesystem=xdg-download:rw` via `flatpak override --user`, and linked PoB's internal `Builds` folder directly to `/home/exeq/Juegos/PathOfExile/Builds`.
  - **Wayland / Xwayland Overlay Hotkeys, XKB Map & Always-On-Top KWin Rule:** enabled `XwaylandEavesdrops=all` and `XwaylandEavesdropsMouse=true` in `kwinrc`, added `setxkbmap us` + `--ozone-platform=x11` / `GDK_BACKEND=x11` in addon launchers (fixing `XkbGetKeyboard failed to locate a valid keyboard!`), added KWin rule `[5]` (`above=true`, `noborder=true`) for `awakened-poe-trade|exilecompass`, updated `poe-ahk` to run inside the PoE Proton prefix, added `/dev/uinput` + `input` group rule in `blindaje-seguridad`, and updated `poe-tools` to launch overlays as persistent `systemd-run --user` services.
- **ViewSonic VX2457-mhd Hardware DDC/CI Tuning & Native EDID Color Calibration (22:53 – 22:56):**
  - **Hardware Inspection (`ddcutil` on `/dev/i2c-2` + `fwupdmgr`):** identified monitor as **ViewSonic VX2457** (`VSC-VX2457`, Product Code `0xb931`, Mfg Year `2016/W51`, `15,032` hours runtime, internal **Novatek** scaler controller `0x12`, ROM firmware `0.0` non-flashable via HDMI/LVFS).
  - **Native EDID Color Profile & Hardware VCP Calibration:** switched KWin Wayland `HDMI-A-1` color profile source from generic `sRGB` to native hardware `EDID` (`Gamma 2.2`, exact panel RGB chromaticity primaries), calibrated the Novatek controller via DDC/CI (`Brightness=90`, `Contrast=72`, `Sharpness=75`, `Color Preset=6500K D65`), added `i2c` group and `i2c-dev` module persistence in `blindaje-seguridad`, and created `~/.local/bin/monitor-ctl` (`status`, `gamer`, `studio`, `noche`, `brillo`, `contraste`, `nitidez`).

### November 19 — Awakened PoE Trade `app.asar` ENOENT & FUSE Mount Crash Fix + System-Wide US English Locale & Keyboard Lock
- **Awakened PoE Trade Root Cause (`Error: ENOENT, logs not found in /tmp/.mount_Awaken.../resources/app.asar` & `status=7/BUS`):**
  1. In `main.js`, Awakened PoE Trade's embedded HTTP server (`127.0.0.1:<port>`) handles static requests with `fs.createReadStream(path.join(__dirname, r)).pipe(t)` without attaching an `.on("error", ...)` listener to the `ReadStream`. Any request to a non-existent route (such as `/logs`) throws an unhandled `ENOENT` stream error on `app.asar`, triggering Electron's fatal `A JavaScript error occurred in the main process` dialog.
  2. Running directly from a FUSE-mounted AppImage in `/tmp/.mount_Awaken*` caused `SIGBUS` (`status=7/BUS`) crashes during long gaming sessions or service restarts when the temporary FUSE mount invalidated open file descriptors.
- **Permanent Fix (`~/.local/bin/awakened-poe-trade` & `~/Juegos/PathOfExile/Awakened-PoE-Trade/appdir`):**
  - Extracted the AppImage permanently into `~/Juegos/PathOfExile/Awakened-PoE-Trade/appdir` (eliminating `/tmp/.mount_*` FUSE volatility) and patched `appdir/resources/app.asar` (`main.js` + SHA-256 ASAR header integrity) so `createReadStream` strips query strings and catches `error` events cleanly with HTTP `404` (`S.on("error",()=>{t.statusCode=404,t.end()})`).
  - Updated `~/.local/bin/awakened-poe-trade` to automatically re-extract and re-apply the binary ASAR patch whenever the auto-updater downloads a newer `Awakened-PoE-Trade-*.AppImage`.
- **Recovery: KDE Plasma 6 (EndeavourOS + Matrix KWin FX) Restored as Primary Desktop, 100% English OS, XDG Dirs & US Keyboard (05:09 – 05:19):**
  - **Primary Desktop Restored to KDE Plasma 6 (`plasma.desktop`):** reverted `plasmalogin` default in `~/.local/bin/blindaje-seguridad` from `hyprland.desktop` back to `/usr/share/wayland-sessions/plasma.desktop` (preserving Hyprland as optional secondary session), restoring full EndeavourOS Plasma 6 workflow and KWin 6 Matrix FX (`kwin6_effect_tv_glitch` in `#00ff41` for window open/close, `better_blur_dx`, `kwin4_effect_shapecorners`, `kwin4_effect_geometry_change`, `wobblywindows`, `magiclamp`, `diminactive`, `sheet`, `slide`, `mouseclick`, plus `org.matrix.shaderwall` and `org.matrix.hud`).
  - **100% English (`en_US.UTF-8`) & US Keyboard (`us`) Locked Everywhere (`es` purged):** enforced `us` keyboard layout live in Hyprland (`hyprctl reload`), `~/.config/hypr/hyprland.lua`, `~/.config/kxkbrc`, `~/.config/environment.d/00-english-locale.conf`, `~/.config/plasma-workspace/env/00-english-locale.sh`, `~/.config/shell/matrix-core.sh`, `~/.local/bin/{poe-tools,awakened-poe-trade,exile-compass}`, and `~/.local/bin/blindaje-seguridad` (`/etc/locale.conf`, `/etc/vconsole.conf`, `/etc/X11/xorg.conf.d/00-keyboard.conf`), with layout-switching hotkeys disabled in `~/.config/kglobalshortcutsrc`.
  - **Standardized English XDG User Directories (Zero Data Loss):** migrated `~/Escritorio → ~/Desktop`, `~/Descargas → ~/Downloads`, `~/Documentos → ~/Documents` (merged cleanly with `My Games`), `~/Imágenes → ~/Pictures`, `~/Música → ~/Music`, `~/Vídeos → ~/Videos`, `~/Plantillas → ~/Templates`, `~/Público → ~/Public`, updated `~/.config/user-dirs.{dirs,locale}` to `en_US`, and updated all 13 scripts/KDE configs (`plasma-org.kde.plasma.desktop-appletsrc`, `kscreenlockerrc`, `spectaclerc`, `user-places.xbel`, `centro_de_control.py`, `cambiar-fondo-{matrix,espacio}`, `convert-to-instagram`, `wow-addons-manager`, `respaldo-usb`, `matrix-core.sh`, `matrix-wall.py`, `matrix-shot.sh`) to English paths.

---

## 2026-11-21 — UKI Boot Lockout Recovery, Log Hygiene, ~32 GiB NVMe Cleanup, Offline Docs & Rescue USB v2.0

### 1. Root Cause Analysis & Recovery from UKI Boot Lockout (`02:59 – 04:41`)
- **Incident:** After running `~/Proyectos/matrix-os/bin/matrix-harden-and-tune`, the system failed to boot on both the main kernel (`7.3.5-arch1-1`) and the LTS fallback (`6.18.64-1-lts`).
- **Root Cause:** `matrix-harden-and-tune` wrote `/etc/dracut.conf.d/99-matrix-cmdline.conf` with `kernel_cmdline+=` omitting `rw root=UUID=...`. Under `dracut --uefi` (UKI layout), defining `kernel_cmdline` bypasses dracut's `hostonly_cmdline` fallback, causing `reinstall-kernels` to bake UKIs with no root filesystem parameter.
- **Recovery:** Booted from the Ventoy Live USB, chrooted into the NVMe root partition, restored `rw root=UUID=d472a732-533b-44d1-9026-b8cebd2a1fc8` inside `/etc/dracut.conf.d/99-matrix-cmdline.conf`, and rebuilt kernels.
- **Permanent Prevention:** Hardened `~/Proyectos/matrix-os/bin/matrix-harden-and-tune` to dynamically source `/etc/kernel/cmdline`, enforce a strict pre-flight check that aborts before touching `dracut` or running `reinstall-kernels` if `root=` is absent, stop auto-enabling `irqbalance`, and make `usbguard` opt-in (`ENABLE_USBGUARD=1`).

### 2. System-Wide Boot & Journal Error Elimination (`04:45 – 05:06`)
- **Idempotent TITAN Boot Shield (`~/.local/bin/blindaje-seguridad` -> `/usr/local/bin/titan-blindaje-root.sh`):**
  - Introduced `write_if_changed` so `blindaje-seguridad.service` no longer unconditionally reloads `firewalld`, `NetworkManager`, `systemd-resolved`, `docker`, or fires a global `udevadm trigger` on every boot — eliminating 75 boot-time `firewalld` `iptables` warnings, Docker firewall reload races, and ~5.5 s of startup overhead.
- **NVMe udev Rule Fix (`/etc/udev/rules.d/60-nvme-performance.rules`):**
  - Scoped the rule with `ENV{DEVTYPE}=="disk"` so `queue/scheduler`, `queue/read_ahead_kb`, and `queue/rq_affinity` target only the block device (`nvme0n1`) rather than partitions (`nvme0n1p1`/`p2`), cutting 24 `udev-worker` boot errors to `0`.
- **UKI Pipeline & Permissions Hygiene (`/etc/kernel/`):**
  - Normalized `/etc/kernel/install.conf` (`0644`, fixing non-root `kernel-install inspect`), masked duplicate non-UKI `kernel-install-for-dracut` hooks (`50-dracut-kifd.install`, `50-dracut-fallback.install`, `90-loaderentry-kifd.install`, `90-loaderentry-fallback.install`) via `/dev/null` symlinks in `/etc/kernel/install.d/`, verified embedded `.cmdline` PE sections in both `7.3.5-arch1-1` and `6.18.64-1-lts` UKIs with `objcopy`, and removed 140 MiB of redundant `/boot/initramfs-*.img` files.
- **Kernel Modules, Services & Shutdown Fixes:**
  - Added `sch_fq` alongside `tcp_bbr` in `/etc/modules-load.d/bbr.conf` and set `0644` permissions across `/etc/modules-load.d/*.conf` (fixes early-boot `systemd-sysctl` `net/core/default_qdisc` error).
  - Disabled `irqbalance.service` (eliminates thermal and NVMe managed-IRQ `Permission denied` errors on single-socket Ryzen desktop) and `usbguard.service` (eliminates 35 UEvent warnings and prevents silent USB/ESP32/phone lockout).
  - Uncommented `WIRELESS_REGDOM="AR"` in `/etc/conf.d/wireless-regdom` (fixes `cfg80211` udev exit code 1), fixed `root:root` ownership on `/usr/share/plymouth/themes/zion-matrix`, and added `~/.config/systemd/user/docker-desktop.service.d/timeout.conf` (`TimeoutStopSec=15s`) to prevent 90 s shutdown timeouts.
  - Packaged the full idempotent verification/repair routine into `~/.local/bin/reparar-linux`. Post-run status: `0` failed systemd units (system & user), `pacman -Dk` clean, `titan-doctor` 100% `[OK]`.

### 3. NVMe Disk Cleanup & Single-DE Consolidation (`05:07 – 05:45`, ~32 GiB Reclaimed)
- **Removed Secondary Compositor (Hyprland):** uninstalled `hyprland`, `vshyprland-manager`, `vswallpaper-effect`, `xdg-desktop-portal-hyprland`, `grim`, `slurp`, and 15 exclusive dependencies (`21` packages, `101.82 MiB`) plus `~/.config/hypr`, leaving KDE Plasma 6 (`plasma.desktop`) as the sole Wayland session.
- **Reclaimed ~32 GiB Safely (Zero Personal Data/Games Touched):**
  - User caches: `~/.cache/yay` (`10 GiB`), `~/.npm/{_cacache,_npx}` (`3.8 GiB`), closed Chromium/Brave caches (`2.9 GiB`), `~/.gradle/caches` (`1.4 GiB`), plus `debuginfod_client`, `pip`, and `pnpm` caches.
  - System caches (`~/.local/bin/limpiar-disco-root`): `/var/cache/pacman/pkg` (`6.68 GiB`, 1,774 packages via `paccache -rk0`), orphaned system Flatpak ostree objects (`741.7 MB`), journal vacuum (`50M`), and `fstrim -a`.
  - Obsolete Antigravity duplicates: removed `~/.local/lib/antigravity-updated` (`500 MB`), uninstalled legacy `antigravity-ide 2.7.0-1` + `/opt/antigravity-ide` + `~/.antigravity-ide` + `~/.config/Antigravity IDE` + `~/.gemini/antigravity-ide` (`~4.2 GiB`), and pruned its two stale MCP entries from `~/.gemini/config/mcp_config.json` (preserving `antigravity-appimage 2.31.2-1` and `data-agent-kit-plugin`).
  - **Result:** NVMe root (`/`) reduced from `293 GiB` (`67%`, `151 GiB` free) to **`261 GiB` (`59%`, `182 GiB` free)**.

### 4. Offline Linux Knowledge Base & Ventoy Master Rescue USB v2.0 (`05:16 – 06:06`)
- **Offline Reference Stack on Host:**
  - Installed `arch-wiki-lite` (`20261101-1`, `20.87 MiB`, full offline ArchWiki via `wiki-search`), refreshed `tldr` offline cache (`7,648` pages), and authored host-specific reference guides (`~/Documents/MANUAL_LINUX_ENDEAVOUROS_EXEQ.md`, `~/Desktop/LEEME_INICIAR_DESDE_BASH.txt`).
- **Ventoy Master Rescue USB (`/dev/sda1`) Upgraded:**
  - Added SHA-256-verified **`systemrescue-13.03-amd64.iso`** (`1.4 GB`: GParted, TestDisk, PhotoRec, `ddrescue`, `smartctl`, `fsarchiver`, `Memtest86+`, `arch-chroot`, Xfce) alongside `EndeavourOS_Titan-Nova-2026.08.15.iso`.
  - Upgraded `RESCATE_NOTEBOOK.sh` to v2.0 with automatic UKI/`dracut` `root=UUID=` validation and repair, `reinstall-kernels`, `objcopy` `.cmdline` verification, `sbctl` signing, forensic `journalctl --root=/mnt` inspection, chroot service disabling, and `/etc/kernel` + `/etc/dracut.conf.d` restore from USB backup.
  - Upgraded portable AI suite (`herramientas_live/ai_asistente/`): refreshed `Antigravity-portable.AppImage` (`v2.31.2`) and `claude-standalone` (`v2.1.330`), fixed root launcher `ASISTENTE_IA.sh`, added automatic `noexec` `/tmp` staging and `APPIMAGE_EXTRACT_AND_RUN=1` fallback for Live ISOs without `libfuse2`, added Linux-expert system prompt, repaired `CHAT_IA_BASH.sh` (fixed `<stdin>` `__file__` bug, updated to `gemini-2.5-flash` + offline `/diag`, `/manual`, `/rescate` commands), and added a 1-step Desktop IDE + Mini AI installer.
  - Added unified beginner-friendly entry point `EMPEZAR.sh` + `00_LEEME_INICIAR_DESDE_BASH.txt`, synced `Respaldo_PC_Exeq` (and updated `~/.local/bin/respaldo-usb` to always back up `/etc/kernel/` and `/etc/dracut.conf.d/`), regenerated `CHECKSUMS_SHA256.txt`, verified `100%` SHA-256 integrity, and safely unmounted `/dev/sda1`.

### 5. System & AI Desktop Upgrades (`14:15 – 14:28`)
- Full system sync and AUR desktop upgrades (`pacman.log`):
  - Core/libraries: `gwenhywfar` (`5.14.1-4 -> 5.14.4-1`), `gwenhywfar-gtk` (`5.14.1-4 -> 5.14.4-1`), `python-yaml` (`6.0.3-2 -> 6.0.4-1`).
  - AI desktop clients: `claude-desktop` (`5.11492.5-1 -> 6.28899.2-1`), `chatgpt-desktop` (`26.1111.71704-1 -> 26.1117.41037-1`).

### 6. Autonomous Multi-Agent Stack, Global Skills & Energy-Efficient Tier Protocol (`16:09 – 16:28`)
- **Dune Architecture & 3-Pillar Autonomous Loop (`ExePaginasweb`):**
  - Deployed physical guardrails (`scripts/audit-guardrails.mjs`), layer import-boundary enforcement (`scripts/audit-imports.mjs`), unified verification loop (`scripts/verify-loop.mjs`, `PASS 100/100` in `9.7 s`), and self-testing adversarial evals (`scripts/run-agent-evals.mjs`, `6/6` `100%`), enforced in `.husky/pre-commit` and `.github/workflows/verify.yml`.
- **Global Agent Skills & Permanent Startup Protocol (`~/.gemini/config/`):**
  - Integrated Top 10 AI Engineering & UI/UX/Design skill repositories (`addyosmani/agent-skills`, `vercel-labs/agent-skills`, `anthropics/skills`, `promptfoo/promptfoo`, `nextlevelbuilder/ui-ux-pro-max-skill`, `google-labs-code/stitch-skills`, `jakubkrehel/skills`, alongside `obra/superpowers`, `mattpocock/skills`, `google/skills`, and `supabase/agent-skills`).
  - Promoted **20 canonical non-duplicated skills** globally to `~/.gemini/config/skills/` and persisted the **Tier 1–4 Energy-Efficient Execution Protocol** (Progressive Disclosure, max 2–3 local swarm workers, Circuit Breaker at 3 repeated failures, idle daemon hygiene) in `~/.gemini/config/GEMINI.md`.

### 7. Linux Power Efficiency, Matrix OS v3.0 Visual Repair & Automated Anti-Loss Backups (`16:30 – 16:35`)
- **Power & Thermal Efficiency (`amd_pstate`):** switched daily desktop CPU profile from `performance` to `balanced` (`powerprofilesctl set balanced`) to cut idle electricity draw and heat on Ryzen 5 8500G while keeping on-demand boost and `modo-gamer` / `jugar-wow` performance switching intact.
- **Matrix OS v3.0 Graphics & Smoothness (`KDE Plasma 6 Wayland`):**
  - Repaired broken `~/.config/kwinrulesrc` symlink pointing to `~/Proyectos/matrix-os/packages/kwin/.config/kwinrulesrc` (`matrixctl doctor`: `0` critical errors).
  - Switched active desktop profile from `gamer` (shader/blur paused) back to **`cyber`** (Katakana GLSL shader + `better_blur_dx` + KWin 6 Matrix FX) and tuned `AnimationDurationFactor=0.75` for crisp 75 Hz window transitions.
- **Anti-Loss Protection (`matrix-auto-backup.timer`):**
  - Deployed `~/.local/bin/matrix-auto-backup` with a persistent daily user systemd timer (`~/.config/systemd/user/matrix-auto-backup.{service,timer}`, `Nice=19`, `IOSchedulingClass=idle`) creating `chmod 600` rolling 7-day snapshots in `~/.local/share/matrix-backups/` (`chmod 700`) of KDE/KWin configs, `~/.local/bin`, `~/.gemini/config/`, and Matrix OS profiles.
  - Verified `~/Downloads` is clean of exposed sensitive documents and `titan-doctor` is `100% [OK]`.


---

## 2026-11-25 — Dota 2 20-FPS Regression Forensic Investigation (`Mesa 26.3.1` `VK_AMD_anti_lag`), `<1s` RADV Shader Cache & Pure CachyOS Gaming Pipeline

### 1. Problem Statement & Symptoms (`22:54 – 23:15`)
- **Symptoms reported:**
  1. **Slow shader processing & confusing Steam UI:** Steam repeatedly stalled game launches with multi-gigabyte *"Processing Vulkan shaders..."* Fossilize queues (`3.4 GB` `fozpipelinesv6` under `~/.local/share/Steam/steamapps/shadercache/570`), while enabling *"Show all compatibility tools"* in Dota 2's Compatibility menu exposed ~20 legacy Proton/Runtime entries that risked breaking native execution or triggering VAC mismatches.
  2. **Severe framerate collapse (`120 FPS → 20–24 FPS`):** Dota 2 (`AppID 570`) had previously sustained **120+ FPS** on the **AMD Ryzen 5 8500G + Radeon 740M (`PHOENIX2`, 4 CUs RDNA3)**, but was now locked at **20–24 FPS** (`~41.6 ms` frametime) despite low CPU (`~19%`) and GPU (`~35–39%`) utilization.

### 2. Step-by-Step Forensic Investigation & Elimination of Secondary Bottlenecks (`22:55 – 23:44`)

1. **BIOS, UMA VRAM & DDR5 EXPO Hardware Audit (Verified 100% Healthy):**
   - Verified dedicated UMA frame buffer in sysfs (`/sys/class/drm/card1/device/mem_info_vram_total`): **`4096 MiB` (4 GB fixed in BIOS)** active, ~`2.5–3.7 GiB` utilized in-game with zero GTT thrashing.
   - Verified Dual-Channel memory configuration via `udev` SMBIOS DMI tables and `amdgpu` clocks: **`2 × 16 GiB DDR5 @ 6000 MT/s`** (`pp_dpm_mclk = 3000 MHz *`, `pp_dpm_fclk = 2000 MHz *` 1:1 Infinity Fabric).
   - Verified CPU frequency scaling & scheduler: `amd-pstate-epp` (`performance` governor/EPP boosting to **`5.08 GHz`**) running under CachyOS/SteamOS **`scx_lavd 1.1.4`** (`--performance` BPF `sched_ext` scheduler) + `ananicy-cpp` (`cachyos-ananicy-rules-git`, `nice -5` on `dota2`) + Kernel `7.3.6-arch1-1` native `/dev/ntsync`.

2. **Instant `<1s` Vulkan Shader Loading (Eliminating Steam's 3.4 GB Fossilize Replay):**
   - Inspected `~/.local/share/Steam/logs/shader_log.txt` and `config.vdf`: Steam had queued `"ProcessingQueue" "570;"` against a `3.4 GB` `fozpipelinesv6` archive, even though `Mesa 26.3.1-arch1.3` (`RADV` + `ACO` + `VK_EXT_graphics_pipeline_library` / GPL) already held **`645 MiB` of pre-compiled native `PHOENIX2` binary shaders** inside `mesa_shader_cache_sf`.
   - Extracted and preserved the `645 MiB` compiled RADV binary cache into a dedicated NVMe path (`~/.cache/dota2_radv_cache/mesa_shader_cache_sf`), set `"DisableShaderCache" "1"` in `~/.local/share/Steam/config/config.vdf`, purged the redundant `3.4 GB` `fozpipelinesv6` replay queue, configured `~/.local/share/Steam/steam_dev.cfg` (`@unShaderBackgroundProcessingThreads 12` + HTTP/2 multi-connection downloads), and set `CompatToolMapping` global default (`"0"`, priority `75`) to `proton-cachyos-11.0-20261015-slr-x86_64_v3` while leaving `AppID 570` unmapped so Dota 2 defaults cleanly to **`Steam Linux Runtime 3.0 (sniper)`**.

3. **Secondary System & Engine Bottlenecks Discovered and Fixed Along the Way:**
   - **Radeon 740M GPU DPM Clock Lock (`800 MHz → 2799 MHz`):** Removing `gamemoderun` (which conflicted with `scx_lavd` / `ananicy-cpp` and threw `ioprio` + `pkexec` errors inside `pressure-vessel`) left `/sys/class/drm/card1/device/power_dpm_force_performance_level` in `auto`, keeping `sclk` downclocked at **`800 MHz`** instead of **`2799 MHz`**. Wired passwordless Polkit `gpuclockctl 1 set high` on launch (and `set auto` on exit) directly into `~/.local/bin/titan-game-run`, unlocking the full **`2799 MHz`** iGPU clock without `gamemoded` or `LD_PRELOAD` injection.
   - **Audio Mixer Underrun Stall (`snd_mixahead "0.001" → "0.025"`):** Found `"snd_mixahead" "0.001"` (`1 ms` mix buffer — smaller than PipeWire's `5.33 ms` `256/48000` quantum) and `"engine_no_focus_sleep" "30"` (`1000 ms / ~41.6 ms = 24 FPS` during unfocused/Spectacle screenshot events) persisted in `machine_convars.vcfg`. Normalized to `"0.025"` and `"0"`.
   - **KWin 6 Wayland Direct Scanout Restoration (`ICC → sRGB`):** `HDMI-A-1` (`1920x1080@74.99Hz`) had `"colorProfileSource": "ICC"`, causing KWin 6 to disable zero-copy Direct Scanout and run a full-screen color shader pass on every frame. Switched live to `kscreen-doctor output.HDMI-A-1.colorProfileSource.sRGB`, restoring zero-copy Direct Scanout + `AllowTearing=true`.

4. **Differential A/B Test (`Proton-CachyOS 11` vs `Native Linux Vulkan`):**
   - Testing Windows `dota2.exe` under `proton-cachyos-11.0` initially crashed at startup (`dota2_2026_1125_232857_0_accessviolation.mdmp`). Inspecting the minidump binary strings revealed `No valid vcs file found for shader spritecard.vfx` because `-vulkan` was passed to Windows `dota2.exe` without the optional Windows Vulkan DLC.
   - Added automatic `-vulkan → -dx11` translation for Proton sessions in `titan-game-run` (`DXVK 2.x` + `NTSYNC` → `RADV`).
   - **Crucial A/B Result:** Under `Proton-CachyOS 11` (`-dx11` → `DXVK`), Dota 2 immediately surged to **`120+ real FPS`** (`342%` multi-thread Zen 4 CPU load, `2799 MHz` SCLK, `~8.3 ms` frametime) — **proving 100% that BIOS, UMA VRAM, DDR5-6000, KWin, and RADV hardware performance were intact** — However, Valve intentionally blocks **VAC online matchmaking** when running Dota 2 through Proton on Linux (*"Valve Anti-Cheat was unable to verify that your machine is secure"*).
   - Switching back to **Native Linux (`Steam Linux Runtime 3.0`)** immediately dropped performance back from **`120 FPS` to `20 FPS`** (`143%` CPU, `39%` GPU busy), isolating the regression strictly to the **Native Linux Vulkan codepath**.

### 3. The Smoking Gun: Instruction-Level Disassembly (`Mesa 26.3.1` `VK_AMD_anti_lag` Sleep Bug) (`23:45 – 23:51`)
- Attached `eu-stack` and `gdb` to the live `20 FPS` Native Linux `dota2` process (`PID 158307`) across consecutive samples. **100% of main-thread samples (`TID 158307`) were blocked on identical program counters (`SYSCALL=230` = `clock_nanosleep`):**
  ```text
  #0  0x7f52baaa08f2 -> libc.so.6 + 0xa08f2
  #1  0x7f5283173c37 -> libvulkan_radeon.so + 0x373c37  (os_time_nanosleep_until + 0x67 -> call clock_nanosleep)
  #2  0x7f52b10d5d7e -> librendersystemvulkan.so + 0x4d4d7e  (CAMDAntiLagVulkan -> vkAntiLagUpdateAMD)
  #3  0x7f52b117e805 -> librendersystemvulkan.so + 0x57d805
  ```
- **Root Cause:**
  1. `Mesa 26.3.1` (`libvulkan_radeon.so`) advertises the **`VK_AMD_anti_lag`** device extension (`vkAntiLagUpdateAMD`).
  2. Whenever Dota 2's **AMD Anti-Lag 2** setting (`r_low_latency "1"`) is enabled on Native Linux Vulkan, Source 2's `CAMDAntiLagVulkan` invokes `vkAntiLagUpdateAMD` on Thread 1 every frame, which enters `os_time_nanosleep_until` (`clock_nanosleep`) inside `libvulkan_radeon.so` and **sleeps the main engine thread for ~40 ms per frame — hard-clamping framerate to `20–24 FPS`** while leaving both CPU and GPU mostly idle!
  3. Under `Proton-CachyOS` (`-dx11` → `DXVK`), `amd_ags_x64.dll` is a no-op under Wine and never touches `VK_AMD_anti_lag`, explaining why Proton ran at `120 FPS` while Native Vulkan stalled at `20 FPS`.
  4. Furthermore, whenever `"setting.useadvanced"` in `video.txt` drifted to `"0"` (*Use basic settings*), Source 2's startup preset generator saw `VK_AMD_anti_lag` advertised by RADV and **silently re-enabled `"setting.r_low_latency" "1"` on its own**!

### 4. Permanent Fix & Physical Guardrails (`titan-game-run v3.1`) (`23:51 – 23:58`)
- **Live Verification:** Executing `r_low_latency 0` inside the running Native Linux Dota 2 console **immediately jumped framerate from `20 FPS` back to `120+ real FPS`** with **100% valid VAC online matchmaking**.
- **Permanent Multi-Layer Lock (`~/.local/bin/titan-game-run v3.1` & `autoexec.cfg`):**
  1. Updated `autoexec.cfg` with `r_low_latency "0"` and `engine_low_latency_sleep_after_client_tick "false"`.
  2. Hardened `~/.local/bin/titan-game-run` (`v3.1`) to sanitize `video.txt` before every launch (`"setting.useadvanced" "1"`, `"setting.r_low_latency" "0"`, `"setting.shaderquality" "0"`, `"setting.cl_globallight_shadow_mode" "0"`, `"setting.mat_viewportscale" "0.850000"`, `"setting.r_dota_fsr_upsample" "1"`), enforce **`chmod 444` (read-only)** on `video.txt` so Source 2 / Steam Cloud can never flip `useadvanced` back to `"0"` or auto-enable `r_low_latency`, and append `+r_low_latency 0 +engine_no_focus_sleep 0 +fps_max 150` directly to the engine startup arguments.
- **Final Verified State:**
  - **Shader Load Time:** **`< 1 second`** (zero Steam Fossilize replay wait; `645 MiB` pre-compiled RADV cache on NVMe).
  - **In-Game Performance:** **`120–150 real FPS`** (`~6.6–8.3 ms` frametime, `2799 MHz` Radeon 740M, `5.08 GHz` Zen 4, `sRGB` Direct Scanout + `AllowTearing=true`).
  - **Anti-Cheat & Compatibility:** **100% Native Linux (`Steam Linux Runtime 3.0`) with VAC verified**, while all Windows Steam titles default globally to `Proton-CachyOS 11` (`NTSYNC`).
