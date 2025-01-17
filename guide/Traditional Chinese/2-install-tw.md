<img align="right" src="https://raw.githubusercontent.com/erdilS/Port-Windows-11-Xiaomi-Pad-5/main/nabu.png" width="425" alt="Windows 11 Running On A Xiaomi Pad 5">

# 在 Xiaomi Pad 5 上安裝 Windows

## 安裝 Windows

### 請先準備下面的步驟

- [Windows on ARM 映像](https://uupdump.net/)
- [UEFI boot 映像](https://raw.githubusercontent.com/erdilS/Port-Windows-11-Xiaomi-Pad-5/main/images/xiaomi-nabu_secureboot-v2.img)
- [驅動文件](https://github.com/map220v/MiPad5-Drivers/releases/latest)

### 請進入 Fastboot 模式, 並輸入以下命令來啟动到 Recovery
```cmd
fastboot boot <recovery.img>
```

#### 執行 msc 模式腳本
```cmd
adb shell msc
```

### 重新分配驅動器號

#### 啟動 Windows 磁碟管理員
> 請確認您的 Xiaomi Pad 5 被 Windows 識別為磁碟
```cmd
diskpart
```

#### 選中平板上的 Windows 分區
> 使用 `list volume` 以找到名為「WINNABU」的分區
```diskpart
select volume <number>
```

#### 分配 Windows 分區到 `X` 盤
```diskpart
assign letter=x
```

#### 選中平板上的 ESP 分區
> 使用 `list volume` 以找到名為「ESPNABU」的分區
```diskpart
select volume <number>
```

#### 分配 ESP 分區到 `Y` 盤
```diskpart
assign letter=y
```

#### 退出 DiskPart
```diskpart
exit
```

### 安裝
> **使用實際 install.wim 路徑取代 `<path/to/install.wim>`**
> 
> `install.wim` 位於您先前創建的 Windows on ARM ISO 中的 `Source` 文件夾, 可通過解壓或掛載 ISO 來取得這個檔案
```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
```

### 安裝驅動程式
> 您可以下载驱动程序[这里](https://github.com/map220v/MiPad5-Drivers/releases/latest)

```cmd
打开带有驱动程序的文件夹并运行 OfflineUpdater.cmd
```

### 重建 Windows EFI
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

## 啟動到 Windows

### 備份現在的 Boot 分區
```cmd
adb shell "dd if=/dev/block/platform/soc/1d84000.ufshc/by-name/boot$(getprop ro.boot.slot_suffix) of=/tmp/boot.img"
```

### 提取 Boot 鏡像到電腦
```cmd
adb pull /tmp/boot.img
```

### 重啟到 Bootloader
```cmd
adb reboot bootloader
```

### 下載並刷入與您顯示面板相對應的 UEFI Boot 鏡像
> 下载[UEFI图像](https://raw.githubusercontent.com/erdilS/Port-Windows-11-Xiaomi-Pad-5/main/images/xiaomi-nabu_secureboot-v2.img)

```cmd
fastboot flash boot <path to image>
```

### 回退到 Android **可選**
> 使用您之前備份過的 Boot 鏡像, 並刷入到當前激活的 slot
```cmd
fastboot flash boot boot.img
```

## 完成
