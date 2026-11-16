# Linux Escritorio — Bitácora Técnica, Compatibilidad & Correcciones

Registro técnico público de ingeniería, estabilidad de hardware, hardening y compatibilidad gaming sobre **EndeavourOS (Arch Linux)** con **AMD Ryzen 5 8500G + Radeon 740M**, documentando qué funciona directo (*out-of-the-box*), qué falló tras actualizaciones y cómo se resolvió cada incidencia.

---

## Matriz Rápida de Compatibilidad & Fixes

| Área | Hardware / Stack | Estado | Fix / Ajuste Aplicado |
| --- | --- | --- | --- |
| **APU / GPU** | AMD Ryzen 5 8500G + Radeon 740M (`amdgpu`) | ✅ Estable | `ppfeaturemask=0xffffffff`, perfil `balanced` diario + `performance` automático en juegos, `RADV_PERFTEST=sam` |
| **Wi-Fi USB / Hotspot** | Realtek `8188eu` (`wlan0`) | ⚠️ Requirió fix | Blacklist de `rtl8xxxu`, `rtw_power_mgnt=0`, `baliza-watchdog.service` y suspensión KDE extendida |
| **Red / DNS** | Ethernet `enp5s0` (`r8169`) + `systemd-resolved` | ✅ Optimizado | `TCP BBR` + `fq`, `tcp_timestamps=1`, DNS-over-TLS estricto (`1.1.1.1`) y `-LLMNR` |
| **Gaming (Proton / Wine)** | Path of Exile (Standalone), WoW 3.3.5a, Dota 2 | ✅ Optimizado | `Proton - Experimental` / `Proton-CachyOS`, `ananicy-cpp` (15k reglas CachyOS), `gamemoderun`, `MangoHud` |
| **Audio** | PipeWire + WirePlumber + EasyEffects | ✅ Estable | Fijado `System Default` en Wine/WoW (`Sound_EnableHardware "0"`) para evitar desvíos HDMI |
| **Seguridad / Hardening** | Suite TITAN (`sysctl`, `firewalld`, MAC) | ✅ Activo | Modo fantasma ICMP, `firewalld` DROP, MAC aleatoria en escaneos, ClamAV y `.pacnew` watcher |

---

## Documentación del Repositorio

- **[BITACORA.md](BITACORA.md)** — Línea de tiempo cronológica completa (`21 Oct – 16 Nov 2026`) con instalaciones, regresiones detectadas, diagnósticos y soluciones verificadas.
- **[SEGURIDAD.md](SEGURIDAD.md)** — Detalle de las 8 capas de hardening TITAN (`sysctl`, `firewalld`, DNS-over-TLS, privacidad MAC, aislamiento Docker, ClamAV y permisos).
- **[INVENTARIO_Y_ORDEN.md](INVENTARIO_Y_ORDEN.md)** — Mapa de scripts en `~/.local/bin/`, symlinks de usuario y configuraciones en `/etc/`.

---

## Especificación del Sistema (verificado el 16/11/2026)

| Componente | Valor Verificado |
| --- | --- |
| **Distribución** | EndeavourOS (Arch Linux, `BUILD_ID=2026.08.15`) |
| **Instalación base** | `2026-10-21 17:56` |
| **Kernel** | `7.3.4-arch1-2` |
| **Entorno gráfico** | KDE Plasma 6 sobre Wayland (`plasmalogin.service`) |
| **CPU / GPU** | AMD Ryzen 5 8500G / Radeon 740M (`amdgpu` / `RADV`) |
| **RAM / Swap** | `27 GiB` DDR5 + `13.5 GiB` ZRAM (`zstd`, `vm.swappiness=100`, `zswap=0`) |
| **Almacenamiento** | `/dev/nvme0n1p2` — `467 GiB` (`278 GiB` usados, `63%`) |

---

## Servicios Activos Relevantes

- **Sistema (`systemd`):** `blindaje-seguridad.service`, `firewalld.service`, `systemd-resolved.service`, `NetworkManager.service`, `ananicy-cpp.service`, `lactd.service`, `ratbagd.service`, `clamav-freshclam.service`, `docker.service`, `containerd.service`, `power-profiles-daemon.service`, `fstrim.timer`, `paccache.timer`, `reflector.timer`.
- **Usuario (`systemd --user`):** `baliza-watchdog.service`, `pipewire.socket`, `pipewire-pulse.socket`, `wireplumber.service`, `pm2.service`, `xdg-user-dirs.service`.
