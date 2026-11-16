# Inventario del Sistema y Plan de Ordenamiento

Mapa verificado de los scripts, enlaces simbólicos, configuraciones de sistema y archivos sueltos detectados al **12/11/2026**.

> **Regla de seguridad:** No mover ni borrar scripts en `~/.local/bin/` sin actualizar antes los symlinks y unidades `systemd` que apuntan a ellos (por ejemplo, `blindaje-seguridad.service` o `baliza-watchdog.service`).

---

## 1. Scripts propios activos (`~/.local/bin/`)

### Mantenimiento, Seguridad y Hardware ("TITAN")
- `~/.local/bin/actualizar-todo` (enlazado desde `~/actualizar-todo.sh` y `~/.local/bin/actualizar-sistema`)
- `~/.local/bin/blindaje-seguridad` (enlazado desde `~/aplicar-seguridad-root.sh` y `~/optimizacion/aplicar-optimizacion-sistema.sh`)
- `~/.local/bin/instalar-seguridad-y-limpieza` (enlazado desde `~/instalar-seguridad-y-limpieza.sh`)
- `~/.local/bin/limpiar-sistema`
- `~/.local/bin/optimizar-ryzen`
- `~/.local/bin/titan-doctor`
- `~/.local/bin/titan-hardware-root`
- `~/.local/bin/titan-oneclick`
- `~/.local/bin/respaldo-usb`
- `~/.local/bin/centro_de_control.py`

### Red y Hotspot WiFi
- `~/.local/bin/baliza` (enlazado como `airforge` y `hotspot`)
- `~/.local/bin/compartir_internet.py`

### Gaming, Escritorio y Utilidades
- `~/.local/bin/jugar-wow`, `wow`, `modo-gamer`, `wow-addons-manager`
- `~/.local/bin/cambiar-fondo-espacio` (usado por `~/Escritorio/Cambiar-Fondo-Espacial.desktop`)
- `~/.local/bin/convert-to-instagram`
- `~/.local/bin/entrar-a-bios`
- `~/.local/bin/agy`, `antigravity`, `gh`, `vc`, `vercel`

---

## 2. Configuraciones personalizadas en `/etc/`

- **Kernel / Sysctl (`/etc/sysctl.d/`):**
  - `50-cursor.conf` — soporte user namespaces para Cursor
  - `98-network-security.conf` — TITAN Security (ICMP/hardening de red)
  - `99-ip-forward.conf` — `net.ipv4.ip_forward = 1` (hotspot)
  - `99-ryzen-gaming.conf` — TITAN Performance (Ryzen 5 8500G + BBR + Wine/Proton)
- **Módulos (`/etc/modprobe.d/`):**
  - `amdgpu.conf` — `ppfeaturemask=0xffffffff`
  - `8188eu.conf` + `blacklist-rtl8xxxu.conf` — driver WiFi USB estable sin ahorro de energía
  - `zswap.conf` — `enabled=0` (en favor de ZRAM `zstd`)
- **Reglas udev (`/etc/udev/rules.d/`):**
  - `51-microcontrollers-mtk.rules` — ESP32 / CP210x / MediaTek
  - `60-nvme-performance.rules` — `scheduler=none`, `read_ahead_kb=256`
  - `99-logitech-c310.rules` — ajuste automático webcam Logitech C310
- **Red (`/etc/NetworkManager/conf.d/`):**
  - `00-mac-randomization.conf`
  - `dns.conf` — `dns=systemd-resolved`

---

## 3. Foco de desorden detectado (pendiente de limpieza gradual)

1. **Symlinks duplicados en `~/` (`$HOME`):**
   - `~/actualizar-todo.sh`, `~/aplicar-seguridad-root.sh`, `~/instalar-seguridad-y-limpieza.sh` y `~/optimizacion/aplicar-optimizacion-sistema.sh` apuntan a scripts que ya están en `~/.local/bin/` (en el `$PATH`).
2. **Carpeta `~/optimizacion/`:**
   - Contiene `backups/`, `obsoleto/`, `backup-addons-20261024-1334/` y `backup-blizzard-addons/` mezclados en la raíz del usuario.
3. **Carpeta `~/Descargas/` (68 elementos, ~3.4 GB):**
   - Instaladores ya aplicados o pesados: `docker-desktop-x86_64.pkg.tar.zst` (644 MB), `Antigravity.tar.gz` (175 MB) y `Antigravity (1).tar.gz` (175 MB).
   - Archivos duplicados: `MATEMATICA-Cursillo-Ingreso-Nivel-Superior-2027.pdf` y `(1).pdf`; `B2B_digital_strategy_outreach_guide_4c577bfa_1080p.mp4` y `(1).mp4`.
   - Archivos comprimidos ya extraídos (`addon-file-24159-6ac28f23cee06.zip`, `AddOns(1).rar`, `ExequielEchevarria.zip`).
   - Documentos personales/sensibles mezclados con descargas temporales (`dni_frente.jpg`, `dni_dorso.jpg`, `Mozilla-Recovery-Key_...txt`).
4. **Scripts sueltos en `~/Proyectos/`:**
   - Múltiples scripts de push/automatización en la raíz de `~/Proyectos/` (`auto_pusher_*.py`, `push_*.py`, `runner_30_pushes.py`, `GUIA_LEVANTAR_SERVIDORES.md`) fuera de una carpeta propia.
5. **Carpetas de trabajo en `~/` fuera de `~/Proyectos/`:**
   - `~/android_audit`, `~/lineageos_poco_c65`, `~/mtkclient`, `~/c-practica`.

