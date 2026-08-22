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

## 使用方法

### 1. Fork 本仓库

点击右上角 **Use this template → Create a new repository**，在自己的账号下创建副本。

### 2. 按需修改配置

- **`packages.list`**：增删软件包（一行一个，`#` 开头为注释，减号开头如 `-ppp` 表示删除默认包）
- **`uci-custom`**：首次启动后自动执行的 shell 脚本（改 LAN IP、设密码、开 Wi-Fi 等）
- **`packages/`**：放入额外的 `.ipk` 文件（架构必须是 `arm_cortex-a9_neon`）

### 3. 触发构建

1. 进入仓库的 **Actions** 标签
2. 选择 **ImmortalWrt Image Builder**
3. 点击 **Run workflow**
   - `Target PROFILE`：默认 `linksys_wrt1900ac-v2`（一般不用改）
   - `Image Builder tar.zst URL`：默认已填 24.10.6 mvebu/cortexa9 地址
4. 等待 3–8 分钟

### 4. 下载固件

构建完成后，在 workflow 运行页面底部的 **Artifacts** 区域下载：

- `linksys_wrt1900ac-v2/` 目录，内含：
  - `*-squashfs-sysupgrade.bin` → 从已有 OpenWrt/ImmortalWrt 系统升级
  - `*-squashfs-factory.img` → 从 Linksys 原厂固件刷入

## 注意事项

- ⚠️ **PROFILE 名称**：24.10 系列中 WRT1900AC v2 的 profile 是 `linksys_wrt1900ac-v2`（注意 `ac` 和 `v2` 之间有横杠），用旧名 `linksys_wrt1900acv2` 会构建失败。
- ⚠️ **无线驱动**：WRT1900AC v2 使用 `mwlwifi` 驱动，稳定性不如 ath79 平台，部分 5G 客户端可能有兼容问题。
- ⚠️ **双分区**：Linksys 双 boot 设计，刷机失败可手动切换分区救砖（也可通过 `advanced-reboot` 网页操作）。
- ⚠️ **NTFS 读写**：使用内核 `ntfs3` 驱动（kmod-fs-ntfs3），无需 ntfs-3g；极少数老移动硬盘盒若 UAS 不兼容，需在启动参数加 quirk 黑名单。
- ⚠️ **Flash 空间**：128MB Flash 足够安装上述所有包，但如需额外安装大型应用（如 Docker），建议先确认剩余空间。

## 相关链接

- [ImmortalWrt 官网](https://immortalwrt.org/)
- [ImmortalWrt 24.10.6 下载](https://downloads.immortalwrt.org/releases/24.10.6/targets/mvebu/cortexa9/)
- [noviachen/Image-Builder 原模板](https://github.com/noviachen/Image-Builder)
- [OpenWrt WRT1900AC v2 设备页](https://openwrt.org/toh/linksys/wrt1900ac_v2)

## License

本仓库仅包含构建配置脚本，固件版权归 ImmortalWrt 项目所有。
