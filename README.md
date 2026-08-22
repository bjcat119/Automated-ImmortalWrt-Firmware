# ImmortalWrt Image Builder for Linksys WRT1900AC v2

使用 GitHub Actions + ImmortalWrt 24.10.6 Image Builder 为 **Linksys WRT1900AC v2** 自动构建固件。

## 硬件信息

| 项目 | 详情 |
|------|------|
| 设备 | Linksys WRT1900AC v2 |
| 平台 | mvebu / cortexa9 |
| SoC | Marvell Armada 385 88F6820 |
| 无线 | 88W8864 (2.4G + 5G, mwlwifi) |
| Flash | 128MB |
| RAM | 256MB DDR3 |
| PROFILE | `linksys_wrt1900ac-v2` |

## 固件版本

- **ImmortalWrt 24.10.6**
- 架构：`mvebu/cortexa9`

## 预装软件包

| 分类 | 包含 |
|------|------|
| LuCI 核心 | luci-light, luci-compat, luci-i18n-base-zh-cn, luci-i18n-firewall-zh-cn, luci-i18n-package-manager-zh-cn |
| 主题 | luci-theme-argon |
| 双分区管理 | luci-app-advanced-reboot + 中文翻译 |
| 定时重启 | luci-app-autoreboot + 中文翻译 |
| UPnP | luci-app-upnp + 中文翻译 |
| 存储挂载 | automount, block-mount, kmod-usb3, kmod-usb-storage, kmod-usb-storage-uas, kmod-fs-ext4/vfat/ntfs3/exfat, e2fsprogs |
| 网络增强 | dnsmasq-full, wpad-openssl, ip-full |
| 无线驱动 | kmod-mwlwifi, mwlwifi-firmware-88w8864 |
| 系统工具 | autocore, bash, curl, wget, ca-certificates, ca-bundle |

## 仓库结构
.

├── .github/

│   └── workflows/

│       └── image-builder.yml      # GitHub Actions 构建流程

├── packages.list                  # 预装软件包清单（一行一个）

├── uci-custom                     # 首次启动配置脚本（写入 /etc/uci-defaults/99-custom）

├── packages/                      # 可选：存放额外 .ipk 文件（需 mvebu/cortexa9 架构）

└── README.md
