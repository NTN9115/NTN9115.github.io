---
layout: post
title:  "树莓派IR设备配置"
date:   2020-11-25 22:15:38 +0800
categories: linux IR
typora-root-url: ..
---

## 消息来源

信了淘宝的邪，丢了个[链接](http://raspberrypiwiki.com/Raspberry_Pi_IR_Control_Expansion_Board#LIRC_Software)
来,看上面写的```/boot/config.txt```加点东西

```
dtoverlay=lirc-rpi,gpio_in_pin=18,gpio_out_pin=17
```

配好设备弄死没显示在```/dev```里面，勉为其难的看了下```/boot```下的readme

```
Name:   lirc-rpi
Info:   This overlay has been deprecated and removed - see gpio-ir
Load:   <Deprecated>
```

我擦，上当了，看看```gpio-ir```

```
Name:   gpio-ir
Info:   Use GPIO pin as rc-core style infrared receiver input. The rc-core-
        based gpio_ir_recv driver maps received keys directly to a
        /dev/input/event* device, all decoding is done by the kernel - LIRC is
        not required! The key mapping and other decoding parameters can be
        configured by "ir-keytable" tool.
Load:   dtoverlay=gpio-ir,<param>=<val>
Params: gpio_pin                Input pin number. Default is 18.

        gpio_pull               Desired pull-up/down state (off, down, up)
                                Default is "up".

        invert                  "1" = invert the input (active-low signalling).
                                "0" = non-inverted input (active-high
                                signalling). Default is "1".

        rc-map-name             Default rc keymap (can also be changed by
                                ir-keytable), defaults to "rc-rc6-mce"

```
 
还有 ```gpio-ir-tx```

```
Name:   gpio-ir-tx
Info:   Use GPIO pin as bit-banged infrared transmitter output.
        This is an alternative to "pwm-ir-tx". gpio-ir-tx doesn't require
        a PWM so it can be used together with onboard analog audio.
Load:   dtoverlay=gpio-ir-tx,<param>=<val>
Params: gpio_pin                Output GPIO (default 18)

        invert                  "1" = invert the output (make it active-low).
                                Default is "0" (active-high).

```

配置改成

```
dtoverlay=gpio-ir
dtoverlay=gpio-ir-tx,gpio_pin=17
```

其他选项默认值即可，重启，然后```/dev/lirc0```就出现了。
