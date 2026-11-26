# Upstream Bug Reports — Mesa RADV `VK_AMD_anti_lag` 20 FPS Stall (`#16959`) & Valve Dota 2

## 1. Pre-Submission Triage Summary

1. **Duplicate Search on Mesa GitLab (`gitlab.freedesktop.org/mesa/mesa`):**
   - **MATCH FOUND (Open Upstream Issue):** [**Mesa `#16959`** — *radv: anti-lag runaway underpacing after a sudden GPU load change (60 → 20 FPS for 20–50 s, GPU mostly idle)*](https://gitlab.freedesktop.org/mesa/mesa/-/issues/16959) (opened `2026-11-21`).
   - **Why commenting on `#16959` is critical for upstream triage:** `#16959` was originally reported via a complex Windows translation chain (`UE5 Reflex → OptiScaler fakenvapi → Anti-Lag 2 DX12 → vkd3d-proton → RADV`). Our case reproduces the **exact same self-sustaining ~40–50 ms `os_time_nanosleep_until` runaway (`120 FPS → 20 FPS`, GPU idle at `35–39%` even with `power_dpm_force_performance_level=high`)** in a **100% pure, unmodded, first-party Native Linux Vulkan title (`Dota 2` → `CAMDAntiLagVulkan` → `vkAntiLagUpdateAMD`)** on an **RDNA3 APU (`Radeon 740M PHOENIX2`, `GFX1103`)**, and stays **permanently stuck at ~20 FPS** rather than recovering after 20–50 seconds.
2. **Regression Window (`pacman.log`):**
   - `mesa 1:26.2.7-1` (`2026-11-12`): **120+ FPS** (prior to native `VK_AMD_anti_lag` / `vk_frame_pacer` landing in Mesa `26.3`, MR `!42048`).
   - `mesa 1:26.3.1-2` (`2026-11-20`) & `1:26.3.1-3` (`2026-11-23`): **20–24 FPS** (`~41.6–50 ms` frametime).
3. **Duplicate Search on Valve Dota 2 GitHub (`github.com/ValveSoftware/Dota-2`):**
   - **0 existing issues** found for `VK_AMD_anti_lag` / `vkAntiLagUpdateAMD` / `r_low_latency`. Ready to file as a new issue so Valve is aware both of the RADV `26.3.1` interaction and that `setting.useadvanced "0"` silently forces `r_low_latency "1"` back on.

---

## 2. Ready-to-Post Comment on Mesa GitLab Issue `#16959`

**Target URL:** https://gitlab.freedesktop.org/mesa/mesa/-/issues/16959

````markdown
### Pure Native Linux Vulkan Reproducer (No Wine / Proton / fakenvapi): Dota 2 Native Client (`120+ FPS → permanent 20 FPS` on RDNA3 iGPU `PHOENIX2`)

Confirming this exact `VK_AMD_anti_lag` / `vk_frame_pacer` runaway underpacing in **100% native Linux Vulkan** (**Dota 2** native client via `CAMDAntiLagVulkan` -> `vkAntiLagUpdateAMD`, zero translation layers or `fakenvapi`/`OptiScaler` involved).

In Dota 2's native Vulkan renderer, instead of recovering after 20–50 seconds, the feedback loop **locks permanently at ~20–24 FPS (~41–50 ms frame time) indefinitely** as soon as a scene/replay/demo loads with `r_low_latency 1` (AMD Anti-Lag 2 enabled), while GPU utilization drops to ~35–39% even with clocks forced to `high` (`2799 MHz`). Toggling `r_low_latency 0` in the in-game console immediately restores **120–150 FPS (~6.6–8.3 ms frame time)** within the same frame.

This strongly corroborates @chura.vip's finding that measuring the first-to-last submission span (`frame_time`) feeds the anti-lag sleep back into the estimator on engines that submit multiple command buffers across the frame (e.g. Panorama UI + 3D scene passes in Source 2).

#### Environment
- **Mesa:** `26.3.1-arch1.3` (`radv`) — regression after upgrading from `1:26.2.7-1` -> `1:26.3.1-2` (where native `VK_AMD_anti_lag` was introduced)
- **GPU:** AMD Radeon 740M Graphics (`RADV PHOENIX2`, `0x1002:0x15c8`, RDNA3 / GFX1103 iGPU, 4 CUs), 4096 MiB fixed BIOS UMA frame buffer
- **CPU / RAM:** AMD Ryzen 5 8500G (`amd-pstate-epp` `performance`, 5.08 GHz), Gigabyte A620M H, 32 GiB Dual-Channel DDR5-6000 (`MCLK=3000 MHz`, `FCLK=2000 MHz`)
- **OS / Kernel:** EndeavourOS (Arch Linux), kernel `7.3.6-arch1-1`
- **Desktop:** KDE Plasma 6.8.3 Wayland (`sRGB` Direct Scanout, `AllowTearing=true`, 1920×1080 @ 74.99 Hz; Dota 2 running via `Steam Linux Runtime 3.0 (sniper)`)

#### Steps to Reproduce (100% deterministic)
1. Launch native Linux Dota 2 (`Steam Linux Runtime 3.0`, `-vulkan`) with AMD Anti-Lag 2 enabled (`r_low_latency 1`, `fps_max 150`).
2. Load Hero Demo or spectate any live match.
3. Observe framerate immediately clamped at **20–24 FPS** (`~41.6–50 ms`), CPU low, and GPU busy at **~35–39%** even with `/sys/class/drm/card1/device/power_dpm_force_performance_level` forced to `high` (`sclk = 2799 MHz`).
4. Run `r_low_latency 0` in the Dota 2 console -> framerate immediately jumps to **120–150 FPS** (`gpu_busy_percent` rises normally to ~65–85%).
5. Running the Windows `dota2.exe` build on the exact same machine via Proton (`-dx11` / DXVK, where `VK_AMD_anti_lag` is not used by `amd_ags_x64.dll`) also runs at a steady **120+ FPS**.

#### Main Thread Backtrace & ELF Offsets (`eu-stack` / `gdb` / `objdump`)
Sampling the live `20 FPS` `dota2` main thread (`TID 158307`) shows `SYSCALL=230` (`clock_nanosleep`) on 100% of consecutive samples:

```text
TID 158307 (main thread, WCHAN=0, SYSCALL=230 clock_nanosleep):
#0  0x00007f52baaa08f2 -> libc.so.6 + 0xa08f2
#1  0x00007f5283173c37 -> libvulkan_radeon.so + 0x373c37  (os_time_nanosleep_until + 0x67 -> call clock_nanosleep@GLIBC_2.17)
#2  0x00007f52b10d5d7e -> librendersystemvulkan.so + 0x4d4d7e  (CAMDAntiLagVulkan -> vkAntiLagUpdateAMD)
#3  0x00007f52b117e805 -> librendersystemvulkan.so + 0x57d805
#4  0x00007f52b710ad6b -> libengine2.so + 0x309d6b
#5  0x00007f52b711e441 -> libengine2.so + 0x31d441
```

*(Note: the intermediate RADV frame is elided in the call stack because `libvulkan_radeon.so + 0xcf13d` tail-calls `jmp 0x373bd0 <os_time_nanosleep_until>`).*

<details>
<summary><code>vulkaninfo --summary</code></summary>

```text
Vulkan Instance Version: 1.4.363

Devices:
========
GPU0:
	apiVersion         = 1.4.363
	driverVersion      = 26.3.1
	vendorID           = 0x1002
	deviceID           = 0x15c8
	deviceType         = PHYSICAL_DEVICE_TYPE_INTEGRATED_GPU
	deviceName         = AMD Radeon 740M Graphics (RADV PHOENIX2)
	driverID           = DRIVER_ID_MESA_RADV
	driverName         = radv
	driverInfo         = Mesa 26.3.1-arch1.3
	conformanceVersion = 1.4.5.3
```
</details>
````

---

## 3. Ready-to-Post Issue on Valve Dota 2 Tracker (`ValveSoftware/Dota-2`)

**Target URL:** https://github.com/ValveSoftware/Dota-2/issues/new

**Title:** `[Linux / Vulkan] AMD Anti-Lag 2 (r_low_latency 1) caps native Dota 2 to ~20 FPS on Mesa 26.3 RADV (VK_AMD_anti_lag) and auto-re-enables under basic video presets`

````markdown
### Description
On Mesa `26.3.1` (`RADV`), which newly exposes native `VK_AMD_anti_lag`, running the native Linux Dota 2 Vulkan client with **AMD Anti-Lag 2** enabled (`r_low_latency 1`) locks the game into a permanent **~20–24 FPS** cap (`~41–50 ms` frametime) with the GPU mostly idle (`~35–39%` busy even at max 2799 MHz DPM clock).

Additionally, if the user switches Video settings to Basic presets (`"setting.useadvanced" "0"` in `video.txt`), Dota 2 automatically re-enables `"setting.r_low_latency" "1"` on launch whenever `VK_AMD_anti_lag` is advertised by the driver, re-triggering the 20 FPS lock even after manually disabling it in console.

### Upstream Driver Tracking
This interacts with Mesa RADV's `vk_frame_pacer` submission-span calculation tracked upstream in **Mesa `#16959`** (`https://gitlab.freedesktop.org/mesa/mesa/-/issues/16959`): because Source 2 submits multiple command buffers across a frame, RADV's first-to-last submission span includes the Anti-Lag sleep itself, creating a self-sustaining ~40–50 ms sleep inside `CAMDAntiLagVulkan` -> `vkAntiLagUpdateAMD` -> `os_time_nanosleep_until` (`clock_nanosleep`).

### System Information
- **OS:** EndeavourOS (Arch Linux), Kernel `7.3.6-arch1-1`, KDE Plasma 6.8.3 Wayland
- **CPU / RAM:** AMD Ryzen 5 8500G, 32 GiB DDR5-6000 Dual-Channel
- **GPU / Driver:** AMD Radeon 740M (`RADV PHOENIX2`, `0x1002:0x15c8`, 4 GiB UMA), `Mesa 26.3.1-arch1.3` (Vulkan `1.4.363`)
- **Runtime:** `Steam Linux Runtime 3.0 (sniper)` (Native Linux client)

### Steps to Reproduce
1. Launch native Linux Dota 2 on Mesa `26.3.1` (`RADV`) with `r_low_latency 1`.
2. Enter Hero Demo or spectate a match -> FPS is stuck at **20–24 FPS**; Thread 1 spends ~100% of frame time in `librendersystemvulkan.so` (`CAMDAntiLagVulkan` -> `vkAntiLagUpdateAMD`) -> `libvulkan_radeon.so` (`os_time_nanosleep_until`).
3. Enter `r_low_latency 0` in console -> FPS immediately jumps to **120–150 FPS**.
````
