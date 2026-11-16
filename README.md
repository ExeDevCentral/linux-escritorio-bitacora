```c
/* ============================================================================
 *  LINUX_ESCRITORIO(7)        SYSTEM LOGBOOK & FIXES        LINUX_ESCRITORIO(7)
 *
 *  FILE:    README.md
 *  HOST:    EndeavourOS (Arch Linux) // Kernel 7.3.4-arch1-2 // x86_64
 *  ARCH:    AMD Ryzen 5 8500G + Radeon 740M (amdgpu / RADV) + 32GB DDR5
 *  AUTHOR:  Exequiel Echevarria <ExeDevCentral>
 *  LICENSE: MIT
 *
 *  DESCRIPTION:
 *    Registro tecnico en estilo clasico UNIX/C de compatibilidad,
 *    correcciones de drivers, hardening y gaming bajo Linux de escritorio.
 *    Documenta que funciona directo, que se rompio y como se reparo.
 * ============================================================================ */
```

```text
+------------------------------------------------------------------------------+
|  MATRIZ DE COMPATIBILIDAD & FIXES  [ VERIFICADO: 2026-11-16 ]                |
+--------------------+----------------------------------+----------------------+
| SUBSISTEMA         | HARDWARE / STACK                 | ESTADO & FIX         |
+--------------------+----------------------------------+----------------------+
| [01] APU / GPU     | Ryzen 5 8500G / Radeon 740M      | [OK] ppfeaturemask   |
|                    | amdgpu + RADV + powerprofilesctl |      + RADV SAM      |
+--------------------+----------------------------------+----------------------+
| [02] WIFI USB      | Realtek 8188eu (wlan0 Hotspot)   | [FX] blacklist       |
|                    |                                  |      rtl8xxxu + WD   |
+--------------------+----------------------------------+----------------------+
| [03] RED / DNS     | enp5s0 (r8169) + resolved        | [OK] TCP BBR + fq    |
|                    |                                  |      DoT + -LLMNR    |
+--------------------+----------------------------------+----------------------+
| [04] GAMING        | PoE Standalone / WoW 3.3.5a      | [OK] Proton Exp /    |
|                    | Dota 2 / ananicy-cpp / MangoHud  |      Proton-CachyOS  |
+--------------------+----------------------------------+----------------------+
| [05] AUDIO         | PipeWire + WirePlumber           | [OK] System Default  |
+--------------------+----------------------------------+----------------------+
| [06] HARDENING     | Suite TITAN (sysctl + firewalld) | [OK] ICMP Drop + MAC |
+--------------------+----------------------------------+----------------------+
```

## /* INDICE DE DOCUMENTACION */

| Archivo | Descripcion |
| --- | --- |
| [BITACORA.md](BITACORA.md) | Linea de tiempo cronologica (`21 Oct - 16 Nov 2026`) con instalaciones, regresiones y fixes. |
| [SEGURIDAD.md](SEGURIDAD.md) | Blindaje TITAN en 8 capas (`sysctl`, `firewalld` DROP, DNS-over-TLS, MAC, Docker, ClamAV). |
| [INVENTARIO_Y_ORDEN.md](INVENTARIO_Y_ORDEN.md) | Mapa de scripts en `~/.local/bin/`, symlinks de usuario y archivos en `/etc/`. |

## /* ESPECIFICACION DEL SISTEMA */

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

## /* DAEMONS & SERVICIOS ACTIVOS */

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
