### Bluetooth Audio

1. A2DP

You can set respeaker v2 board as A2DP slave/master device. But you need restart pulseaudio first.
```sh
#restart pulsaudio
respeaker@v2:~$ pulseaudio --kill
```
Then setup bluetooth
```sh
respeaker@v2:~$ bluetoothctl 
[NEW] Controller E0:76:D0:37:6E:B2 v2 [default]
Agent registered
[bluetooth]# scan on
Discovery started
[CHG] Controller E0:76:D0:37:6E:B2 Discovering: yes
[NEW] Device AC:BC:32:8D:F1:B6 AC-BC-32-8D-F1-B6
[NEW] Device 65:90:F4:04:E3:E0 65-90-F4-04-E3-E0
[NEW] Device 4F:C0:E6:1E:F7:5D 4F-C0-E6-1E-F7-5D
[NEW] Device D4:61:9D:21:06:93 D4-61-9D-21-06-93
[NEW] Device 51:E0:41:3A:63:63 51-E0-41-3A-63-63
[NEW] Device FE:90:CF:8B:D5:78 Puck.js d578
[NEW] Device D2:77:98:22:67:C8 MI Band 2
[NEW] Device 55:38:4C:D6:BD:B0 55-38-4C-D6-BD-B0
[CHG] Device FE:90:CF:8B:D5:78 UUIDs: 6e400001-b5a3-f393-e0a9-e50e24dcca9e
[NEW] Device 73:DB:71:FD:37:2B 73-DB-71-FD-37-2B
[NEW] Device E8:8F:34:8B:07:91 WeLoop XH3 28E20C
[NEW] Device 60:4A:BD:B6:31:B4 60-4A-BD-B6-31-B4
[NEW] Device 88:0F:10:10:BC:4E 88-0F-10-10-BC-4E
[NEW] Device 6C:5A:B5:DE:A5:2E DingDong智能音箱
[NEW] Device F0:1B:6C:88:8F:9B vivo X6D
[NEW] Device 60:36:DD:FD:71:C6 c
[NEW] Device 50:01:D9:BE:5E:7C 123456
[bluetooth]# scan off
[CHG] Device 50:01:D9:BE:5E:7C RSSI is nil
[CHG] Device 60:36:DD:FD:71:C6 TxPower is nil
[CHG] Device 60:36:DD:FD:71:C6 RSSI is nil
[CHG] Device F0:1B:6C:88:8F:9B RSSI is nil
[CHG] Device 6C:5A:B5:DE:A5:2E RSSI is nil
[CHG] Device 88:0F:10:10:BC:4E RSSI is nil
[CHG] Device 60:4A:BD:B6:31:B4 RSSI is nil
[CHG] Device E8:8F:34:8B:07:91 RSSI is nil
[CHG] Device 73:DB:71:FD:37:2B RSSI is nil
[CHG] Device 55:38:4C:D6:BD:B0 RSSI is nil
[CHG] Device D2:77:98:22:67:C8 RSSI is nil
[CHG] Device FE:90:CF:8B:D5:78 RSSI is nil
[CHG] Device 51:E0:41:3A:63:63 RSSI is nil
[CHG] Device 04:52:C7:0B:ED:B2 TxPower is nil
[CHG] Device 04:52:C7:0B:ED:B2 RSSI is nil
[CHG] Device D4:61:9D:21:06:93 RSSI is nil
[CHG] Device 4F:C0:E6:1E:F7:5D RSSI is nil
[CHG] Device 65:90:F4:04:E3:E0 RSSI is nil
[CHG] Device AC:BC:32:8D:F1:B6 RSSI is nil
[CHG] Controller E0:76:D0:37:6E:B2 Discovering: no
Discovery stopped
[bluetooth]# pair 50:01:D9:BE:5E:7C
Attempting to pair with 50:01:D9:BE:5E:7C
[CHG] Device 50:01:D9:BE:5E:7C Connected: yes
Request confirmation
[agent] Confirm passkey 778261 (yes/no): yes
[CHG] Device 50:01:D9:BE:5E:7C Modalias: bluetooth:v000Fp1200d1436
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00000000-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001105-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 0000110a-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 0000110c-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001112-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001115-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001116-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 0000111f-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 0000112f-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001132-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001200-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001800-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C UUIDs: 00001801-0000-1000-8000-00805f9b34fb
[CHG] Device 50:01:D9:BE:5E:7C ServicesResolved: yes
[CHG] Device 50:01:D9:BE:5E:7C Paired: yes
Pairing successful
[CHG] Device 50:01:D9:BE:5E:7C ServicesResolved: no
[CHG] Device 50:01:D9:BE:5E:7C Connected: no
[bluetooth]# connect 50:01:D9:BE:5E:7C
Attempting to connect to 50:01:D9:BE:5E:7C
[CHG] Device 50:01:D9:BE:5E:7C Connected: yes
Connection successful
[CHG] Device 50:01:D9:BE:5E:7C ServicesResolved: yes
[123456]# quit
Agent unregistered
[DEL] Controller E0:76:D0:37:6E:B2 v2 [default]
```
You can play music on your phone and the on-board speaker will playback your music.

2. HSP

You can set respeaker v2 board as HSP master device connect your Headset. 
```sh
#start pulseaudio in command line
respeaker@v2:~$ touch .config/pulse/client.conf
respeaker@v2:~$ echo "autospawn = no" >> .config/pulse/client.conf 
respeaker@v2:~$ pulseaudio --kill
respeaker@v2:~$ pulseaudio -vvvv
```
Then setup bluetooth
```
respeaker@v2:~$ bluetoothctl 
[NEW] Controller E0:76:D0:37:6E:B2 v2 [default]
[NEW] Device 50:01:D9:BE:5E:7C 123456
Agent registered
[123456]# scan on
Discovery started
[CHG] Controller E0:76:D0:37:6E:B2 Discovering: yes
[NEW] Device 76:CE:53:73:F4:2E 76-CE-53-73-F4-2E
[NEW] Device 4F:C0:E6:1E:F7:5D 4F-C0-E6-1E-F7-5D
[NEW] Device 04:52:C7:0B:ED:B2 LE-Jeremy' Bose QC35
[NEW] Device D4:61:9D:21:06:93 D4-61-9D-21-06-93
[NEW] Device AC:BC:32:8D:F1:B6 AC-BC-32-8D-F1-B6
[NEW] Device D2:77:98:22:67:C8 MI Band 2
[NEW] Device AC:BC:32:7F:1E:14 AC-BC-32-7F-1E-14
[NEW] Device 51:E0:41:3A:63:63 51-E0-41-3A-63-63
[NEW] Device 47:18:A2:5E:8E:D1 47-18-A2-5E-8E-D1
[NEW] Device 73:DB:71:FD:37:2B 73-DB-71-FD-37-2B
[CHG] Device AC:BC:32:8D:F1:B6 RSSI: -85
[NEW] Device EF:67:4B:F3:4A:A6 fenix 3
[NEW] Device 55:38:4C:D6:BD:B0 55-38-4C-D6-BD-B0
[NEW] Device 68:06:FB:EC:46:7D 68-06-FB-EC-46-7D
[NEW] Device FE:90:CF:8B:D5:78 Puck.js d578
[CHG] Device D2:77:98:22:67:C8 RSSI: -83
[CHG] Device D2:77:98:22:67:C8 UUIDs: 0000fee0-0000-1000-8000-00805f9b34fb
[NEW] Device 42:1E:E3:DE:91:E0 42-1E-E3-DE-91-E0
[NEW] Device E8:8F:34:8B:07:91 WeLoop XH3 28E20C
[NEW] Device 88:0F:10:10:BC:4E MI
[NEW] Device 6C:5A:B5:DE:A5:2E DingDong智能音箱
[NEW] Device 1C:77:F6:1B:A2:C3 1C-77-F6-1B-A2-C3
[NEW] Device F0:1B:6C:88:8F:9B vivo X6D
[NEW] Device 60:36:DD:FD:71:C6 c
[CHG] Device 1C:77:F6:1B:A2:C3 Name: OPPO R9tm
[CHG] Device 1C:77:F6:1B:A2:C3 Alias: OPPO R9tm
[CHG] Device 73:DB:71:FD:37:2B RSSI: -93
[CHG] Device 73:DB:71:FD:37:2B ManufacturerData Key: 0x004c
[CHG] Device 73:DB:71:FD:37:2B ManufacturerData Value:
  0c 0e 00 88 2d e7 a1 af 17 c0 af 12 b4 1a 16 7e  ....-..........~
[CHG] Device 51:E0:41:3A:63:63 ManufacturerData Key: 0x004c
[CHG] Device 51:E0:41:3A:63:63 ManufacturerData Value:
  0c 0e 08 b8 e5 9c cb 42 2d 14 84 83 0c d6 2e 7e  .......B-......~
[NEW] Device 4C:32:75:89:85:1E 4C-32-75-89-85-1E
[CHG] Device 68:06:FB:EC:46:7D ManufacturerData Key: 0x004c
[CHG] Device 68:06:FB:EC:46:7D ManufacturerData Value:
  10 05 0a 10 32 63 2d                             ....2c-         
[CHG] Device 55:38:4C:D6:BD:B0 ManufacturerData Key: 0x004c
[CHG] Device 55:38:4C:D6:BD:B0 ManufacturerData Value:
  0c 0e 08 6c 24 75 af 8a 87 63 fd ff 3e 39 d1 b3  ...l$u...c..>9..
[NEW] Device 0C:A6:94:FB:16:38 0C-A6-94-FB-16-38
[CHG] Device 0C:A6:94:FB:16:38 TxPower: 4
[CHG] Device 0C:A6:94:FB:16:38 Name: JBL GO
[CHG] Device 0C:A6:94:FB:16:38 Alias: JBL GO
[CHG] Device 0C:A6:94:FB:16:38 Modalias: bluetooth:v000Ap8615d0100
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110d-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110b-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110e-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110f-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000111e-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 00001108-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 00001131-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 00001112-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 00001203-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 0000111f-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 00001116-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 00001115-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 0000110c-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 0000110e-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 0000110f-0000-1000-8000-00805f9b34fb
[CHG] Device 1C:77:F6:1B:A2:C3 UUIDs: 0000110a-0000-1000-8000-00805f9b34fb
[CHG] Device F0:1B:6C:88:8F:9B RSSI: -70
[CHG] Device 73:DB:71:FD:37:2B ManufacturerData Key: 0x004c
[CHG] Device 73:DB:71:FD:37:2B ManufacturerData Value:
  0c 0e 00 8a 2d ed 4b 64 78 65 e3 44 a0 be a5 6f  ....-.Kdxe.D...o
[CHG] Device 51:E0:41:3A:63:63 RSSI: -72
[CHG] Device 73:DB:71:FD:37:2B RSSI: -84
[CHG] Device 68:06:FB:EC:46:7D RSSI: -74
[123456]# scan off
[CHG] Device 0C:A6:94:FB:16:38 TxPower is nil
[CHG] Device 0C:A6:94:FB:16:38 RSSI is nil
[CHG] Device 4C:32:75:89:85:1E RSSI is nil
[CHG] Device 60:36:DD:FD:71:C6 TxPower is nil
[CHG] Device 60:36:DD:FD:71:C6 RSSI is nil
[CHG] Device F0:1B:6C:88:8F:9B RSSI is nil
[CHG] Device 1C:77:F6:1B:A2:C3 RSSI is nil
[CHG] Device 6C:5A:B5:DE:A5:2E RSSI is nil
[CHG] Device 88:0F:10:10:BC:4E RSSI is nil
[CHG] Device E8:8F:34:8B:07:91 RSSI is nil
[CHG] Device 42:1E:E3:DE:91:E0 RSSI is nil
[CHG] Device FE:90:CF:8B:D5:78 RSSI is nil
[CHG] Device 68:06:FB:EC:46:7D RSSI is nil
[CHG] Device 55:38:4C:D6:BD:B0 RSSI is nil
[CHG] Device EF:67:4B:F3:4A:A6 RSSI is nil
[CHG] Device 73:DB:71:FD:37:2B RSSI is nil
[CHG] Device 47:18:A2:5E:8E:D1 RSSI is nil
[CHG] Device 51:E0:41:3A:63:63 RSSI is nil
[CHG] Device AC:BC:32:7F:1E:14 RSSI is nil
[CHG] Device D2:77:98:22:67:C8 RSSI is nil
[CHG] Device AC:BC:32:8D:F1:B6 RSSI is nil
[CHG] Device D4:61:9D:21:06:93 RSSI is nil
[CHG] Device 04:52:C7:0B:ED:B2 TxPower is nil
[CHG] Device 04:52:C7:0B:ED:B2 RSSI is nil
[CHG] Device 4F:C0:E6:1E:F7:5D RSSI is nil
[CHG] Device 76:CE:53:73:F4:2E RSSI is nil
Discovery stopped
[CHG] Controller E0:76:D0:37:6E:B2 Discovering: no
[123456]# pair 0C:A6:94:FB:16:38
Attempting to pair with 0C:A6:94:FB:16:38
[CHG] Device 0C:A6:94:FB:16:38 Connected: yes
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 00001108-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110b-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110c-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000110e-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 0000111e-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 UUIDs: 00001200-0000-1000-8000-00805f9b34fb
[CHG] Device 0C:A6:94:FB:16:38 ServicesResolved: yes
[CHG] Device 0C:A6:94:FB:16:38 Paired: yes
Pairing successful
[CHG] Device 0C:A6:94:FB:16:38 ServicesResolved: no
[CHG] Device 0C:A6:94:FB:16:38 Connected: no
[123456]# connect 0C:A6:94:FB:16:38
Attempting to connect to 0C:A6:94:FB:16:38
[CHG] Device 0C:A6:94:FB:16:38 Connected: yes
Connection successful
[CHG] Device 0C:A6:94:FB:16:38 ServicesResolved: yes
[JBL GO]# quuit
Invalid command
[JBL GO]# quit

```
Open pavucontrol-qt you will see the HSP profile 
![](/img/respeakerv2_hsp_1.png)

Record  some audio select bluetooth-voice as input device

![](/img/respeakerv2_hsp_2.png)

- TODO
    - Support HFP protocol
    - Enable  pulsaudio autostart  without kill it
    - A2DP audio stream  not smooth
    - Autoconnect device