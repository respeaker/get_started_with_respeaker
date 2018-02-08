### Graphics and Display

1, Hardware property

- GPU
    - ARM Mali400 MP2
    - High performance OpenGL ES1.1 and 2.0, OpenVG1.1 etc
    - Embedded 4 shader cores with shared hierarchical tiler
- Display
    - HDMI interface: Support YUV420 4k x 2k @ 60fps. Support for 4k x 2k and 3D video formats. Support for up to 10.2bps bandwidth. Compliant HDMI 2.0. Compliance HDMI compliance Test specification 1.4. Support HDCP 2.2.

2, Software

 Graphics's software is not fully functional. It can work in:
 
 - Displaying the system GUI or the headless command line screen
 
 But with limitations:
 
 - The GPU is not fully driven, we'll get a low refresh rate with the HDMI display and there's no video decoding acceleration.
 - HDMI does not support hot-plug, we need to plug the HDMI cable first, then power on the board
 - No HDMI audio
 - No arbitrary screen resolution (See below `3, Screen resolution`)

3, Screen resolution

 We can not change the screen resolution. The default resolution is gotten from the EDID of the monitor. We can check the current screen resolution from the EDID detection with:
 
```sh
respeaker@v2:~$ export DISPLAY=:0
respeaker@v2:~$ xrandr 
xrandr: Failed to get size of gamma for output default
Screen 0: minimum 1920 x 1080, current 1920 x 1080, maximum 1920 x 1080
default connected 1920x1080+0+0 0mm x 0mm
   1920x1080      0.00* 
```
