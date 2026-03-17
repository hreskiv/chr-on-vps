# CHR on VPS

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Shell](https://img.shields.io/badge/Shell-Bash-green.svg)]()
[![RouterOS](https://img.shields.io/badge/RouterOS-v7-orange.svg)](https://mikrotik.com/download)

Bash script to automatically install MikroTik Cloud Hosted Router (CHR) on any Linux VPS. Downloads the official image, injects network config from the host, writes to disk, and reboots — ready to use in under a minute.

---

## Quick Start

```bash
curl -L -O https://github.com/hreskiv/chr-on-vps/raw/refs/heads/main/chr-install.sh
NEW_PASSWORD="YourStrongPass!" bash chr-install.sh
```

> **Warning:** This script completely overwrites the system disk. Use only on fresh VPS instances.

---

## Features

- Downloads the latest stable MikroTik CHR image automatically
- Auto-detects host IP, gateway, and target disk
- Injects `autorun.scr` to configure CHR on first boot
- Hardens default services (disables API, Telnet, FTP, DHCP client, bandwidth server)
- Works on **Debian/Ubuntu** and **CentOS/RHEL**

---

## How It Works

1. **Detect** — reads the current VPS network configuration (IP, gateway, interface)
2. **Download** — fetches the official CHR `.zip` from MikroTik (latest stable or specified version)
3. **Mount & Inject** — mounts the image, writes an `autorun.scr` script with network config, password, and identity
4. **Write** — `dd` the image to the target disk
5. **Reboot** — uses `sysrq` for safe remount, sync, and reboot into CHR

---

## Usage

### Clone and run

```bash
git clone https://github.com/hreskiv/chr-on-vps.git
cd chr-on-vps
NEW_PASSWORD="YourStrongPass!" ./chr-install.sh
```

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `ROS_VER` | latest stable | RouterOS version (e.g. `7.18`) |
| `NEW_PASSWORD` | `changeMeNOW!` | Admin password |
| `IDENTITY` | `chr-<version>` | Router identity |
| `TARGET_DISK` | auto-detected | Disk to overwrite (e.g. `/dev/vda`) |

### Examples

```bash
# Specific RouterOS version
ROS_VER="7.18" NEW_PASSWORD="S3cure!Pass" ./chr-install.sh

# Custom identity
IDENTITY="edge-router" NEW_PASSWORD="S3cure!Pass" ./chr-install.sh

# Explicit target disk
TARGET_DISK="/dev/sda" NEW_PASSWORD="S3cure!Pass" ./chr-install.sh
```

---

## What autorun.scr Configures

On first boot, the injected script:

- Sets static IP address and default gateway (from host)
- Sets admin password
- Sets system identity
- Sets DNS to `8.8.8.8`
- Disables: API, Telnet, FTP, DHCP client, bandwidth server

---

## After Installation

After reboot, CHR is accessible on the same IP the Linux VPS was using:

```bash
ssh admin@<your-vps-ip>
```

Verify the configuration:

```routeros
/ip address print
/ip route print
/system identity print
```

---

## Tested On

- **DigitalOcean** — Debian, Ubuntu

Tested on another provider? Open an issue or PR to add it to the list.

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg)](https://www.digitalocean.com/?refcode=a5fd7bce5490&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)

---

## Troubleshooting

**Can't SSH after reboot**
The VPS console (e.g. DigitalOcean Droplet Console) should still work. Check `/ip address print` and `/ip route print` via console.

**Wrong IP configured**
Override via VPS console:

```routeros
/ip address add address=CORRECT.IP/MASK interface=ether1
/ip route add dst-address=0.0.0.0/0 gateway=GATEWAY.IP
```

**Disk not detected**
Set `TARGET_DISK` explicitly: `TARGET_DISK="/dev/vda" ./chr-install.sh`

---

## Links

- [MikroTik CHR Documentation](https://help.mikrotik.com/docs/spaces/ROS/pages/18350234/Cloud+Hosted+Router+CHR)
- [RouterOS Downloads](https://mikrotik.com/download)

---

## Contributing

Contributions, issues, and feature requests are welcome.
Feel free to open a [Pull Request](https://github.com/hreskiv/chr-on-vps/pulls) or [Issue](https://github.com/hreskiv/chr-on-vps/issues).

---

## License

[MIT](LICENSE)
