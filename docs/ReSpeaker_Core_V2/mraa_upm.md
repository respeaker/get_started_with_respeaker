#  MRAA and UPM
This document will introduce how to use MRAA and UPM on respeaker v2 platform.

## Update  MRAA and UPM libraries to latest version

First, we need to check the kernel version of the system we're running, if you're not sure that you flashed the system image of version `20171128` and later.

```sh
uname -a
```

If you're using system image prior to version `4.4.95-respeaker-r2`, please upadte the kernel first with

```sh
sudo apt update
sudo apt install linux-image-4.4.95-respeaker-r2
```

Then we install the latest MRAA and UPM packages.

```sh
sudo apt install  python-mraa python-upm libmraa1 libupm1 mraa-tools
```

## Check your platform information

```sh
#only have bus 0 and id=03(/dev/i2c-3), 0 is the i2c number for mraa and upm
respeaker@v2:~$ mraa-i2c list
Bus   0: id=03 type=linux 

#mraa gpio numbers and system gpio numbers and it's pinmux
respeaker@v2:~$ mraa-gpio list
00      GPIO91: GPIO 
01         VCC: 
02      GPIO43: GPIO 
03     GPIO127: GPIO 
04      GPIO17: GPIO 
05      GPIO67: GPIO 
06         GND: 
07      GPIO13: GPIO 
08    I2C2_SCL: I2C  
09    I2C2_SDA: I2C  
10         VCC: 
11         GND: 
12      GPIO66: GPIO 
```

And here we have the description of the PIN defines for the ReSpeaker Core V2 board. https://github.com/respeaker/mraa/blob/master/docs/axolotl.md

## use MRAA library

The MRAA library wraps the kernel implementations of GPIO/I2C interfaces and makes Linux boxes easy to interact with phsical sensors / peripharals. The basic use case of MRAA library is the GPIO. 

### GPIO example

This is a simple GPIO example. It toggles the `0` GPIO. `0` is the MRAA index in the [pin mapping table](https://github.com/respeaker/mraa/blob/master/docs/axolotl.md#pin-mapping).

```sh
respeaker@v2:~$ python
Python 2.7.13 (default, Jan 19 2017, 14:48:08) 
[GCC 6.3.0 20170118] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import mraa
>>> x = mraa.Gpio(0)
>>> x.dir(mraa.DIR_OUT)
7
>>> x.write(0)
7
>>> x.write(1)
7
>>> 
```

### PIR sensor example

In this example, we're gonna to listen on the trigger of the [Grove PIR sensor](https://www.seeedstudio.com/Grove-PIR-Motion-Sensor%EF%BC%88BISS0001%EF%BC%89--p-802.html), in Python code.

```python
import mraa
import time
import sys

def on_trigger(gpio):
    print("pin " + repr(gpio.getPin(True)) + " = " + repr(gpio.read()))

pin = 0

try:
    x = mraa.Gpio(pin)
    print("Starting ISR for pin " + repr(pin))
    x.dir(mraa.DIR_IN)
    # respeaker v2 only support EDGE_BOTH
    x.isr(mraa.EDGE_BOTH, on_trigger, x)
    var = raw_input("Press ENTER to stop")
    x.isrExit()
except ValueError as e:
    print(e)
```

Save the above code snippet into a file, e.g. `mraa_pir.py`. Wire the Grove PIR sensor's `D1` pin with the ReSpeaker Core V2's `0` header pin. Don't forget to wire the VCC and GND at the same time. Then run the code with

```shell
$ sudo python mraa_pir.py
```

### More about MRAA

MRAA's Python documentation main page: http://iotdk.intel.com/docs/master/mraa/python/

Intel has written lots of examples, please refer to https://software.intel.com/en-us/mraa-sdk/documentation

## use UPM librarys

UPM have supported a lots sensors. https://iotdk.intel.com/docs/master/upm/modules.html. But we didnt confirm every sensors works
on repeaker v2 platform.
There is a grove digital light sensor examples.
```
respeaker@v2:~$ cat tsl2561.py 
#!/usr/bin/env python
# Author: Zion Orent <zorent@ics.com>
# Copyright (c) 2015 Intel Corporation.
#
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the
# "Software"), to deal in the Software without restriction, including
# without limitation the rights to use, copy, modify, merge, publish,
# distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so, subject to
# the following conditions:
#
# The above copyright notice and this permission notice shall be
# included in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
# NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
# LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
# WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

from __future__ import print_function
import time, sys, signal, atexit
from upm import pyupm_tsl2561 as upmTsl2561

def main():
    # Instantiate a digital light sensor TSL2561 on I2C
    myDigitalLightSensor = upmTsl2561.TSL2561()

    ## Exit handlers ##
    # This function stops python from printing a stacktrace when you hit control-C
    def SIGINTHandler(signum, frame):
        raise SystemExit

    # This function lets you run code on exit, including functions from myDigitalLightSensor
    def exitHandler():
        print("Exiting")
        sys.exit(0)

    # Register exit handlers
    atexit.register(exitHandler)
    signal.signal(signal.SIGINT, SIGINTHandler)

    while(1):
        print("Light value is " + str(myDigitalLightSensor.getLux()))
        time.sleep(1)
if __name__ == '__main__':
    main()
```
result: 

```sh
respeaker@v2:~$ python tsl2561.py       
Light value is 0
Light value is 38
Light value is 20
Light value is 54
Light value is 13
Light value is 44
Light value is 31    
```

