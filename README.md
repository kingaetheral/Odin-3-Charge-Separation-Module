# Odin-3-Charge-Separation-Module
Charge separation (bypass charging) for the Unihertz Odin 3. Keeps the battery at a target state of charge while the phone runs off the charger, reducing battery wear during long plugged-in sessions (navigation, hotspot duty, desk use, media boxes).

How it works
On install, the module probes for the kernel's native charge control interfaces:
/sys/class/qcom-battery/charge_control_en
/sys/class/power_supply/battery/charge_control_end_threshold
/sys/class/power_supply/battery/charge_control_start_threshold
When the threshold nodes are available, the module runs in THRESHOLD mode: the kernel itself enforces the start/stop levels, so separation keeps working even when the device is in deep sleep — no userspace polling loop required.
Requirements
Unihertz Odin 3
Rooted with Magisk
A kernel exposing the charge control sysfs nodes above (stock kernel works)
Installation
Download the latest Odin3-ChargeSeparation-vX.X.X-KingAether.zip from Releases.
Install via Magisk → Modules → Install from storage.
Reboot to activate.
Upgrading over an existing install preserves your configuration.
Configuration
Settings live in: /data/adb/charge_sep/config.conf

Edit with a root file manager or shell, then reboot (or re-trigger the service) to apply. To reset to defaults, delete the file and reinstall the module.
Tip: If separation never engages with TARGET=100, set TARGET=99 instead. Some kernels interpret an end threshold of 100 as "charge fully / feature off."
Verifying it's working
After reboot, plug in and let the battery reach your target. Check:

cat /sys/class/power_supply/battery/charge_control_end_threshold
cat /sys/class/power_supply/battery/charge_control_start_threshold

The values should match your config, and the battery percentage should hold at the target while the device draws power from the charger.
Uninstalling
Remove the module in Magisk and reboot. Kernel thresholds return to their defaults. Optionally delete /data/adb/charge_sep/ to remove leftover config.
Notes
A/B slot devices and system-as-root layouts are supported.
This module changes charging behavior only; it does not modify system partitions.
