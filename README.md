# CHR on VPS

**Bash script to automatically install MikroTik Cloud Hosted Router (CHR) on a VPS.**  
It downloads and extracts the official CHR image, injects an autorun script with network configuration (IP + gateway taken from the host Linux) and admin password, writes the image to the VPS disk, and reboots.

⚠️ **Warning:** This script completely overwrites the system disk. Use only on fresh VPS instances.

---

## Features

- Downloads the official MikroTik CHR image (**fixed to 7.19.4**).
- Extracts the `.img` file from the `.zip` package.
- Mounts the image and injects an `autorun.scr` into the `rw/` partition:
  - Configures static IP and gateway from the current Linux VPS.
  - Sets the admin password (`NEW_PASSWORD`).
  - Sets system identity (`IDENTITY`).
- Uses Linux `sysrq` (`u`, `s`, `b`) for safe remount, sync, and reboot.
- Works on **Debian/Ubuntu** and **CentOS/RHEL** VPS running in **BIOS boot mode**.

---

## Requirements

- Root privileges.
- VPS in **BIOS (legacy) boot mode**.  
  ⚠️ CHR raw `.img` does **not boot on UEFI-only VPS** (e.g., DigitalOcean, Vultr). Use the official `.iso` in that case.
- Tools: `wget`, `unzip`, `losetup`, `mount`, `umount`, `dd`, `lsblk`, `findmnt`.  
  (The script attempts to auto-install missing packages.)

---

## Usage

```bash
# Clone the repository
git clone https://github.com/hreskiv/chr-on-vps.git
cd chr-on-vps

# Run installer with a custom password
NEW_PASSWORD="Strong!Pass123" ./chr-install.sh
