# make-linux-speedy
Make Linux run very fast (NVIDIA Only)

## Boot Options
```
quiet loglevel=4 nvidia-drm.modeset=1 nowatchdog mitigations=off split_lock_detect=off intel_pstate=support_acpi_ppc tsc=reliable clocksource=tsc vm.vfs_cache_pressure=1 splash
```
What does each parameter do?
- quiet: Suppresses most boot messages (kernel log)
- loglevel=4: Sets the verbosity of kernel logs (4 = warning and above)
- splash: Shows a graphical splash screen during boot (if enabled in init system)
- nvidia-drm.modeset=1: Enables DRM KMS (Kernel Mode Setting) for NVIDIA drivers, generally just latency overhead and can make the system feel a little more resposnive
- nowatchdog: Disables kernel and hardware watchdogs (useful for performance/debugging), it's also great for Wayland compatibility
- mitigations=off: Master switch that disables all mitigations above (redundant with specific ones, but ensures coverage), could be anywhere from 3-8% gain in FPS
- split_lock_detect=off: Disables warnings or crashes from split-lock detection (helps performance on newer Intel CPUs), helps 1% lows by disabling split lock detect
- intel_pstate=support_acpi_ppc: Tells intel_pstate driver to obey ACPI _PPC limits (useful on laptops with thermal limits or undervolting), about ~1-2% gain in performance from my testing
- tsc=reliable: Marks the Time Stamp Counter (TSC) as a reliable time source, TSC is used for reducing latency
- clocksource=tsc: Forces the TSC as the system clock source (usually more performant than HPET or ACPI), used for reducing latency
- vm.vfs_cache_pressure=1: Drastically reduces how aggressively the kernel drops directory and inode cache (helps filesystem performance at the cost of RAM)
