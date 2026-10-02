# loongson-2k0300-uboot

在 GitHub Actions 中交叉编译 **龙芯 2K0300 / 久久派 99PAI Wi-Fi 版** 的 U-Boot 固件。

## 用途

配合「久久派从 U 盘更新固件」流程使用（U-Boot `bootmenu`）：

| 产物 | 用途 |
|---|---|
| `u-boot-with-spl.bin` | 写入 SPI NOR 的 `uboot` 分区（`bootmenu → Update u-boot`） |
| `ls2k300_99pi_wifi.dtb` | 写入 SPI NOR 的 `dtb` 分区（`bootmenu → Update dtb`，U 盘上命名为 `dtb.bin`） |

## 编译配置

- 源码：<https://gitee.com/open-loongarch/u-boot>（广东龙芯维护）
- defconfig：`loongson_2k300_99pi_wifi_defconfig`
- 工具链：Ubuntu 24.04 官方源的 `gcc-14-loongarch64-linux-gnu`（新世界 ABI 2.0）
- 关键配置：
  - `CONFIG_DEFAULT_DEVICE_TREE="ls2k300_99pi_wifi"`
  - `CONFIG_BOOTCOMMAND="... sf read ${fdt_addr} dtb; ext4load mmc 0:${syspart} ${loadaddr} /boot/uImage; bootm"`

## 使用

1. 在仓库的 **Actions** 页选择 `Build Loongson 2K0300 U-Boot`，点 **Run workflow**（或直接 push 到 `main`）。
2. 编译完成后，在该次运行的 **Artifacts** 下载 `loongson-2k0300-uboot`。
3. 解压得到 `u-boot-with-spl.bin` 与 `ls2k300_99pi_wifi.dtb`，按 U 盘更新流程放入 U 盘 `/update/`（dtb 命名为 `dtb.bin`）。
