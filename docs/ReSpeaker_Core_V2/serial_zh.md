### 如何设置板子调试串口为普通串口

Note: 本教程基于20180801版本固件编写。

1， 添加板子描述文件
拷贝[rk3229-respeaker-v2.dtb](https://github.com/respeaker/get_started_with_respeaker/raw/master/docs/ReSpeaker_Core_V2/rk3229-respeaker-v2.dtb)
到板子的/boot/dtb/4.4.138-respeaker-r0/目录下面

```
sudo cp rk3229-respeaker-v2.dtb /boot/dtb/4.4.138-respeaker-r0/
```
2, 修改板子的启动脚本
修改/boot/uEnv.txt 与下面一致
```
respeaker@v2:~$ sudo cat /boot/uEnv.txt 
uname_r=4.4.138-respeaker-r0
earlycon=uart8250,mmio32,0x11020000 
console=ttyS1,115200n8 rw
#uuid=
dtb=rk3229-respeaker-v2.dtb
#dtb=
cmdline=coherent_pool=1M quiet init=/lib/systemd/systemd
##enable  Flasher:
##make sure, these tools are installed: dosfstools rsync
#cmdline=init=/opt/scripts/init-eMMC-flasher-respeaker.sh

```

3， 重新启动板子

4， 测试
```
sudo pip install pyserial
#输入字符会自动发送，接受字符会自动显示
sudo miniterm.py /dev/ttyS2 115200
```
