### Graphics and Display

1, Hardware property

- GPU
    - ARM Mali400 MP2
    - High performance OpenGL ES1.1 and 2.0, OpenVG1.1 etc
    - Embedded 4 shader cores with shared hierarchical tiler
- Display
    - HDMI interface: Support YUV420 4k x 2k @ 60fps. Support for 4k x 2k and 3D video formats. Support for up to 10.2bps bandwidth. Compliant HDMI 2.0. Compliance HDMI compliance Test specification 1.4. Support HDCP 2.2.

2, Software

 Graphics and Display's software is not ready. 
 - No GPU Software
 - HDMI does not support hot-plug, you need to plugin the HDMI, then power on 
 - No HDMI audio

3, Screen resolution

 You cannt change the screen resolution. Default resolution get from monitor edid. But you can check current screen resolution.
```sh
respeaker@v2:~$ export DISPLAY=:0
respeaker@v2:~$ xrandr 
xrandr: Failed to get size of gamma for output default
Screen 0: minimum 1920 x 1080, current 1920 x 1080, maximum 1920 x 1080
default connected 1920x1080+0+0 0mm x 0mm
   1920x1080      0.00* 
```