# ImmortalWrt Image Builder for Linksys WRT1900AC v2

Automated firmware builder for **Linksys WRT1900AC v2** using GitHub Actions + ImmortalWrt 24.10.6 Image Builder, based on the [noviachen/Image-Builder](https://github.com/noviachen/Image-Builder) template.

## Hardware Information

| Item | Details |
|------|---------|
| Device | Linksys WRT1900AC v2 |
| Target | mvebu / cortexa9 |
| SoC | Marvell Armada 385 88F6820 |
| Wireless | 88W8864 (2.4G + 5G, mwlwifi) |
| Flash | 128MB |
| RAM | 256MB DDR3 |
| PROFILE | `linksys_wrt1900ac-v2` |

## Firmware Version

- **ImmortalWrt 24.10.6**
- Architecture: `mvebu/cortexa9`

## Preinstalled Packages

| Category | Packages |
|----------|----------|
| LuCI Core | luci-light, luci-compat, luci-i18n-base-zh-cn, luci-i18n-firewall-zh-cn, luci-i18n-package-manager-zh-cn |
| Theme | luci-theme-argon |
| Dual-Partition | luci-app-advanced-reboot + zh-cn translation |
| Scheduled Reboot | luci-app-autoreboot + zh-cn translation |
| UPnP | luci-app-upnp + zh-cn translation |
| Storage Mount | automount, block-mount, kmod-usb3, kmod-usb-storage, kmod-usb-storage-uas, kmod-fs-ext4/vfat/ntfs3/exfat, e2fsprogs |
| Network Enhancements | dnsmasq-full, wpad-openssl, ip-full |
| Wireless Drivers | kmod-mwlwifi, mwlwifi-firmware-88w8864 |
| System Tools | autocore, bash, curl, wget, ca-certificates, ca-bundle |

## Repository Structure
.

├── .github/

│   └── workflows/

│       └── image-builder.yml      # GitHub Actions build workflow

├── packages.list                  # Package list (one per line)

├── uci-custom                     # First-boot config script (written to /etc/uci-defaults/99-custom)

├── packages/                      # Optional: extra .ipk files (must be mvebu/cortexa9 architecture)

└── README.md

## How to Use

### 1. Fork This Repository

Click **Use this template → Create a new repository** at the top right to create your own copy.

### 2. Customize (Optional)

- **`packages.list`**: Add/remove packages (one per line, `#` for comments, `-packagename` to remove default packages)
- **`uci-custom`**: Shell script executed on first boot (set LAN IP, password, Wi-Fi, etc.)
- **`packages/`**: Place extra `.ipk` files here (must be `arm_cortex-a9_neon` architecture)

### 3. Trigger Build

1. Go to the **Actions** tab in your repository
2. Select **ImmortalWrt Image Builder**
3. Click **Run workflow**
   - `Target PROFILE`: defaults to `linksys_wrt1900ac-v2` (usually no need to change)
   - `Image Builder tar.zst URL`: pre-filled with 24.10.6 mvebu/cortexa9
4. Wait 3–8 minutes

### 4. Download Firmware

After the build completes, download from the **Artifacts** section at the bottom of the workflow run page:

- `linksys_wrt1900ac-v2/` directory containing:
  - `*-squashfs-sysupgrade.bin` → Upgrade from existing OpenWrt/ImmortalWrt
  - `*-squashfs-factory.img` → Flash from stock Linksys firmware

## Important Notes

- ⚠️ **PROFILE name**: In 24.10.x, the correct profile for WRT1900AC v2 is `linksys_wrt1900ac-v2` (note the hyphen between `ac` and `v2`). Using the old name `linksys_wrt1900acv2` will cause a build failure.
- ⚠️ **Wireless driver**: WRT1900AC v2 uses the `mwlwifi` driver. Stability may not match ath79 platforms; some 5G clients may have compatibility issues.
- ⚠️ **Dual boot**: Linksys devices have dual firmware partitions. If a flash fails, you can switch partitions manually or via the `advanced-reboot` web UI.
- ⚠️ **NTFS write support**: Uses the kernel `ntfs3` driver (`kmod-fs-ntfs3`), no need for `ntfs-3g`. If your old USB HDD enclosure has UAS compatibility issues, add a `usb-storage.quirks` blacklist parameter.
- ⚠️ **Flash space**: 128MB Flash is sufficient for all packages listed above. If installing large additional apps (e.g., Docker), check free space first.

## Related Links

- [ImmortalWrt Official Site](https://immortalwrt.org/)
- [ImmortalWrt 24.10.6 Downloads](https://downloads.immortalwrt.org/releases/24.10.6/targets/mvebu/cortexa9/)
- [noviachen/Image-Builder Original Template](https://github.com/noviachen/Image-Builder)
- [OpenWrt WRT1900AC v2 Device Page](https://openwrt.org/toh/linksys/wrt1900ac_v2)

## License

This repository contains build configuration scripts only. Firmware copyright belongs to the ImmortalWrt project.
