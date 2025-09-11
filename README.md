# CHR on VPS

**Bash script to automatically install MikroTik Cloud Hosted Router (CHR) on a VPS.**  
It downloads and extracts the official CHR image, injects an autorun script with network configuration (IP + gateway taken from the host Linux) and admin password, writes the image to the VPS disk, and reboots.

⚠️ **Warning:** This script completely overwrites the system disk. Use only on fresh VPS instances.

---

## Features

- Downloads the official MikroTik CHR image (**default 7.19.4**).
- Extracts the `.img` file from the `.zip` package.
- Mounts the image and injects an `autorun.scr` into the `rw/` partition:
  - Configures static IP and gateway from the current Linux VPS.
  - Sets the admin password (`NEW_PASSWORD`).
  - Sets system identity (`IDENTITY`).
- Uses Linux `sysrq` (`u`, `s`, `b`) for safe remount, sync, and reboot.
- Works on **Debian/Ubuntu** and **CentOS/RHEL** VPS.
- Tested on DigitalOcean
- [![DigitalOcean Referral Badge](https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg)](https://www.digitalocean.com/?refcode=a5fd7bce5490&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)

---

## Requirements

- Root privileges.
- Tools: `wget`, `unzip`, `losetup`, `mount`, `umount`, `dd`, `lsblk`, `findmnt`.  
  (The script attempts to auto-install missing packages.)

---

## Usage

```bash
# Clone the repository
git clone https://github.com/hreskiv/chr-on-vps.git
cd chr-on-vps

# or download with CURL
curl -L -O https://github.com/hreskiv/chr-on-vps/raw/refs/heads/main/chr-install.sh

# Run installer with a custom password
NEW_PASSWORD="Strong!Pass123" ./chr-install.sh
```
## Environment variables

```text
ROS_VER - version of the RouterOS version (default: 7.19.4)
NEW_PASSWORD – admin password (default: changeMeNOW!).
IDENTITY    – router identity (default: chr-7.19.4).
TARGET_DISK – system disk to overwrite (auto-detected, or manually e.g. /dev/vda).
```
## Notes

- After reboot, CHR will be accessible on the same IP address the Linux VPS was using.  
- Default user: `admin` with password set via `NEW_PASSWORD`.  

---

## Contributing

Contributions, issues, and feature requests are welcome!  
Feel free to open a Pull Request or Issue if you’d like to improve this project.  

📧 Contact: [ihor@hreskiv.pl](mailto:ihor@hreskiv.pl)
