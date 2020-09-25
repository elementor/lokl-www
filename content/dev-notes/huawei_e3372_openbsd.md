+++
date = "2018-12-13"
title = "Huawei E3372 USB 3G/4G modem under OpenBSD 6.4"
tags = ["openbsd", "usb modem"]
categories = ["dev notes"]
summary = "Notes on getting connected to 3G/4G in Thailand on the dtac prepaid SIM"
+++

Notes on getting connected to 3G/4G in Thailand on the dtac prepaid SIM

```
obsd# gammu identify
Device               : /dev/cuaU0
Manufacturer         : Huawei
Model                : E3372 (E3372)
Firmware             : 22.329.63.00.74
IMEI                 : 866785033700732
SIM IMSI             : 520050314388175
```

Example `smsd.conf`

```
loglevel = 7

user = _smsd

pidfile = /var/run/smsd/smsd.pid
infofile = /var/run/smsd/smsd.info

logfile = /var/log/smsd/smsd.log

[GSM1]
device = /dev/cuaU0

init = AT^CURC=0

# For this one I have set incoming=no so it doesn't pull
# the existing messages off the phone.
incoming = no
rtscts = no
baudrate = 115200
eventhandler = /home/leon/smstoolsevents.sh
send_delay = 20
```

And a simple script to log events `/home/leon/smstoolsevents.sh`

```


#!/bin/ksh

# script to react to events, like receiving an SMS

echo '' >> /home/leon/logsmsevents
echo 'received a message' >> /home/leon/logsmsevents


```

`smsd` log with above settings gives the following when `smsd` is restarted:

```
2018-12-13 12:24:38,6, GSM1: Modem is registered to a roaming partner network                                                                                                             [79/2614]
2018-12-13 12:24:38,2, smsd: Smsd mainprocess is awaiting the termination of all modem handlers. PID: 830.                                                                                        
2018-12-13 12:24:38,2, GSM1: Modem handler 0 terminated. PID: 61548, was started 18-12-13 12:15:24, up 9 min.                                                                                     
2018-12-13 12:24:38,2, smsd: Smsd mainprocess terminated. PID: 830, was started 18-12-13 12:15:24, up 9 min.                                                                                      
2018-12-13 12:24:38,2, smsd: Smsd v3.1.21 started.
2018-12-13 12:24:38,2, smsd: Running as _smsd:_smsd (590:590).
2018-12-13 12:24:38,7, smsd: Running startup_check (shell): /var/spool/sms/incoming/smsd_script.FZMA0a /tmp/smsd_data.sP5YHd                                                                      
2018-12-13 12:24:38,7, smsd: Done: startup_check (shell), execution time 0 sec., status: 0 (0)
2018-12-13 12:24:38,4, smsd: File mode creation mask: 022 (0644, rw-r--r--).
2018-12-13 12:24:38,5, GSM1: Modem handler 0 has started. PID: 50667.
2018-12-13 12:24:38,5, GSM1: Using check_memory_method 1: CPMS is used.
2018-12-13 12:24:38,5, smsd: Outgoing file checker has started. PID: 80713.
2018-12-13 12:24:38,7, smsd: All PID's: 80713,50667
2018-12-13 12:24:38,6, GSM1: Checking device for incoming SMS
2018-12-13 12:24:38,6, GSM1: Checking if modem is ready
2018-12-13 12:24:39,7, GSM1: -> AT
2018-12-13 12:24:39,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:39,7, GSM1: <- OK
2018-12-13 12:24:39,6, GSM1: Pre-initializing modem
2018-12-13 12:24:39,7, GSM1: -> ATE0
2018-12-13 12:24:39,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:39,7, GSM1: <- OK                                                                                                                                                                
2018-12-13 12:24:39,7, GSM1: -> AT+CMEE=1;+CREG=2
2018-12-13 12:24:40,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:40,7, GSM1: <- OK
2018-12-13 12:24:40,6, GSM1: Checking if modem needs PIN
2018-12-13 12:24:40,7, GSM1: -> AT+CPIN?
2018-12-13 12:24:40,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:40,7, GSM1: <- +CPIN: READY OK
2018-12-13 12:24:40,6, GSM1: Initializing modem
2018-12-13 12:24:40,7, GSM1: -> AT^CURC=0
2018-12-13 12:24:41,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:41,7, GSM1: <- OK
2018-12-13 12:24:41,7, GSM1: -> AT+CSQ
2018-12-13 12:24:41,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:41,7, GSM1: <- +CSQ: 22,99 OK
2018-12-13 12:24:41,6, GSM1: Signal Strength Indicator: (22,99) -69 dBm (Excellent), Bit Error Rate: not known or not detectable                                                                  
2018-12-13 12:24:41,6, GSM1: Checking if Modem is registered to the network
2018-12-13 12:24:41,7, GSM1: -> AT+CREG?
2018-12-13 12:24:42,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:42,7, GSM1: <- +CREG: 2,5,"792D","04947221" OK
2018-12-13 12:24:42,6, GSM1: Modem is registered to a roaming partner network
2018-12-13 12:24:42,6, GSM1: Location area code: 792D, Cell ID: 7221
2018-12-13 12:24:42,7, GSM1: -> AT+CSQ
2018-12-13 12:24:42,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:42,7, GSM1: <- +CSQ: 21,99 OK
2018-12-13 12:24:42,6, GSM1: Signal Strength Indicator: (21,99) -71 dBm (Excellent), Bit Error Rate: not known or not detectable                                                                  
2018-12-13 12:24:42,6, GSM1: Selecting PDU mode
2018-12-13 12:24:42,7, GSM1: -> AT+CMGF=0
2018-12-13 12:24:42,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:42,7, GSM1: <- OK
2018-12-13 12:24:43,7, GSM1: -> AT+CGSN
2018-12-13 12:24:43,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:43,7, GSM1: <- 866785033700732 OK                                                                                                                                        [26/2614]
2018-12-13 12:24:43,5, GSM1: IMEI: 866785033700732
2018-12-13 12:24:43,7, GSM1: -> AT+CIMI
2018-12-13 12:24:43,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:43,7, GSM1: <- 520050314388175 OK
2018-12-13 12:24:43,5, GSM1: IMSI: 520050314388175
2018-12-13 12:24:43,6, GSM1: Checking if reading of messages is supported
2018-12-13 12:24:43,7, GSM1: -> AT+CPMS?
2018-12-13 12:24:44,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:44,7, GSM1: <- +CPMS: "ME",0,20,"ME",0,20,"ME",0,20 OK
2018-12-13 12:24:44,6, GSM1: Checking memory size
2018-12-13 12:24:44,7, GSM1: -> AT+CPMS?
2018-12-13 12:24:44,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:44,7, GSM1: <- +CPMS: "ME",0,20,"ME",0,20,"ME",0,20 OK
2018-12-13 12:24:44,6, GSM1: Used memory is 0 of 20
2018-12-13 12:24:44,6, GSM1: No SMS received
2018-12-13 12:24:54,6, GSM1: Checking device for incoming SMS
2018-12-13 12:24:54,6, GSM1: Checking if modem is ready
2018-12-13 12:24:54,7, GSM1: -> AT
2018-12-13 12:24:55,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:55,7, GSM1: <- OK
2018-12-13 12:24:55,6, GSM1: Pre-initializing modem
2018-12-13 12:24:55,7, GSM1: -> ATE0
2018-12-13 12:24:55,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:55,7, GSM1: <- OK
2018-12-13 12:24:55,7, GSM1: -> AT+CMEE=1;+CREG=2
2018-12-13 12:24:56,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:56,7, GSM1: <- OK
2018-12-13 12:24:56,6, GSM1: Initializing modem
2018-12-13 12:24:56,7, GSM1: -> AT^CURC=0
2018-12-13 12:24:56,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:56,7, GSM1: <- OK
2018-12-13 12:24:56,7, GSM1: -> AT+CSQ
2018-12-13 12:24:57,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:57,7, GSM1: <- +CSQ: 21,99 OK
2018-12-13 12:24:57,6, GSM1: Signal Strength Indicator: (21,99) -71 dBm (Excellent), Bit Error Rate: not known or not detectable                                                                  
2018-12-13 12:24:57,6, GSM1: Checking if Modem is registered to the network
2018-12-13 12:24:57,7, GSM1: -> AT+CREG?
2018-12-13 12:24:57,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:57,7, GSM1: <- +CREG: 2,5,"792D","04947221" OK
2018-12-13 12:24:57,6, GSM1: Modem is registered to a roaming partner network
2018-12-13 12:24:57,6, GSM1: Selecting PDU mode
2018-12-13 12:24:57,7, GSM1: -> AT+CMGF=0
2018-12-13 12:24:57,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:57,7, GSM1: <- OK
2018-12-13 12:24:57,6, GSM1: Checking memory size
2018-12-13 12:24:58,7, GSM1: -> AT+CPMS?
2018-12-13 12:24:58,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:58,7, GSM1: <- +CPMS: "ME",0,20,"ME",0,20,"ME",0,20 OK
2018-12-13 12:24:58,6, GSM1: Used memory is 0 of 20
2018-12-13 12:24:58,6, GSM1: No SMS received
```

and continues to poll 


Now, we need to set up pppd. Assuming that no received activation SMS is due to not registering on the data network yet, so leaving smsd running and will attempt the ppd setup


Much talk is had about HiLink mode vs Stick mode. 

I decided to figure out what mine was being detected as:

```
obsd# usbdevs
Controller /dev/usb0:
addr 01: 8086:0000 Intel, xHCI root hub
addr 02: 12d1:1442 HUAWEI_MOBILE, HUAWEI_MOBILE
addr 03: 05ac:12aa Apple Inc., iPod
Controller /dev/usb1:
addr 01: 8086:0000 Intel, EHCI root hub
addr 02: 8087:8000 Intel, Rate Matching Hub
addr 03: 04f3:012d ELAN, Touchscreen
addr 04: 04ca:7036 SC20A38485AA4C8E3D, Integrated Camera
```

Based on the above, it looks like we're already operating in Stick mode, with `12d1:1442`, else assuming the support in umsm in OpenBSD for this device is handling the USB modeswitching automagically...


Setting up the pppd stuff

I have 

/etc/ppp/peers/e3372:

```
cuaU0
115200
debug
noauth
nocrtscts
:10.254.254.1
ipcp-accept-remote
defaultroute
user isp@cingulargprs.com
demand
active-filter 'not udp port 123'
persist
idle 600
connect "/usr/sbin/chat -v -f /etc/ppp/dtac-chat"

```

/etc/ppp/dtac-chat:
```
TIMEOUT 10
REPORT CONNECT
ABORT BUSY
ABORT 'NO CARRIER'
ABORT ERROR
'' ATZ OK AT&F OK
AT+CGDCONT=1,"IP","www.dtac.co.th" OK
ATD*99***1# CONNECT

```


/etc/ppp/chap-secrets
```
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
isp@cingulargprs.com    *       CINGULAR1

```



Finally, run

 `ifconfig ppp0 create` (should be showing in ifconfig now) and then

`pppd e3372 call`

This doesn't give me much, so I tail `/var/log/daemon`, resulting in:

```
Dec 13 13:09:29 obsd pppd[80914]: pppd 2.3.5 started by leon, uid 0
Dec 13 13:09:29 obsd pppd[80914]: ioctl(TIOCSETD): Device not configured

```

A clue, when I try to manually talk to the modem, `cu -s 115200 -l cuaU0`, I get device is busy. So I stop the smsd daemon and try again:

This time, it connects, but nothing is given back...whereas previously, I think I was able to type commands over the serial connection...

To isolate the cause, I duplicated the peers/e3372 file with another with just these lines

/etc/ppp/peers/dummytest
```
cuaU0
115200
debug
```

which got me one more line in the output:

```
Dec 13 13:40:24 obsd pppd[46299]: pppd 2.3.5 started by leon, uid 0
Dec 13 13:40:27 obsd pppd[46299]: Serial connection established.
Dec 13 13:40:28 obsd pppd[46299]: ioctl(TIOCSETD): Device not configured
```

I'm also seeing the device being disconnected at hangup and then re-detected:

```
Dec 13 13:40:32 obsd /bsd: ucom0 detached
Dec 13 13:40:32 obsd /bsd: umsm0 detached
Dec 13 13:40:32 obsd /bsd: umsm1 detached
Dec 13 13:40:39 obsd /bsd: umsm0 at uhub0 port 2 configuration 1 interface 0 "HUAWEI_MOBILE HUAWEI_MOBILE" rev 2.10/1.02 addr 2                                                                   
Dec 13 13:40:39 obsd /bsd: umsm0 detached
Dec 13 13:40:39 obsd /bsd: umsm0 at uhub0 port 2 configuration 1 interface 0 "HUAWEI_MOBILE HUAWEI_MOBILE" rev 2.10/1.02 addr 2                                                                   
Dec 13 13:40:39 obsd /bsd: ucom0 at umsm0
Dec 13 13:40:39 obsd /bsd: umsm1 at uhub0 port 2 configuration 1 interface 1 "HUAWEI_MOBILE HUAWEI_MOBILE" rev 2.10/1.02 addr 2
```

So will continue adjusting that file or the ones it links to and see if we get any more output...


Trying `cu` again, allowed some commands to come back from the modem, but no input was accepted:

```
obsd# cu -s 115200 -l cuaU0
Connected to /dev/cuaU0 (speed 115200)

^RSSI:20

^HCSQ:"LTE",48,35,86,14

^RSSI:1

^HCSQ:"LTE",11,0,96,0

+CGREG: 5,"792D","04947221"

+CREG: 5,"792D","04947221"

^RSSI:20

^HCSQ:"LTE",48,37,91,18

^RSSI:20

^HCSQ:"LTE",48,36,91,16

^RSSI:21

^HCSQ:"LTE",50,36,86,14

....


^HCSQ:"LTE",54,41,111,16

^RSSI:1

^HCSQ:"LTE",11,0,106,0

+CGREG: 5,"007C","00B19502"

+CREG: 5,"007C","00B19502"

^RSSI:25

^HCSQ:"LTE",58,47,116,14

+CGREG: 1,"007C","00B19502"

+CREG: 1,"007C","00B19502"

^XLEMA:1,2,112,0,1,fff

^XLEMA:2,2,911,0,1,fff

^NWTIME:18/12/13,06:20:04+28,00


^EONS:0

+CGREG: 1,"792D","04947220"

+CREG: 1,"792D","04947220"

^RSSI:20

^HCSQ:"LTE",48,39,71,22

+CGREG: 5,"792D","04947220"

+CREG: 5,"792D","04947220"

^XLEMA:1,2,112,0,1,fff

^XLEMA:2,2,911,0,1,fff

^NWTIME:18/12/13,06:20:08+28,00


^EONS:0

```

This would appear that it's getting real messages from the network.

About to reboot, then try under Ubuntu live CD. Then try to get to the bottom of the 

> ioctl(TIOCSETD): Device not configured

Looking at https://gist.github.com/artizirk/20acc2ab07fe6cad9fcc

Will install `socat` and try sending some AT commands to get a better picture

`socat - /dev/cuaU0`

```
ATI
Manufacturer: huawei

Model: E3372

Revision: 22.329.63.00.74

IMEI: 866785033700732

+GCAP: +CGSM,+DS,+ES



OK

AT + CSQ


+CSQ: 20,99
# signal strength (over 19 is excellent)



AT ^ CARDLOCK?


^CARDLOCK: 2,10,0

# shows modem is enabled

AT ^ FHVER


^FHVER:"E3372H-607 22.329.63.00.74,CL2E3372HM Ver.A"

AT ^ VERSION?


^VERSION:BDT:Apr 08 2018, 12:13:47

^VERSION:EXTS:22.329.63.00.74

^VERSION:INTS:

^VERSION:EXTD:WEBUI_17.100.20.02.74_HILINK

^VERSION:INTD:

^VERSION:EXTH:CL2E3372HM Ver.A

^VERSION:INTH:

^VERSION:EXTU:E3372

^VERSION:INTU:

^VERSION:CFG:1004

^VERSION:PRL:

^VERSION:OEM:

^VERSION:INI:



OK

```

Finally, booted up an Ubuntu live image, which did the switchmode to "HiLink", adding
a new device to ifconfig

This modified the device, as then booting back into OpenBSD, we have the cdce0 ethernet device showing. Adding a simple `/etc/hostname.cdce0` with `dhcp up` and a `sh /etc/netstart/` and we're all good.


[back](/)
