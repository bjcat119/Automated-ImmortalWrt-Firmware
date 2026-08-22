[🇨🇳 中文版](README.md) | [🇺🇸 English Version](README_EN.md)
# ImmortalWrt 24.10.6 for Linksys WRT1900AC v2

[![ImmortalWrt](https://img.shields.io/badge/ImmortalWrt-24.10.6-brightgreen)](https://immortalwrt.org/)
[![Target](https://img.shields.io/badge/target-mvebu%2Fcortexa9-blue)](https://downloads.immortalwrt.org/releases/24.10.6/targets/mvebu/cortexa9/)
[![PROFILE](https://img.shields.io/badge/profile-linksys_wrt1900ac--v2-orange)](https://openwrt.org/toh/linksys/wrt1900ac_v2)

Automated firmware builder for **Linksys WRT1900AC v2** using GitHub Actions + ImmortalWrt Image Builder, based on [noviachen/Image-Builder](https://github.com/noviachen/Image-Builder).

---

## Hardware

| Item | Details |
|------|---------|
| Device | Linksys WRT1900AC v2 |
| SoC | Marvell Armada 385 88F6820 (Cortex-A9) |
| Wireless | 88W8864 — 2.4G + 5G (mwlwifi) |
| Flash | 128MB |
| RAM | 256MB DDR3 |
| Architecture | `mvebu/cortexa9` |
| PROFILE | `linksys_wrt1900ac-v2` |

---

## Preinstalled Packages

### Network (replacing defaults)
| Package | Notes |
|---------|-------|
| `dnsmasq-full` | Replaces dnsmasq — DNSSEC, Nftables support |
| `wpad-openssl` | Replaces wpad-basic — WPA3 / 802.11r |
| `ip-full` | Full iproute2 suite |

### LuCI & Theme
| Package | Notes |
|---------|-------|
| `luci-light` + zh-cn translations | Lightweight LuCI core |
| `luci-theme-argon` | Argon theme |
| `luci-app-package-manager` | Web-based package manager |

### System Management
| Package | Notes |
|---------|-------|
| `luci-app-advanced-reboot` | Dual-partition switch / factory flash |
| `luci-app-autoreboot` | Scheduled reboot |
| `luci-app-partexp` | Web-based rootfs expansion |
| `luci-app-upnp` | UPnP / NAT-PMP |
| `autocore` | CPU freq / temp status |
| `bash` `curl` `wget` `ca-certificates` `ca-bundle` | Essentials |

### Storage & USB
| Package | Notes |
|---------|-------|
| `automount` + `block-mount` | Auto-mount USB drives |
| `kmod-usb3` `kmod-usb-storage` `kmod-usb-storage-uas` | USB 3.0 storage drivers |
| `kmod-fs-ext4` `kmod-fs-vfat` `kmod-fs-ntfs3` `kmod-fs-exfat` | Full filesystem support |
| `e2fsprogs` | ext partition tools |

### Wireless
| Package | Notes |
|---------|-------|
| `kmod-mwlwifi` | mwlwifi driver (Marvell 88W8864) |
| `mwlwifi-firmware-88w8864` | Firmware blob |
| `iwinfo` | Wireless info utility |

---

## Repository Structure
├── .github/workflows/

│   └── image-builder.yml      # Build workflow

├── packages.list              # Package list

├── uci-custom                 # First-boot script → /etc/uci-defaults/99-custom

├── packages/                  # Optional: extra .ipk (must be arm_cortex-a9_neon)

├── README.md

└── README_EN.md

---

## How to Build

### 1. Fork This Repo
Click **Use this template → Create a new repository**.

### 2. Customize (Optional)
| File | Purpose |
|------|---------|
| `packages.list` | Add/remove packages, one per line; `-pkgname` to remove defaults |
| `uci-custom` | First-boot script (LAN IP / password / Wi-Fi) |
| `packages/` | Extra `.ipk` files (must be `arm_cortex-a9_neon`) |

### 3. Trigger Build
- **Actions** → **ImmortalWrt Image Builder** → **Run workflow**
- `Target PROFILE`: defaults to `linksys_wrt1900ac-v2`
- Wait 3–8 minutes

### 4. Download Firmware
Download `linksys_wrt1900ac-v2/` from Artifacts:

| File | Use |
|------|-----|
| `*-squashfs-sysupgrade.bin` | Upgrade from OpenWrt / ImmortalWrt |
| `*-squashfs-factory.img` | Flash from stock Linksys firmware |

---

## Important Notes

- ⚠️ **PROFILE name**: 24.10.x uses `linksys_wrt1900ac-v2` (with hyphen). Old name `linksys_wrt1900acv2` will fail.
- ⚠️ **Dual boot**: Two firmware partitions. Hold the **power button for 3 sec** on boot to switch, or use `advanced-reboot` in LuCI.
- ⚠️ **Wireless**: mwlwifi driver — stability varies; some 5G clients may have issues.
- ⚠️ **NTFS**: Uses kernel `ntfs3` driver (no `ntfs-3g` needed). If UAS fails on old USB enclosures, add `usb-storage.quirks`.
- ⚠️ **Expand rootfs**: After first boot, go to LuCI → System → Partition Expansion.

---

## Links

- [ImmortalWrt](https://immortalwrt.org/)
- [24.10.6 mvebu/cortexa9](https://downloads.immortalwrt.org/releases/24.10.6/targets/mvebu/cortexa9/)
- [OpenWrt Device Page](https://openwrt.org/toh/linksys/wrt1900ac_v2)
- [Original Template](https://github.com/noviachen/Image-Builder)

---

*Firmware copyright belongs to the ImmortalWrt project. This repo provides build configuration only.*
