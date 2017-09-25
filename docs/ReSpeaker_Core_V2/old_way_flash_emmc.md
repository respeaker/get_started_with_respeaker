# The old way to flash eMMC

### For Windows Users

1. Download from Github:
```
git clone https://github.com/respeaker/rkbin.git
```
2. Download from [百度云盘](https://pan.baidu.com/s/1c2piKW4#list/path=%2F) or [Google Drive](https://drive.google.com/drive/folders/0B7R2TH-ioqAKQjBfZGp0M3VaVjQ): `boot.img` `linaro-rootfs.img` `u-boot/uboot.img` `AndroidTool_Release_v2.31.zip` `DriverAssitant_v4.4.zip`

3. Unzip `DriverAssitant_v4.4.zip` and install `DriverInstall`

4. Unzip `AndroidTool_Release_v2.31.zip` and run `AndroidTool`. Configure address, name and file path as the following picture:
![](/img/emmc-1.png)

5. Press the `UPDATE` button on ReSpeaker Core V2 and connect its `OTG` port to your PC.

6. Click `执行` to flash emmc, it takes about 10min. When it finish, reboot ReSpeaker Core V2 and that is ok.
![](/img/emmc-2.png)

### For Linux User

1. Download from Github:
```
git clone https://github.com/respeaker/rkbin.git
cd rkbin
```
2. Download from [百度云盘](https://pan.baidu.com/s/1c2piKW4#list/path=%2F) or [Google Drive](https://drive.google.com/drive/folders/0B7R2TH-ioqAKQjBfZGp0M3VaVjQ): `boot.img` `linaro-rootfs.img` `u-boot/uboot.img`

3. Press the `UPDATE` button on ReSpeaker Core V2 and connect its `OTG` port to your PC.

4. run flash-emmc commands:
```
sudo ./tools/upgrade_tool ul rk32/rk322x_loader_v1.04.232.bin
sudo ./tools/upgrade_tool di -p rk32/rk3229_parameter
sudo ./tools/upgrade_tool wl 0x2000 uboot.img
sudo ./tools/upgrade_tool wl 0x4000 rk32/trust.img
sudo ./tools/upgrade_tool wl 0x6000 boot.img
sudo ./tools/upgrade_tool wl 0x3e000 linaro-rootfs.img
sudo ./tools/upgrade_tool rd
```

### For Mac User

You need to use a Windows or Linux virtual machine to achieve this.
