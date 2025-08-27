# chr-on-vps
Bash script to automatically install MikroTik Cloud Hosted Router (CHR) on a VPS. It downloads and extracts the CHR image, injects an autorun script with static IP, gateway, and password from the host Linux, then writes the image to disk and reboots. Works on Debian/Ubuntu and CentOS
