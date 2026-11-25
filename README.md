```c
/* ============================================================================
 *  LINUX_DESKTOP(7)           SYSTEM LOGBOOK & FIXES           LINUX_DESKTOP(7)
 *
 *  FILE:    README.md
 *  HOST:    EndeavourOS (Arch Linux) // Kernel 7.3.5-arch1-1 (+ LTS 6.18.64) // x86_64
 *  ARCH:    AMD Ryzen 5 8500G + Radeon 740M (amdgpu / RADV) + 32GB DDR5
 *  AUTHOR:  Exequiel Echevarria <ExeDevCentral>
 *  LICENSE: MIT
 *
 *  DESCRIPTION:
 *    Technical logbook in classic UNIX/C style covering hardware compatibility,
 *    driver fixes, UKI boot engineering, system hardening, and desktop Linux tweaks.
 *    Documents what works out of the box, what broke, and how it was fixed.
 * ============================================================================ */
```

```text
+------------------------------------------------------------------------------+
|  COMPATIBILITY & FIXES MATRIX  [ VERIFIED: 2026-11-21 ]                      |
+--------------------+----------------------------------+----------------------+
| SUBSYSTEM          | HARDWARE / STACK                 | STATUS & FIX         |
+--------------------+----------------------------------+----------------------+
| [01] APU / GPU     | Ryzen 5 8500G / Radeon 740M      | [OK] ppfeaturemask   |
|                    | amdgpu + RADV + powerprofilesctl |      + RADV SAM      |
+--------------------+----------------------------------+----------------------+
| [02] USB WIFI      | Realtek 8188eu (wlan0 Hotspot)   | [FX] blacklist       |
|                    | + WIRELESS_REGDOM="AR"           |      rtl8xxxu + WD   |
+--------------------+----------------------------------+----------------------+
| [03] NET / DNS     | enp5s0 (r8169) + resolved        | [OK] TCP BBR + sch_fq|
|                    |                                  |      DoT + -LLMNR    |
+--------------------+----------------------------------+----------------------+
| [04] GAMING        | PoE Standalone / WoW 3.3.5a      | [OK] Proton Exp /    |
|                    | Dota 2 / ananicy-cpp / MangoHud  |      Proton-CachyOS  |
+--------------------+----------------------------------+----------------------+
| [05] AUDIO         | PipeWire + WirePlumber + ALC897  | [OK] S32LE + PS=0    |
+--------------------+----------------------------------+----------------------+
| [06] HARDENING     | TITAN Suite + AppArmor + earlyoom| [OK] ICMP Drop + MAC |
+--------------------+----------------------------------+----------------------+
| [07] BOOT / UKI    | systemd-boot + dracut UKI (x2)   | [FX] root=UUID guard |
|                    | 7.3.5-arch1-1 & 6.18.64-1-lts    |      + masked kifd   |
+--------------------+----------------------------------+----------------------+
| [08] RESCUE USB    | Ventoy Dual-ISO + Portable AI    | [OK] SystemRescue    |
|                    | EMPEZAR.sh + RESCATE_NOTEBOOK v2 |      13.03 + SHA-256 |
+--------------------+----------------------------------+----------------------+
```

## /* DOCUMENTATION INDEX */

| File | Description |
| --- | --- |
| [BITACORA.md](BITACORA.md) | Chronological timeline (`Oct 21 – Nov 21, 2026`) of installations, regressions, and fixes. |
| [SEGURIDAD.md](SEGURIDAD.md) | 8-layer TITAN hardening (`sysctl`, `firewalld` DROP, DNS-over-TLS, MAC, Docker, AppArmor, ClamAV). |
| [INVENTARIO_Y_ORDEN.md](INVENTARIO_Y_ORDEN.md) | Map of `~/.local/bin/` scripts, user symlinks, offline manuals, and `/etc/` system overrides. |

## /* SYSTEM SPECIFICATION */

```c
struct system_state {
    const char *distro;       /* "EndeavourOS (Arch Linux, BUILD_ID=2026.08.15)" */
    const char *installed_at; /* "2026-10-21 17:56 -03" */
    const char *kernel;       /* "7.3.5-arch1-1 (+ linux-lts 6.18.64-1 fallback UKI)" */
    const char *desktop;      /* "KDE Plasma 6 / Wayland only (plasmalogin.service)" */
    const char *cpu_gpu;      /* "AMD Ryzen 5 8500G / Radeon 740M (amdgpu)" */
    const char *memory;       /* "27 GiB DDR5 + 13.5 GiB ZRAM (zstd, swappiness=100)" */
    const char *storage;      /* "/dev/nvme0n1p2 - 467 GiB (261 GiB used, 59%)" */
};
```

## /* ACTIVE DAEMONS & SERVICES */

```text
[SYSTEMD // SYSTEM]
  * blindaje-seguridad.service   * firewalld.service        * systemd-resolved.service
  * NetworkManager.service       * ananicy-cpp.service      * lactd.service
  * apparmor.service             * earlyoom.service         * clamav-freshclam.service
  * docker.service               * power-profiles-daemon    * fstrim.timer / paccache.timer

[SYSTEMD // USER]
  * baliza-watchdog.service      * pipewire.socket          * wireplumber.service
  * pm2.service                  * xdg-user-dirs.service    * docker-desktop (15s stop cap)
```

_Última revisión: 2026-11-25 20:05_
