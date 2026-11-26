```c
/* ============================================================================
 *  LINUX_DESKTOP(7)           SYSTEM LOGBOOK & FIXES           LINUX_DESKTOP(7)
 *
 *  FILE:    README.md
 *  HOST:    EndeavourOS (Arch Linux) // Kernel 7.3.6-arch1-1 (+ LTS 6.18.64) // x86_64
 *  ARCH:    AMD Ryzen 5 8500G (5.08 GHz) + Radeon 740M (2799 MHz, 4GB UMA) + 32GB DDR5-6000
 *  SCHED:   sched_ext scx_lavd 1.1.4 (--performance) + ananicy-cpp + /dev/ntsync
 *  AUTHOR:  Exequiel Echevarria <ExeDevCentral>
 *  LICENSE: MIT
 *
 *  DESCRIPTION:
 *    Engineering logbook, forensic root-cause analyses, hardware compatibility
 *    matrix, UKI boot architecture, 8-layer security hardening, and CachyOS-grade
 *    low-latency gaming pipeline on EndeavourOS (KDE Plasma 6 Wayland).
 * ============================================================================ */
```

```text
+------------------------------------------------------------------------------+
|  COMPATIBILITY & FIXES MATRIX  [ VERIFIED: 2026-11-25 ]                      |
+--------------------+----------------------------------+----------------------+
| SUBSYSTEM          | HARDWARE / STACK                 | STATUS & FIX         |
+--------------------+----------------------------------+----------------------+
| [01] APU / GPU     | Ryzen 5 8500G / Radeon 740M      | [OK] 2799 MHz DPM +  |
|                    | Mesa 26.3.1 RADV + 4GB UMA       |      sRGB Scanout    |
+--------------------+----------------------------------+----------------------+
| [02] MEMORY        | 32 GiB Dual-Channel DDR5 EXPO    | [OK] 6000 MT/s       |
|                    | MCLK 3000 MHz / FCLK 2000 MHz    |      + 13.5G ZRAM    |
+--------------------+----------------------------------+----------------------+
| [03] GAMING        | Dota 2 Native (VAC) / PoE / WoW  | [FX] r_low_latency=0 |
|                    | scx_lavd + NTSYNC + NVMe Shaders |      (<1s, 120+ FPS) |
+--------------------+----------------------------------+----------------------+
| [04] USB WIFI      | Realtek 8188eu (wlan0 Hotspot)   | [FX] blacklist       |
|                    | + WIRELESS_REGDOM="AR"           |      rtl8xxxu + WD   |
+--------------------+----------------------------------+----------------------+
| [05] NET / DNS     | enp5s0 (r8169) + resolved        | [OK] TCP BBR + sch_fq|
|                    |                                  |      DoT + -LLMNR    |
+--------------------+----------------------------------+----------------------+
| [06] AUDIO         | PipeWire + WirePlumber + ALC897  | [OK] S32LE + PS=0    |
+--------------------+----------------------------------+----------------------+
| [07] HARDENING     | TITAN Suite + AppArmor + earlyoom| [OK] ICMP Drop + MAC |
+--------------------+----------------------------------+----------------------+
| [08] BOOT / UKI    | systemd-boot + dracut UKI (x2)   | [FX] root=UUID guard |
|                    | 7.3.6-arch1-1 & 6.18.64-1-lts    |      + masked kifd   |
+--------------------+----------------------------------+----------------------+
| [09] RESCUE USB    | Ventoy Dual-ISO + Portable AI    | [OK] SystemRescue    |
|                    | EMPEZAR.sh + RESCATE_NOTEBOOK v2 |      13.03 + SHA-256 |
+--------------------+----------------------------------+----------------------+
```

## /* DOCUMENTATION INDEX */

| File | Description |
| --- | --- |
| [LOGBOOK.md](LOGBOOK.md) | Complete chronological engineering timeline (`Oct 21 – Nov 25, 2026`), forensic debugging sessions, regressions, and verified fixes. |
| [SECURITY.md](SECURITY.md) | 8-layer TITAN security architecture (`sysctl`, `firewalld` DROP, DNS-over-TLS, MAC randomization, Docker isolation, AppArmor, ClamAV). |
| [INVENTORY.md](INVENTORY.md) | Verified map of `~/.local/bin/` automation scripts, user symlinks, XDG paths, and `/etc/` kernel/udev overrides. |
| [LICENSE](LICENSE) | MIT License. |

## /* FEATURED FORENSIC CASE STUDIES */

- **[2026-11-25 — Dota 2 Native Vulkan `120 FPS → 20 FPS` Regression (`Mesa 26.3.1` `VK_AMD_anti_lag`)](LOGBOOK.md#2026-11-25--dota-2-20-fps-regression-forensic-investigation-mesa-2631-vk_amd_anti_lag-1s-radv-shader-cache--pure-cachyos-gaming-pipeline):** Live stack sampling (`eu-stack`/`gdb`) and ELF disassembly (`objdump`) of `librendersystemvulkan.so` + `libvulkan_radeon.so` isolated a ~40 ms main-thread `clock_nanosleep` stall inside `vkAntiLagUpdateAMD` (`os_time_nanosleep_until`) triggered by `r_low_latency 1`. Resolved via read-only config guardrails in `titan-game-run v3.1`, dedicated NVMe RADV shader caching (`<1s` boot), `2799 MHz` GPU DPM unlocking, and `sRGB` Direct Scanout (`120–150 real FPS` + 100% VAC compliance).
- **[2026-11-21 — Dracut UKI Boot Lockout Recovery & `.cmdline` Guardrail](LOGBOOK.md#2026-11-21--uki-boot-lockout-recovery-log-hygiene-32-gib-nvme-cleanup-offline-docs--rescue-usb-v20):** Recovered unbootable UKI PE images lacking `root=UUID=...` when `kernel_cmdline+=` bypassed dracut's `hostonly_cmdline`, and built pre-flight `objcopy` `.cmdline` verification.
- **[2026-11-19 — Awakened PoE Trade Electron `app.asar` `ENOENT` & FUSE `SIGBUS` Fix](LOGBOOK.md#november-19--awakened-poe-trade-appasar-enoent--fuse-mount-crash-fix--system-wide-us-english-locale--keyboard-lock):** Binary-patched Electron ASAR stream error handling and eliminated `/tmp/.mount_*` AppImage FUSE volatility.

## /* SYSTEM SPECIFICATION */

```c
struct system_state {
    const char *distro;       /* "EndeavourOS (Arch Linux, BUILD_ID=2026.08.15)" */
    const char *installed_at; /* "2026-10-21 17:56 -03" */
    const char *kernel;       /* "7.3.6-arch1-1 (+ linux-lts 6.18.64-1 fallback UKI)" */
    const char *scheduler;    /* "sched_ext scx_lavd 1.1.4 (--performance) + ananicy-cpp" */
    const char *desktop;      /* "KDE Plasma 6 / Wayland only (Direct Scanout + AllowTearing)" */
    const char *cpu_gpu;      /* "AMD Ryzen 5 8500G (5.08 GHz) / Radeon 740M (2799 MHz, RADV)" */
    const char *memory;       /* "32 GiB DDR5-6000 (28 GiB OS + 4 GiB UMA VRAM) + 13.5 GiB ZRAM" */
    const char *storage;      /* "/dev/nvme0n1p2 - 467 GiB NVMe PCIe 4.0" */
};
```

## /* ACTIVE DAEMONS & SERVICES */

```text
[SYSTEMD // SYSTEM]
  * blindaje-seguridad.service   * firewalld.service        * systemd-resolved.service
  * NetworkManager.service       * scx_loader.service       * ananicy-cpp.service
  * apparmor.service             * earlyoom.service         * clamav-freshclam.service
  * docker.service               * power-profiles-daemon    * fstrim.timer / paccache.timer

[SYSTEMD // USER]
  * baliza-watchdog.service      * pipewire.socket          * wireplumber.service
  * matrix-auto-backup.timer     * xdg-user-dirs.service    * docker-desktop (15s stop cap)
```

_Última revisión: 2026-11-25 20:05_
