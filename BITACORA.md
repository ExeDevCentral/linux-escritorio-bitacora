# Bitácora de Linux — Línea de Tiempo (Octubre – Noviembre 2026)

Registro cronológico reconstruido a partir de `/var/log/pacman.log`, configuraciones en `/etc/`, scripts en `~/.local/bin/` y sesiones de trabajo registradas.

---

## Octubre 2026

### 21 de octubre — Instalación base de EndeavourOS
- **17:56 – 18:03:** Instalación limpia de **EndeavourOS** (`BUILD_ID=2026.08.15`, `+1320` paquetes base, `~50` actualizados).
- Entorno inicial: KDE Plasma, `plasmalogin.service`, `firewalld.service`, `NetworkManager`.
- **18:42 – 23:33:** Instalación de herramientas de trabajo y entorno inicial:
  - `~/Aplicaciones/Antigravity-x64` y `~/.antigravity-ide`.
  - Scripts iniciales en `~/.local/bin/`: `entrar-a-bios`, enlaces `vc`/`vercel`.

### 22 de octubre — BIOS, +4 GB de RAM, GPU AMD y Gaming (WoW)
- **02:05 – 02:08:** Ajustes en BIOS y ampliación de RAM (+4 GB). Configuración de `/etc/modprobe.d/amdgpu.conf` (`options amdgpu ppfeaturemask=0xffffffff`) para habilitar control completo de energía/frecuencias en Radeon 740M.
- Optimización de **World of Warcraft (ChromieCraft 3.3.5a)** en `~/Juegos/ChromieCraft_3.3.5a`:
  - Creación de `~/.local/bin/wow-addons-manager` y `~/.local/bin/convert-to-instagram`.
  - Instalación de `discord` (`pacman.log`: +1 paquete).

### 23 de octubre — Desarrollo Web, Node y Control de GPU
- Instalación de **13 paquetes** (`pacman.log`): `nodejs`, `npm`, `node-gyp`, `lact` (control de GPU AMD), `opencode-beta`, `gst-plugins-good`, `qt6-multimedia-gstreamer`.
- Activación de `systemd-resolved` (`dbus-org.freedesktop.resolve1.service`) y creación del symlink `~/.local/bin/actualizar-sistema`.

### 24 de octubre — Scripts de Gaming y Backups de Addons
- Creación de `~/.local/bin/modo-gamer` y `~/.local/bin/wow`.
- Respaldos de addons en `~/optimizacion/backup-addons-20261024-1334` y `~/optimizacion/backup-blizzard-addons`.

### 25 de octubre — Red WiFi USB, Audio, Cámara, ZRAM y Docker
Día clave de infraestructura (`+38` paquetes instalados en `pacman.log`):
- **WiFi USB (`8188eu`):**
  - `/etc/modprobe.d/blacklist-rtl8xxxu.conf` (bloqueo del driver genérico conflictivo `rtl8xxxu`).
  - `/etc/modprobe.d/8188eu.conf` (`rtw_power_mgnt=0 rtw_ips_mode=0 rtw_enusbss=0` para evitar suspensiones/cortes del adaptador USB).
  - `/etc/sysctl.d/99-ip-forward.conf` (`net.ipv4.ip_forward = 1` para compartir internet/hotspot).
  - `/etc/NetworkManager/conf.d/dns.conf` (`dns=systemd-resolved`).
  - Creación de enlaces `airforge` y `hotspot` hacia `baliza`.
- **Hardware y Rendimiento:**
  - `/etc/udev/rules.d/60-nvme-performance.rules` (`scheduler="none"`, `read_ahead_kb="256"` para NVMe).
  - `/etc/udev/rules.d/99-logitech-c310.rules` + `guvcview-qt` para webcam Logitech C310.
  - `/etc/modprobe.d/zswap.conf` (`enabled=0`) + `zram-generator` para swap comprimida en RAM (`zstd`).
  - `piper` / `ratbagd.service` para configuración de mouse.
- **Audio y Apps:**
  - `easyeffects`, `lsp-plugins-lv2`, `calf`, `webrtc-audio-processing`.
  - `whatsie-git` (cliente WhatsApp Desktop), `android-tools`, `android-udev`, `docker`, `docker-compose`.

### 26 – 27 de octubre — Hyprland (pruebas), Depuración y Herramientas CLI
- **26 Oct:** Instalación de `valgrind`, `debuginfod`, `opencode-desktop-bin`, `electron44`; binario `agy` en `~/.local/bin/`.
- **27 Oct:** Instalación de `sbctl`, `hyprland`, `aquamarine`, `vswallpaper-effect`, `plasma6-wallpapers-smart-video-wallpaper-reborn` (`+53` paquetes). Wrapper `antigravity` en `~/.local/bin/`.

### 28 – 30 de octubre — Energía en KDE y Estabilización del Hotspot WiFi
- **28 Oct:** Ajuste de `~/.config/powerdevilrc` para retrasar suspensión y apagado de pantalla a **120 minutos (7200 s)** y evitar cortes del hotspot WiFi.
- **29 – 30 Oct:** Diagnóstico y ajuste de `Red-Exeq-WiFi` en `wlan0` (canal 6, 2.4 GHz) para compartir internet al celular (POCO C65).

### 31 de octubre — Diagnóstico inicial de Audio en WoW
- Corrección en `~/Juegos/ChromieCraft_3.3.5a/WTF/Config.wtf` (`Sound_EnableHardware "0"` por incompatibilidad de FMOD) y `~91` paquetes actualizados.

---

## Noviembre 2026

### 1 de noviembre — Limpieza de Kernel/GRUB, Node conflictivo y WiFi USB
- `+58` paquetes instalados (entre ellos `dkms`, `qemu-*`, `pass`, `tree`), `-7` removidos (incluyendo `linux-lts`).
- **Corrección crítica de entorno:** Detección y limpieza de un binario antiguo de `node` (v20.18 en `~/.local/bin/node`, movido a `old_node_backup`) que tapaba al `/usr/bin/node` oficial del sistema.
- **WiFi USB y Arranque:** Recuperación del adaptador USB WiFi tras desconexión en caliente y limpieza de entradas viejas en el menú de arranque (dejando kernel principal + respaldo).

### 2 de noviembre — Solución definitiva de Audio en WoW
- Actualización de `~/.local/bin/jugar-wow`: fijación de salida en **System Default** y ajuste de buffer de latencia para evitar desvíos a salidas HDMI/DisplayPort secundarias.

### 3 de noviembre — Watchdog de Hotspot WiFi y Herramientas Android/MTK
- Actualización de `~/.local/bin/baliza` y `~/.local/bin/compartir_internet.py`.
- Activación de `baliza-watchdog.service` (servicio de usuario en segundo plano que protege el enlace del hotspot).
- Trabajo con herramientas Android/MediaTek (`~/android_audit`, `~/lineageos_poco_c65`, `~/mtkclient`).

### 4 de noviembre — Escritorio Espacial, ClamAV y Limpieza
- Creación de `~/.local/bin/cambiar-fondo-espacio` y acceso directo en `~/Escritorio/Cambiar-Fondo-Espacial.desktop`.
- Instalación de `clamav`, `bleachbit` y activación de `/etc/systemd/system/paccache.timer` y `paccache.service` para limpieza automática de caché de `pacman`.

### 5 – 8 de noviembre — Mantenimiento de sistema y trabajo en proyectos
- **5 Nov:** `~37` paquetes actualizados.
- **7 Nov:** `~10` paquetes actualizados; creación de proyectos en `~/Proyectos/`.

### 9 de noviembre — Suite "TITAN" de Seguridad, Optimización y Control
Gran jornada de estandarización y hardening del sistema (`02:47 – 03:03`):
- Creación/actualización de la suite en `~/.local/bin/`:
  - `actualizar-todo` (con symlink `~/actualizar-todo.sh`)
  - `blindaje-seguridad` (con symlinks `~/aplicar-seguridad-root.sh` y `~/optimizacion/aplicar-optimizacion-sistema.sh`) — ver detalle completo de las 8 capas en [SEGURIDAD.md](SEGURIDAD.md)
  - `instalar-seguridad-y-limpieza` (con symlink `~/instalar-seguridad-y-limpieza.sh`)
  - `limpiar-sistema`, `optimizar-ryzen`, `respaldo-usb`, `titan-doctor`, `titan-hardware-root`, `titan-oneclick`, `centro_de_control.py`
- Symlinks de carpetas en inglés → español: `~/Documents -> ~/Documentos`, `~/Downloads -> ~/Descargas`, `~/Videos -> ~/Vídeos`.
- Verificación de microcódigo AMD y chipset A620M H + Ryzen 5 8500G.

### 10 – 11 de noviembre — Actualización masiva, ChatGPT Desktop y Git
- **11 Nov:** `~150` paquetes actualizados (`openssl-3`, etc.).
- Diagnóstico de `chatgpt-desktop` / `~/.config/Codex` (bloqueos huérfanos `Singleton*` y errores de gateway).
- Actualización de credenciales Git en `~/.gitconfig` y `~/.git-credentials`.

### 12 de noviembre — Cursor, Blindaje persistente y Bitácora central
- **14:02 – 18:19:** Steam, `pm2` y actualizaciones (`~31` paquetes).
- **22:35 – 22:37:** Instalación de `cursor-bin` (`electron43`) y aplicación/actualización de archivos de hardening en `/etc/`:
  - `/etc/sysctl.d/50-cursor.conf`
  - `/etc/sysctl.d/98-network-security.conf` (TITAN Security: modo fantasma ICMP y hardening de red)
  - `/etc/sysctl.d/99-ryzen-gaming.conf` (TITAN Performance: Ryzen 5 8500G + BBR + Wine/Proton)
  - `/etc/udev/rules.d/51-microcontrollers-mtk.rules` (ESP32, CP210x, MediaTek)
  - `/etc/NetworkManager/conf.d/00-mac-randomization.conf`
  - `/etc/systemd/system/blindaje-seguridad.service`
- **23:04 – 23:12:** Creación de este workspace central `/home/exeq/Proyectos/linux-escritorio-correcciones-programas` y actualización integral de la suite TITAN:
  - **Seguridad (`blindaje-seguridad` / `titan-doctor`):** agregado `yama.ptrace_scope=1`, `randomize_va_space=2`, `sysrq=0`, `dev.tty.ldisc_autoload=0` y chequeos de `~/.seguridad` y `.pacnew`.
  - **Rendimiento (`blindaje-seguridad` / `titan-hardware-root`):** `vm.swappiness=100` para ZRAM `zstd` y eliminación de `drop_caches` para no destruir la page cache útil.
  - **Actualizaciones (`actualizar-todo`):** eliminado el `--ignore` de `steam`/`wine`/`gamemode` (evita *partial upgrades* en Arch) y agregado detector de `.pacnew` en `/etc/`.
  - **TITAN Panel (`centro_de_control.py`):** telemetría de subprocesos (`systemctl`, `nmcli`, `docker`) movida a `TelemetryWorker(QThread)` para no bloquear la UI, tráfico de red físico real (sin `lo`/`docker0`), temperatura GPU `amdgpu`, `run_repair` asíncrono y botón `🔍 Ver Updates`.
- **23:27:** Corrección de accesos directos y wrapper de **Antigravity**: había dos paquetes instalados (`antigravity-ide 2.6.0-1` de Oct 28 vs `antigravity-appimage 2.31.2-1` de hoy 22:36) más un wrapper viejo en `~/.local/bin/antigravity`; se redirigieron `~/.local/bin/antigravity` y los `.desktop` locales a `/usr/bin/antigravity`.
- **23:29 – 23:50:** Corrección integral de **TITAN Control Center**, cuidado de hardware, optimización de juegos y ordenamiento de rutas personales:
  - **Fix PolKit en bucle (`centro_de_control.py` y `titan-doctor`):** reemplazada la llamada a `firewall-cmd --list-all --zone=public` (disparaba `org.fedoraproject.FirewallD1.config.info` cada 2 s) por lectura directa de `/etc/firewalld/zones/public.xml`.
  - **Cuidado de hardware y estado (`titan-oneclick`, `titan-hardware-root`, `blindaje-seguridad`, `centro_de_control.py`):** perfil `balanced` (`balance_performance`) para uso diario (reservando `performance` puro para `modo-gamer`), `vm.compaction_proactiveness=0`, `transparent_hugepage=madvise` (`/etc/tmpfiles.d/titan-thp.conf`), botón **Aplicar Blindaje Root** con salida visible en `konsole` y fix de ruta `is_gamer_mode_active()` (`/run/user/1000/modo-gamer`).
  - **WoW 3.3.5a (`Warlogz` + `Config.wtf` + `jugar-wow`):** restaurado `Warlogz/macros-cache.txt` desde `macros-cache.old` con `SET synchronizeMacros "0"`, eliminada macro general `INSTA` de `CANALLA01/macros-cache.txt`, optimización 25-man (`processAffinityMask="4095"`, `M2Faster="3"`, `extShadowQuality="0"`, `gxTextureCacheSize="1024"`, `ffxGlow="0"`, `maxFPS="120"`, `maxFPSBk="30"`, `Sound_EnableSoftwareHRTF="0"`) y restauración automática de `powerprofilesctl` al salir de `jugar-wow`.
  - **Dota 2 (`autoexec.cfg` y `video.txt`):** `fps_max="120"`, `fps_max_ui="60"`, desactivación de `r_depth_of_field`, `r_dota_bloom_compute_shader`, `r_dota_allow_wind_on_trees`, `r_dota_allow_parallax_mapping`, `r_dashboard_render_quality` y `dsp_enhance_stereo`.
  - **Limpieza y ordenamiento de rutas:** clasificación de `~/Descargas` en `~/Documentos/Descargas_Docs`, `~/Vídeos/Descargas_Videos`, `~/Imágenes/WhatsApp`, `~/Música/WhatsApp_Audios`, organización de 98 capturas en `~/Imágenes/Screenshots/{2026-10,2026-11}`, resguardo de `Mozilla-Recovery-Key` en `~/.seguridad/documentos/` (`chmod 700/600`) y eliminación de `~3.17 GB` de basura/duplicados.

### 14 de noviembre — TITAN Panel v2.2 (Linux Hacker / Matrix 60-30-10) & Stack Gaming CachyOS-style
- **TITAN Panel (`~/.local/bin/centro_de_control.py`):**
  - Rediseño completo (`TitanControlCenter`, `LogViewerDialog`, `DoctorDialog`) a estética **Terminal CRT / Linux Hacker retro** con regla cromática **60 / 30 / 10** (`#040604` base oscura, `#163616`/`#7bc47b` estructura verde terminal, `#ffb000` ámbar + `#00ff41` neón en acentos).
  - Agregado `MatrixHeaderFrame` (lluvia Matrix sutil en el fondo de la cabecera a ~12 FPS) + cursor CRT parpadeante `TITAN://CORE_v2.2 █`.
  - Agregado módulo **`:: GAMING_STACK [CACHYOS-STYLE / RADEON-740M]`** con telemetría en vivo de `ananicy-cpp`, `mangohud`, `gamescope` y acceso directo a `ProtonUp-Qt` y `Steam`.
- **Stack gaming instalado y verificado (sin alterar repositorios de EndeavourOS):**
  - Paquetes instalados: `ananicy-cpp 1.2.0-1`, `cachyos-ananicy-rules-git 20260908.r2762.g03ef03fb-1`, `mangohud 0.8.4-1`, `lib32-mangohud 0.8.4-1`, `gamescope 3.17.0-1`, `protonup-qt-bin 2.15.1-1`.
  - Servicio `ananicy-cpp.service` habilitado y activo (`enabled`/`active`): detectó topología híbrida del Ryzen 5 8500G (`12 CPUs`, `big.LITTLE=yes`, núcleos P/E) y cargó `15812` reglas de CachyOS.
  - **`Proton-CachyOS` instalado en Steam:** `proton-cachyos-11.0-20261015-slr-x86_64_v3` desplegado en `~/.steam/root/compatibilitytools.d/`.
  - **`MangoHud`:** verificado `~/.config/MangoHud/MangoHud.conf` activo (toggle `Shift_R+F12`, `top-right`, `fps`, `frametime`, `cpu/gpu temp`, `ram`, `vram`, `gamemode`).
- **Optimización WoW 3.3.5a (`jugar-wow` + `dxvk.conf` + afinidad Zen 4 P-cores):**
  - Fijado `Wow.exe` en vivo a los 4 hilos Zen 4 Performance (`taskset -pc 0,4,6,10`).
  - Actualizado `~/.local/bin/jugar-wow` para aplicar `SET processAffinityMask "1105"` en `Config.wtf` antes de cada inicio y exportar `MANGOHUD=1`.
  - Ajustado `d3d9.samplerAnisotropy = 8` en `/home/exeq/Juegos/ChromieCraft_3.3.5a/dxvk.conf` para aliviar ancho de banda DDR5 de la Radeon 740M.
- **Nuevo Fondo Espacial Animado ("Anomalía Cuántica & Monolito Alienígena"):**
  - Generada base 16:9 en `~/Imágenes/cosmic_anomaly_base.jpg` y renderizado loop orbital 1080p en `~/Vídeos/wallpapers/espacio/cosmic_anomaly_loop.mp4`.
  - Integrado como opción `5` en `~/.local/bin/cambiar-fondo-espacio` y activado en KDE Plasma 6 (`smart.video.wallpaper.reborn`).

### 16 de noviembre — Red DNS-over-TLS + BBR Timestamps & Stack Path of Exile
- **Optimización de red (`enp5s0` + `sysctl`):**
  - Activado **DNS-over-TLS estricto** (`connection.dns-over-tls 2`) y desactivado **LLMNR** (`connection.llmnr 0`) en `Conexión cableada 1` (`systemd-resolved` con `1.1.1.1`).
  - Habilitado `net.ipv4.tcp_timestamps = 1` en `/etc/sysctl.d/98-network-security.conf` y `~/.local/bin/blindaje-seguridad` para precisión de RTT en `TCP BBR` y protección PAWS (con offset aleatorizado por conexión en kernels modernos).
- **Path of Exile (Standalone) + Overlays + Rendimiento Radeon 740M:**
  - **Diagnóstico y corrección de ruta:** `PathOfExile.exe` había descargado `Content.ggpk` (70 GB) en `~/` por falta de `cd "$(dirname "$EXE")"` en el lanzador; reubicado al prefix `Standalone/pfx/.../Path of Exile/` y corregido el directorio de trabajo.
  - **Separación clara de lanzadores (`~/.local/bin/` y `.desktop`):** `poe-jugar` (juego instalado + alias `poe-standalone`), `poe-instalar` (instalador/reparador) y `poe-overlays` / `poe-tools` (`Awakened-PoE-Trade`, `ExileCompass`, `Exilence-CE` sin procesos duplicados).
  - **Automatización de rendimiento en `poe-jugar`:** `powerprofilesctl performance` con restauración a `balanced` al salir, variables `AMD_VULKAN_ICD=RADV`, `RADV_PERFTEST=sam`, `MESA_SHADER_CACHE_MAX_SIZE=12G`, `WINEFSYNC=1`, `WINEESYNC=1`, `gamemoderun`, `MANGOHUD=1`, `--nologo` y auto-ajuste de `production_Config.ini` (`Vulkan`, `texture_quality=Medium`, `shadow_type=Low`, `vsync_mode=Off`, `use_dynamic_particle_culling=true`, `framerate_limit=120`).



