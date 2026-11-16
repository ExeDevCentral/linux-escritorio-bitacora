```c
/* ============================================================================
 *  LINUX_DESKTOP(7)           SYSTEM LOGBOOK & FIXES           LINUX_DESKTOP(7)
 *
 *  FILE:    README.md
 *  HOST:    EndeavourOS (Arch Linux) // Kernel 7.3.4-arch1-2 // x86_64
 *  ARCH:    AMD Ryzen 5 8500G + Radeon 740M (amdgpu / RADV) + 32GB DDR5
 *  AUTHOR:  Exequiel Echevarria <ExeDevCentral>
 *  LICENSE: MIT
 *
 *  DESCRIPTION:
 *    Technical logbook in classic UNIX/C style covering hardware compatibility,
 *    driver fixes, system hardening, and desktop Linux gaming tweaks.
 *    Documents what works out of the box, what broke, and how it was fixed.
 * ============================================================================ */
```

```text
+------------------------------------------------------------------------------+
|  COMPATIBILITY & FIXES MATRIX  [ VERIFIED: 2026-11-16 ]                      |
+--------------------+----------------------------------+----------------------+
| SUBSYSTEM          | HARDWARE / STACK                 | STATUS & FIX         |
+--------------------+----------------------------------+----------------------+
| [01] APU / GPU     | Ryzen 5 8500G / Radeon 740M      | [OK] ppfeaturemask   |
|                    | amdgpu + RADV + powerprofilesctl |      + RADV SAM      |
+--------------------+----------------------------------+----------------------+
| [02] USB WIFI      | Realtek 8188eu (wlan0 Hotspot)   | [FX] blacklist       |
|                    |                                  |      rtl8xxxu + WD   |
+--------------------+----------------------------------+----------------------+
| [03] NET / DNS     | enp5s0 (r8169) + resolved        | [OK] TCP BBR + fq    |
|                    |                                  |      DoT + -LLMNR    |
+--------------------+----------------------------------+----------------------+
| [04] GAMING        | PoE Standalone / WoW 3.3.5a      | [OK] Proton Exp /    |
|                    | Dota 2 / ananicy-cpp / MangoHud  |      Proton-CachyOS  |
+--------------------+----------------------------------+----------------------+
| [05] AUDIO         | PipeWire + WirePlumber           | [OK] System Default  |
+--------------------+----------------------------------+----------------------+
| [06] HARDENING     | TITAN Suite (sysctl + firewalld) | [OK] ICMP Drop + MAC |
+--------------------+----------------------------------+----------------------+
```

## /* DOCUMENTATION INDEX */

| File | Description |
| --- | --- |
| [BITACORA.md](BITACORA.md) | Chronological timeline (`Oct 21 – Nov 16, 2026`) of installations, regressions, and fixes. |
| [SEGURIDAD.md](SEGURIDAD.md) | 8-layer TITAN hardening (`sysctl`, `firewalld` DROP, DNS-over-TLS, MAC, Docker, ClamAV). |
| [INVENTARIO_Y_ORDEN.md](INVENTARIO_Y_ORDEN.md) | Map of `~/.local/bin/` scripts, user symlinks, and `/etc/` system overrides. |

## /* SYSTEM SPECIFICATION */

```c
struct system_state {
    const char *distro;       /* "EndeavourOS (Arch Linux, BUILD_ID=2026.08.15)" */
    const char *installed_at; /* "2026-10-21 17:56 -03" */
    const char *kernel;       /* "7.3.4-arch1-2" */
    const char *desktop;      /* "KDE Plasma 6 / Wayland (plasmalogin.service)" */
    const char *cpu_gpu;      /* "AMD Ryzen 5 8500G / Radeon 740M (amdgpu)" */
    const char *memory;       /* "27 GiB DDR5 + 13.5 GiB ZRAM (zstd, swappiness=100)" */
    const char *storage;      /* "/dev/nvme0n1p2 - 467 GiB (278 GiB used, 63%)" */
};
```

## /* ACTIVE DAEMONS & SERVICES */

```text
[SYSTEMD // SYSTEM]
  * blindaje-seguridad.service   * firewalld.service        * systemd-resolved.service
  * NetworkManager.service       * ananicy-cpp.service      * lactd.service
  * ratbagd.service              * clamav-freshclam.service * docker.service
  * power-profiles-daemon        * fstrim.timer             * paccache.timer

[SYSTEMD // USER]
  * baliza-watchdog.service      * pipewire.socket          * wireplumber.service
  * pm2.service                  * xdg-user-dirs.service
```
