openwrt-net-conf
================

Openwrt network config files, mostly helpful for supporting various USB based GSM/UMTS/HSDPA dongles.


Procedure for identifying how to get shit to work:

1. Follow these to get openWRT to a point where it can switch usb modes: https://github.com/ixaxaar/openwrt-net-conf

2. Once that is done, every connected dongle will show up as a bunch of ttyUSB's, mostly 3-4 USB TTY devices (e.g. /dev/ttyUSB0)

3. Check which USB TTY supports AT commands

Terminal 1:
```
cat /dev/ttyUSB0
```

Terminal 2:
```
echo 'AT' > /dev/ttyUSB0
```

If `cat` gives an output like so:
```
OK
```
or 
```
AT
OK
```

that means you've got your ttyUSB port to which pppd can be pointed to.

4. Change the network config to point to the USB TTY, e.g.:
```
config interface wan
        option ifname  ppp0
        option pincode 1234
        option device  /dev/ttyUSB0
        option apn     www
        option proto   3g
	option 'pppd_options' 'debug noipdefault refuse-chap refuse-mschap refuse-mschap-v2 refuse-eap' 
```
