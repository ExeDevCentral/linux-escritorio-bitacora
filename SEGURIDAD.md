# System Security — TITAN Security Shield

Technical documentation of kernel hardening, network privacy, firewall rules, and access controls deployed on EndeavourOS.

- **Master script:** `~/.local/bin/blindaje-seguridad` (installed at `/usr/local/bin/titan-blindaje-root.sh`)
- **Boot persistence:** `/etc/systemd/system/blindaje-seguridad.service` (`enabled`, `Type=oneshot`, `After=network-online.target firewalld.service NetworkManager.service systemd-resolved.service`)

---

## 1. Kernel & Network Hardening (`/etc/sysctl.d/98-network-security.conf`)

- **Stealth Mode (ICMP):**
  - `net.ipv4.icmp_echo_ignore_all = 1`
  - `net.ipv4.icmp_echo_ignore_broadcasts = 1`
  - `net.ipv4.icmp_ignore_bogus_error_responses = 1`
- **IPv6 Privacy Extensions (RFC 4941):**
  - `net.ipv6.conf.all.use_tempaddr = 2` and `default.use_tempaddr = 2` (rotating temporary addresses).
- **Anti-Spoofing, ARP & Anti-MITM Protection:**
  - `rp_filter = 1`, `log_martians = 1` (Reverse Path Filtering and logging of spoofed/martian packets).
  - `arp_ignore = 1`, `arp_announce = 2` (LAN ARP hardening).
  - `accept_redirects = 0`, `secure_redirects = 0`, and `send_redirects = 0` on IPv4/IPv6.
  - `accept_source_route = 0` on IPv4/IPv6.
- **TCP Hardening & Anti-Fingerprinting:**
  - `net.ipv4.tcp_timestamps = 0` (prevents uptime leakage; later tuned to `1` on Nov 16 for `TCP BBR` RTT accuracy and PAWS protection using randomized per-connection offsets).
  - `net.ipv4.tcp_syncookies = 1`, `tcp_rfc1337 = 1`, `tcp_fin_timeout = 15`, `tcp_max_syn_backlog = 4096`, `somaxconn = 4096`.
- **Kernel Memory, BPF, Process & Filesystem Protections:**
  - `net.core.bpf_jit_harden = 2`, `kernel.unprivileged_bpf_disabled = 1`.
  - `kernel.kptr_restrict = 2`, `kernel.dmesg_restrict = 1`, `fs.suid_dumpable = 0`.
  - `kernel.yama.ptrace_scope = 1`, `kernel.randomize_va_space = 2`, `kernel.sysrq = 0`, `dev.tty.ldisc_autoload = 0`.
  - `fs.protected_hardlinks = 1`, `fs.protected_symlinks = 1`, `fs.protected_fifos = 2`, `fs.protected_regular = 2`.

---

## 2. Firewall (`firewalld`)

Verified status: `running`.

- **`public` zone (default, `enp5s0` interface):**
  - Target set to **`DROP`** (silently drops unsolicited inbound packets).
  - `ssh` and `dhcpv6-client` services permanently removed.
- **`nm-shared` zone (`wlan0` interface — Hotspot):**
  - Only `dhcp`, `dns`, and `masquerade` enabled to allow NAT traffic for the `Red-Exeq-WiFi` hotspot.
- **Docker zones:** `docker0` and bridge `br-adf4fab8589c` cleanly assigned to the `docker` zone (removed from `trusted` to prevent `ZONE_CONFLICT`).
- **Idempotent boot hook:** `blindaje-seguridad.service` only invokes `firewall-cmd --reload` when permanent zone settings actually drift, preventing boot-time races against `dockerd` iptables chains.

---

## 3. MAC Address Privacy (`/etc/NetworkManager/conf.d/00-mac-randomization.conf`)

- `wifi.scan-rand-mac-address=yes` (randomized MAC during Wi-Fi scans).
- `wifi.cloned-mac-address=stable` and `ethernet.cloned-mac-address=stable`.
- `Hotspot` profile (`RTL8188EUS` adapter) pinned to `permanent` MAC and `nm-shared` zone to prevent client disconnects.

---

## 4. Encrypted DNS & Local Broadcast Silencing

- **DNS-over-TLS (`/etc/systemd/resolved.conf.d/security.conf`):**
  - Primary: Cloudflare (`1.1.1.1`, `1.0.0.1`, `2606:4700:4700::1111#cloudflare-dns.com`).
  - Fallback: Quad9 (`9.9.9.9`, `149.112.112.112#dns.quad9.net`).
  - `Domains=~.` (enforces encrypted DNS globally over router DHCP), `DNSOverTLS=yes`, `DNSSEC=allow-downgrade`.
- **Local broadcast protocols disabled:**
  - `LLMNR=no` and `MulticastDNS=no` in `systemd-resolved`.
  - `avahi-daemon` stopped and disabled.

---

## 5. Docker Isolation, Coredumps & Logs

- **Docker (`/etc/docker/daemon.json`):** configured with `"ip": "127.0.0.1"` and `"live-restore": true` so published ports bind to `localhost` by default.
- **Coredumps (`/etc/systemd/coredump.conf.d/security.conf`):** `Storage=none` and `ProcessSizeMax=0` (prevents dumping sensitive process memory to disk).
- **Journald:** retention/size cap applied (`--vacuum-size=100M --vacuum-time=7d`).

---

## 6. Antivirus, Secure Boot & Directory Permissions

- **ClamAV:** installed on `2026-11-04` with `clamav-freshclam.service` enabled for signature updates.
- **AppArmor:** active (`apparmor.service` enabled; kernel LSM stack `capability,landlock,lockdown,yama,apparmor,bpf` loaded via UKI cmdline).
- **OOM Protection:** `earlyoom.service` active with D-Bus notifications.
- **Desktop Hardware Services Posture (`2026-11-21`):**
  - `irqbalance.service` disabled (unnecessary on single-socket AMD Ryzen 5 8500G; eliminates thermal and NVMe managed-IRQ `Permission denied` log spam).
  - `usbguard.service` disabled by default (package retained for opt-in use via `ENABLE_USBGUARD=1`, preventing silent lockout of hotplugged USB drives, phones, and ESP32/UART microcontrollers).
- **Secure Boot (`sbctl`):** installed on `2026-10-27` (UKIs automatically signed via `91-sbctl.install` when keys are enrolled).
- **Restrictive permissions (`700` / `drwx------`) verified on:**
  - `~/.seguridad/` (contains `credenciales.txt` with `600` permissions)
  - `~/.ssh/`
  - `~/.gnupg/`
  - `~/.password-store/`
- **Sensitive files hygiene (`2026-11-21`):** verified `~/Downloads/` is completely clean of exposed IDs (`dni_*`) and recovery keys (`Mozilla-Recovery-Key*` relocated to `~/.seguridad/documentos/`).
- **Automated Configuration Vault (`matrix-auto-backup.timer`, `2026-11-21`):** daily persistent `systemd --user` timer (`Nice=19`, `IOSchedulingClass=idle`) running `~/.local/bin/matrix-auto-backup` to store rolling 7-day `chmod 600` snapshots in `~/.local/share/matrix-backups/` (`chmod 700`) protecting KDE/KWin, Matrix OS, `~/.local/bin`, and `~/.gemini/config/`.
