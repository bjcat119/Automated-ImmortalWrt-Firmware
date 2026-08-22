[🇨🇳 中文版](README.md) | [🇺🇸 English Version](README_EN.md)

# ImmortalWrt 24.10.6 for Linksys WRT1900AC v2

[![ImmortalWrt](https://img.shields.io/badge/ImmortalWrt-24.10.6-brightgreen)](https://immortalwrt.org/)
[![Target](https://img.shields.io/badge/target-mvebu%2Fcortexa9-blue)](https://downloads.immortalwrt.org/releases/24.10.6/targets/mvebu/cortexa9/)
[![PROFILE](https://img.shields.io/badge/profile-linksys_wrt1900ac--v2-orange)](https://openwrt.org/toh/linksys/wrt1900ac_v2)

使用 GitHub Actions + ImmortalWrt Image Builder 自动构建 **Linksys WRT1900AC v2** 专用固件。

---

## 硬件信息

| 项目 | 详情 |
|------|------|
| 设备 | Linksys WRT1900AC v2 |
| SoC | Marvell Armada 385 88F6820 (Cortex-A9) |
| 无线 | 88W8864 — 2.4G + 5G（mwlwifi） |
| Flash | 128MB |
| RAM | 256MB DDR3 |
| 架构 | `mvebu/cortexa9` |
| PROFILE | `linksys_wrt1900ac-v2` |

---

## 预装软件

### 网络核心（替换默认）
| 包 | 说明 |
|----|------|
| `dnsmasq-full` | 替换默认 dnsmasq，支持 DNSSEC / Nftables |
| `wpad-openssl` | 替换 wpad-basic，支持 WPA3 / 802.11r |
| `ip-full` | 完整 iproute2 工具集 |

### LuCI & 主题
| 包 | 说明 |
|----|------|
| `luci-light` + 中文翻译 | 精简 LuCI 核心 |
| `luci-theme-argon` | Argon 主题 |
| `luci-app-package-manager` | 网页包管理器 |

### 系统管理
| 包 | 说明 |
|----|------|
| `luci-app-advanced-reboot` | 双分区切换 / 刷 factory（Linksys 救砖神器） |
| `luci-app-autoreboot` | 定时重启 |
| `luci-app-partexp` | 网页分区扩容（一键扩展 rootfs） |
| `luci-app-upnp` | UPnP / NAT-PMP |
| `autocore` | CPU 频率 / 温度等状态信息 |
| `bash` `curl` `wget` `ca-certificates` `ca-bundle` | 基础工具 |

### 存储 & USB
| 包 | 说明 |
|----|------|
| `automount` + `block-mount` | U 盘 / 移动硬盘自动挂载 |
| `kmod-usb3` `kmod-usb-storage` `kmod-usb-storage-uas` | USB 3.0 存储驱动 |
| `kmod-fs-ext4` `kmod-fs-vfat` `kmod-fs-ntfs3` `kmod-fs-exfat` | 全格式文件系统支持 |
| `e2fsprogs` | ext 分区工具 |

### 无线驱动
| 包 | 说明 |
|----|------|
| `kmod-mwlwifi` | mwlwifi 驱动（Marvell 88W8864） |
| `mwlwifi-firmware-88w8864` | 对应固件 |
| `iwinfo` | 无线信息工具 |

---

## 仓库结构
├── .github/workflows/

│   └── image-builder.yml      # 构建流程

├── packages.list              # 软件包清单（本文件）

├── uci-custom                 # 首次启动脚本 → /etc/uci-defaults/99-custom

├── packages/                  # 可选：额外 .ipk（需 arm_cortex-a9_a9_vfpv3-d16 架构）

├── README.md

└── README_EN.md
