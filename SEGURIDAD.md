# Seguridad del Sistema — TITAN Security Shield

Documentación técnica de las medidas de endurecimiento (hardening), privacidad de red, firewall y control de acceso aplicadas en EndeavourOS.

- **Script maestro:** `~/.local/bin/blindaje-seguridad` (instalado en `/usr/local/bin/titan-blindaje-root.sh`)
- **Persistencia al arranque:** `/etc/systemd/system/blindaje-seguridad.service` (`enabled`, `Type=oneshot`, `After=network-online.target firewalld.service NetworkManager.service systemd-resolved.service`)

---

## 1. Endurecimiento de Kernel y Red (`/etc/sysctl.d/98-network-security.conf`)

- **Modo Fantasma (ICMP):**
  - `net.ipv4.icmp_echo_ignore_all = 1`
  - `net.ipv4.icmp_echo_ignore_broadcasts = 1`
  - `net.ipv4.icmp_ignore_bogus_error_responses = 1`
- **Privacidad IPv6 (RFC 4941):**
  - `net.ipv6.conf.all.use_tempaddr = 2` y `default.use_tempaddr = 2` (direcciones temporales rotativas).
- **Protección Anti-Spoofing, ARP y Anti-MITM:**
  - `rp_filter = 1`, `log_martians = 1` (Reverse Path Filtering y registro de paquetes spoofeados).
  - `arp_ignore = 1`, `arp_announce = 2` (endurecimiento ARP en LAN).
  - `accept_redirects = 0`, `secure_redirects = 0` y `send_redirects = 0` en IPv4/IPv6.
  - `accept_source_route = 0` en IPv4/IPv6.
- **Protección TCP y Anti-Fingerprinting:**
  - `net.ipv4.tcp_timestamps = 0` (evita fuga de uptime).
  - `net.ipv4.tcp_syncookies = 1`, `tcp_rfc1337 = 1`, `tcp_fin_timeout = 15`, `tcp_max_syn_backlog = 4096`, `somaxconn = 4096`.
- **Memoria del Kernel, BPF, Procesos y Sistema de Archivos:**
  - `net.core.bpf_jit_harden = 2`, `kernel.unprivileged_bpf_disabled = 1`.
  - `kernel.kptr_restrict = 2`, `kernel.dmesg_restrict = 1`, `fs.suid_dumpable = 0`.
  - `kernel.yama.ptrace_scope = 1`, `kernel.randomize_va_space = 2`, `kernel.sysrq = 0`, `dev.tty.ldisc_autoload = 0`.
  - `fs.protected_hardlinks = 1`, `fs.protected_symlinks = 1`, `fs.protected_fifos = 2`, `fs.protected_regular = 2`.

---

## 2. Firewall (`firewalld`)

Estado verificado: `running`.

- **Zona `public` (por defecto, interfaz `enp5s0`):**
  - Target configurado en **`DROP`** (descarta paquetes entrantes sin responder).
  - Servicios `ssh` y `dhcpv6-client` removidos de forma permanente.
- **Zona `nm-shared` (interfaz `wlan0` — Hotspot):**
  - Habilitados únicamente `dhcp`, `dns` y `masquerade` para permitir el tráfico NAT del hotspot `Red-Exeq-WiFi`.
- **Zonas Docker:** `docker0` en `trusted` y bridge `br-adf4fab8589c` en `docker`.

---

## 3. Privacidad MAC (`/etc/NetworkManager/conf.d/00-mac-randomization.conf`)

- `wifi.scan-rand-mac-address=yes` (MAC aleatoria durante escaneos Wi-Fi).
- `wifi.cloned-mac-address=stable` y `ethernet.cloned-mac-address=stable`.
- Perfil `Hotspot` (adaptador `RTL8188EUS`) fijado con MAC `permanent` y zona `nm-shared` para no cortar la conexión del celular.

---

## 4. DNS Cifrado y Silenciamiento de Difusión Local

- **DNS-over-TLS (`/etc/systemd/resolved.conf.d/security.conf`):**
  - Primario: Cloudflare (`1.1.1.1`, `1.0.0.1`, `2606:4700:4700::1111#cloudflare-dns.com`).
  - Fallback: Quad9 (`9.9.9.9`, `149.112.112.112#dns.quad9.net`).
  - `Domains=~.` (fuerza DNS cifrado globalmente por encima del DHCP del router), `DNSOverTLS=yes`, `DNSSEC=allow-downgrade`.
- **Difusión local desactivada:**
  - `LLMNR=no` y `MulticastDNS=no` en `systemd-resolved`.
  - `avahi-daemon` detenido y deshabilitado.

---

## 5. Aislamiento de Docker, Coredumps y Logs

- **Docker (`/etc/docker/daemon.json`):** configurado con `"ip": "127.0.0.1"` y `"live-restore": true` para que los puertos publicados queden ligados solo a `localhost` por defecto.
- **Coredumps (`/etc/systemd/coredump.conf.d/security.conf`):** `Storage=none` y `ProcessSizeMax=0` (evita volcado de memoria sensible en disco).
- **Journald:** limpieza/límite aplicado (`--vacuum-size=100M --vacuum-time=7d`).

---

## 6. Antivirus, Secure Boot y Control de Directorios

- **ClamAV:** instalado el `2026-11-04` con servicio de actualización de firmas `clamav-freshclam.service` habilitado.
- **AppArmor:** habilitado automáticamente si `apparmor.service` está presente.
- **Secure Boot (`sbctl`):** instalado el `2026-10-27` (requiere ejecución con `root` para consultar/gestionar llaves UEFI).
- **Permisos restrictivos (`700` / `drwx------`) verificados en:**
  - `~/.seguridad/` (contiene `credenciales.txt` con permisos `600`)
  - `~/.ssh/`
  - `~/.gnupg/`
  - `~/.password-store/`
- **Punto de atención pendiente:** en `~/Descargas/` existen archivos sensibles sueltos (`dni_frente.jpg`, `dni_dorso.jpg`, `Mozilla-Recovery-Key_...txt`) que conviene mover a una carpeta privada o cifrada fuera de `Descargas`.

