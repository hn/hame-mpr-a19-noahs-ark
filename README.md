
# hame-mpr-a19-noahs-ark

Technical data (reverse engineered) for the HAME MPR-A19 "Noah's Ark" Router (WiFi + 3G with builtin SIM slot + NFC + Ethernet + 5200mAh battery. Also sold with an Aukey PB-W3 label.

## Manufacturer

[Manufacturer product website](http://hame.hamedata.com/html/product/view2-28-106.html) (for our convenience, their marketing department also provides an [explosion view of the device](http://hame.hamedata.com/uploadfiles/webeditor/63-1.jpg)).

## Hardware

Main ICs are the Ralink RT5350F (SoC, CPU+WiFi+Ethernet+USB) and the Spreadtrum SC7702A (3G modem, HSPA+/UMTS 2100MHz/EDGE/GPRS/GSM 850/900/1800 /1900MHz, 21Mbps Download; 5.76Mbps upload).

## Firmware

Firmware update can be downloaded [here](http://www.aukey.com/public/download/Aukey%20PB-W3%2020140526.rar) and seems to be similiar to all the other HAME devices:

```
$ md5sum AukeyPB_2.4.105.232_20141223102419.bin 
2320d7c8abb853d203bab647af39f6c8  AukeyPB_2.4.105.232_20141223102419.bin
$ mkimage -l AukeyPB_2.4.105.232_20141223102419.bin 
Image Name:   Linux Kernel Image
Created:      Tue Dec 23 03:25:08 2014
Image Type:   MIPS Linux Kernel Image (lzma compressed)
Data Size:    3566386 Bytes = 3482.80 kB = 3.40 MB
Load Address: 80000000
Entry Point:  80295000
```

The v20140526 firmware image contains a [compressed](http://sourceforge.net/p/squashfs/patches/_discuss/thread/fa70e074/d998/attachment/0001-unsquashfs-add-support-for-LZMA-magics.patch) squashfs file system at position 945010 containing all the usual system files (/etc, /sbin, ...).

The device loads a firmware file (file name = serial of device = 9c41...) via TFTP from IP 192.168.1.55 when you press the reset switch during boot.

## Serial Port

The serial port (57600 8n1) is located on the back of the PCB, next to the ethernet port. The three soldering joints are labeled as follows: GND (next to the edge of the PCB), RX (middle), TX.

Boot log:

```
U-Boot 1.1.8 (Feb  6 2012 - 16:07:57)

Board: Ralink APSoC DRAM:  16 MB
relocate_code Pointer at: 80fb0000
spi_wait_nsec: 42 
spi device id: ef 40 16 0 0 (40160000)
find flash: W25Q32BV
raspi_read: from:30000 len:1000 
.*** Warning - bad CRC, using default environment

============================================ 
Ralink UBoot Version: 3.6.0.0
-------------------------------------------- 
ASIC 5350_MP (Port5<->None)
DRAM_CONF_FROM: Boot-Strapping 
DRAM_TYPE: SDRAM 
DRAM_SIZE: 128 Mbits
DRAM_WIDTH: 16 bits
DRAM_TOTAL_WIDTH: 16 bits
TOTAL_MEMORY_SIZE: 16 MBytes
Flash component: SPI Flash
Date:Feb  6 2012  Time:16:07:57
============================================ 
icache: sets:256, ways:4, linesz:32 ,total:32768
dcache: sets:128, ways:4, linesz:32 ,total:16384 

 ##### The CPU freq = 360 MHZ #### 
 estimate memory size =16 Mbytes
raspi_read: from:40028 len:6 
.
raspi_read: from:0 len:30004 
....[ff ff]
Init Plat A18
*************Is_update = 0 plat = 1**************
No lcd plat
Disable usb
Usb ok

Please choose the operation: 
   1: Load system code to SDRAM via TFTP. 
   2: Load system code then write to Flash via TFTP. 
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial. 
   9: Load Boot Loader code then write to Flash via TFTP. 
 0 
   
3: System Boot system code via Flash.
## Booting image at bc050000 ...
raspi_read: from:50000 len:40 
.   Image Name:   Linux Kernel Image
   Created:      2014-12-23   2:25:08 UTC
   Image Type:   MIPS Linux Kernel Image (lzma compressed)
   Data Size:    3566386 Bytes =  3.4 MB
   Load Address: 80000000
   Entry Point:  80295000
raspi_read: from:50040 len:366b32 
.......................................................   Verifying Checksum ... OK
   Uncompressing Kernel Image ... OK
No initrd
## Transferring control to Linux (at address 80295000) ...
## Giving linux memsize in MB, 16

Starting kernel ...


LINUX started...

 THIS IS ASIC
Linux version 2.6.21 (root@localhost) (gcc version 3.4.2) #204 Wed Mar 19 03:46:11 EDT 2014

 The CPU frequency set to 360 MHz
CPU revision is: 0001964c
Determined physical RAM map:
 memory: 01000000 @ 00000000 (usable)
Built 1 zonelists.  Total pages: 4064
Kernel command line: console=ttyS1,57600n8 root=/dev/mtdblock5
Primary instruction cache 32kB, physically tagged, 4-way, linesize 32 bytes.
Primary data cache 16kB, 4-way, linesize 32 bytes.
Synthesized TLB refill handler (20 instructions).
Synthesized TLB load handler fastpath (32 instructions).
Synthesized TLB store handler fastpath (32 instructions).
Synthesized TLB modify handler fastpath (31 instructions).
Cache parity protection disabled
cause = 60808050, status = 11000000
PID hash table entries: 64 (order: 6, 256 bytes)
calculating r4koff... 0015f900(1440000)
CPU frequency 360.00 MHz
Using 180.000 MHz high precision timer.
Dentry cache hash table entries: 2048 (order: 1, 8192 bytes)
Inode-cache hash table entries: 1024 (order: 0, 4096 bytes)
Memory: 13392k/16384k available (2105k kernel code, 2992k reserved, 535k data, 112k init, 0k highmem)
Mount-cache hash table entries: 512
NET: Registered protocol family 16
usbcore: registered new interface driver usbfs
usbcore: registered new interface driver hub
usbcore: registered new device driver usb
Time: MIPS clocksource has been installed.
NET: Registered protocol family 2
IP route cache hash table entries: 1024 (order: 0, 4096 bytes)
TCP established hash table entries: 512 (order: 0, 4096 bytes)
TCP bind hash table entries: 512 (order: -1, 2048 bytes)
TCP: Hash tables configured (established 512 bind 512)
TCP reno registered
deice id : ef 40 16 0 0 (40160000)
W25Q32BV(ef 40160000) (4096 Kbytes)
mtd .name = raspi, .size = 0x00400000 (4M) .erasesize = 0x00010000 (64K) .numeraseregions = 0
Creating 7 MTD partitions on "raspi":
0x00000000-0x00400000 : "ALL"
0x00000000-0x00030000 : "Bootloader"
0x00030000-0x00040000 : "Config"
0x00040000-0x00050000 : "Factory"
0x00050000-0x00136b72 : "Kernel"
mtd: partition "Kernel" doesn't end on an erase block -- force read-only
0x00136b72-0x01000000 : "RootFS"
mtd: partition "RootFS" extends beyond the end of device "raspi" -- size truncated to 0x2c948e
mtd: partition "RootFS" doesn't start on an erase block boundary -- force read-only
0x00050000-0x01000000 : "Kernel_RootFS"
mtd: partition "Kernel_RootFS" extends beyond the end of device "raspi" -- size truncated to 0x3b0000
RT3xxx EHCI/OHCI init.
squashfs: version 3.2-r2 (2007/01/15) Phillip Lougher
squashfs: LZMA suppport for slax.org by jro
NTFS driver 2.1.28 [Flags: R/O].
fuse init (API version 7.8)
io scheduler noop registered (default)
Hame plat 18
Ralink gpio driver initialized [003c7f01]
HDLC line discipline: version $Revision: 1.1.1.1 $, maxframe=4096
N_HDLC line discipline registered.
Serial: 8250/16550 driver $Revision: 1.8 $ 2 ports, IRQ sharing disabled
serial8250: ttyS0 at I/O 0xb0000500 (irq = 37) is a 16550A
serial8250: ttyS1 at I/O 0xb0000c00 (irq = 12) is a 16550A
loop: loaded (max 8 devices)
rdm_major = 253
Ralink APSoC Ethernet Driver Initilization. v2.1  256 rx/tx descriptors allocated, mtu = 1500!
MAC_ADRH -- : 0x00009c41
MAC_ADRL -- : 0x7c76d6a0
PROC INIT OK!
PPP generic driver version 2.4.2
PPP BSD Compression module registered
NET: Registered protocol family 24
PPPoL2TP kernel driver, V0.17
PPTP driver version 0.8.1
block2mtd: version $Revision: 1.1.1.1 $
usbcore: registered new interface driver cdc_acm
drivers/usb/class/cdc-acm.c: v0.25:USB Abstract Control Model driver for USB modems and ISDN adapters
usbcore: registered new interface driver usbhid
drivers/usb/input/hid-core.c: v2.6:USB HID core driver
nf_conntrack version 0.5.0 (128 buckets, 1024 max)
ip_tables: (C) 2000-2006 Netfilter Core Team, Type=Restricted Cone
TCP cubic registered
NET: Registered protocol family 1
NET: Registered protocol family 17
802.1Q VLAN Support v1.8 Ben Greear <greearb@candelatech.com>
All bugs added by David S. Miller <davem@redhat.com>
VFS: Mounted root (squashfs filesystem) readonly.
Freeing unused kernel memory: 112k freed
init started: BusyBox v1.12.1 (2013-11-04 22:59:55 EST)
starting pid 14, tty '': '/etc_ro/rcS'
Algorithmics/MIPS FPU Emulator v1.5
devpts: called with bogus options
-------------------------------------------
Welcome to
    __  __     __     __    __  ______
    | | | |   /  \   |  \  /  | | ____|
    | |_| |  / /\ \  | |\\//| | | |___
    |  _  | |_/__\ \ | | \/ | | | |___|
        | | | | | |  | | | |    | | | |___
        |_| |_| |_|  |_| |_|    |_| |_____|
    HameData Technology Co., Ltd
-------------------------------------------
killall: goahead: no process killed
starting pid 34, tty '/dev/ttyS1': '/bin/sh'


BusyBox v1.12.1 (2013-11-04 22:59:55 EST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

# ******g_iplat = 1000*******
Start gpio 0 monitor
killall: udhcpc: no process killed
ifconfig: ioctl 0x8913 failed: No such device
internet.sh
Password for 'hame' changed
/sbin/internet.sh: line 272: can't create /proc/sys/kernel/hotplug: nonexistent directory
rt3xxx-ehci rt3xxx-ehci: Ralink EHCI Host Controller
rt3xxx-ehci rt3xxx-ehci: new USB bus registered, assigned bus number 1
rt3xxx-ehci rt3xxx-ehci: irq 18, io mem 0x101c0000
rt3xxx-ehci rt3xxx-ehci: USB 0.0 started, EHCI 1.00, driver 10 Dec 2004
usb usb1: configuration #1 chosen from 1 choice
hub 1-0:1.0: USB hub found
hub 1-0:1.0: 1 port detected
rt3xxx-ohci rt3xxx-ohci: RT3xxx OHCI Controller
rt3xxx-ohci rt3xxx-ohci: new USB bus registered, assigned bus number 2
rt3xxx-ohci rt3xxx-ohci: irq 18, io mem 0x101c1000
usb usb2: configuration #1 chosen from 1 choice
hub 2-0:1.0: USB hub found
hub 2-0:1.0: 1 port detected
rmmod: ralink_wdt: No such file or directory
rmmod: cls: No such file or directory
rmmod: hw_nat: No such file or directory
rmmod: raeth: No such file or directory
insmod: bridge.ko: module not found
insmod: mii.ko: module not found
insmod: raeth.ko: module not found

phy_tx_ring = 0x009a7000, tx_ring = 0xa09a7000

phy_rx_ring0 = 0x009a8000, rx_ring0 = 0xa09a8000
MAC_ADRH -- : 0x00009c41
MAC_ADRL -- : 0x7c76d6a0
RT305x_ESW: Link Status Changed

##### disable 1st wireless interface #####
rmmod: rt2860v2_ap_net: No such file or directory
rmmod: rt2860v2_ap: No such file or directory
rmmod: rt2860v2_ap_util: No such file or directory
rmmod: rt2860v2_sta_net: No such file or directory
rmmod: rt2860v2_sta: No such file or directory
rmmod: rt2860v2_sta_util: No such file or directory
insmod: rt2860v2_ap_util.ko: module not found
RT305x_ESW: Link Status Changed
send signal 1
usb 1-1: new high speed USB device using rt3xxx-ehci and address 2
Input usb [1782/0003]
-----not Find 3g.pid---36-------
usb 1-1: configuration #1 chosen from 2 choices
rt2860v2_ap: module license 'unspecified' taints kernel.


=== pAd = c0061000, size = 802328 ===

<-- RTMPAllocAdapterBlock, Status=0
insmod: rt2860v2_ap_net.ko: module not found
rmmod: nf_nat_pptp: No such file or directory
rmmod: nf_conntrack_pptp: No such file or directory
rmmod: nf_nat_proto_gre: No such file or directory
rmmod: nf_conntrack_proto_gre: No such file or directory
RX DESC a043d000  size = 2048
<-- RTMPAllocTxRxRingMemory, Status=0
Key1Str is Invalid key length(0) or Type(0)
Key2Str is Invalid key length(0) or Type(0)
Key3Str is Invalid key length(0) or Type(0)
Key4Str is Invalid key length(0) or Type(0)

1. Phy Mode = 9
2. Phy Mode = 9
3. Phy Mode = 9
pAd->TxPowerCtrl.bInternalTxALC == FALSE !
MCS Set = ff 00 00 00 01
<==== rt28xx_init, Status=0
0x1300 = 00064380
vconfig: ioctl error for rem: Invalid argument
vconfig: ioctl error for rem: Invalid argument
rmmod: 8021q: No such file or directory
insmod: 8021q.ko: module not found
device eth2 entered promiscuous mode
VLAN (eth2.2):  Setting underlying device (eth2) to promiscious mode.
ifconfig: ioctl 0x8913 failed: No such device
brctl: bridge br0: No such device or address
iptables v1.4.0rc1: can't initialize iptables table `mangle': Table does not exist (do you need to insmod?)
Perhaps iptables or your kernel needs to be upgraded.
##### config Ralink ESW vlan partition (LLLLW) #####
switch reg write offset=14, value=405555
switch reg write offset=50, value=2001
switch reg write offset=98, value=7f3f
switch reg write offset=e4, value=3f
switch reg write offset=40, value=1001
switch reg write offset=44, value=1001
switch reg write offset=48, value=1002
switch reg write offset=70, value=ffff506f
done.
device ra0 entered promiscuous mode
eth2.1: dev_set_promiscuity(master, 1)
device eth2.1 entered promiscuous mode
##### start wan #####
cat: can't open '/var/run/3g.pid': No such file or directory
kill: you need to specify whom to kill
##### start lan #####
br0: port 2(eth2.1) entering learning state
br0: port 1(ra0) entering learning state
ifconfig: ioctl 0x8914 failed: Cannot assign requested address
ifconfig: ioctl 0x8914 failed: Cannot assign requested address
br0: topology change detected, propagating
br0: port 2(eth2.1) entering forwarding state
br0: topology change detected, propagating
br0: port 1(ra0) entering forwarding state
killall: udhcpd: no process killed
Set: phy[0].reg[RT305x_ESW: Link Status Changed
send signal 0
0] = 3900
Set: phy[1].reg[0] = 3900
Set: phy[2].reg[0] = 3900
Set: phy[3].reg[0] = 3900
Set: phy[4].reg[0] = 3900
Set: phy[0].reg[0] = 3100
ifconfig: ioctl 0x8913 failed: No such device
/sbin/lan.sh: line 154: can't create /proc/sys/net/ipv6/conf/all/forwarding: nonexistent directory
##### start nat #####
RT305x_ESW: Link Status Changed
send signal 1
=============>lan ip [192.168.169.1]
daemon->ip_to_lan = [0]
##### finish nat #####
killall rt2860apd 1>/dev/null 2>&1
iptables -F -t filter 1>/dev/null 2>&1
iptables -D FORWARD -j macipport_filter 1>/dev/null 2>&1
iptables -F macipport_filter 1>/dev/null 2>&1
iptables -D FORWARD -j web_filter  1>/dev/null 2>&1
iptables -F web_filter  1>/dev/null 2>&1
iptables -D FORWARD -j malicious_filter 1>/dev/null 2>&1
iptables -F malicious_filter  1>/dev/null 2>&1
iptables -D INPUT -j malicious_input_filter 1>/dev/null 2>&1
iptables -F malicious_input_filter  1>/dev/null 2>&1
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -t filter -N web_filter 1>/dev/null 2>&1
iptables -t filter -N macipport_filter 1>/dev/null 2>&1
iptables -t filter -N malicious_filter 1>/dev/null 2>&1
iptables -t filter -N synflood_filter 1>/dev/null 2>&1
iptables -t filter -N malicious_input_filter 1>/dev/null 2>&1
iptables -t filter -N synflood_input_filter 1>/dev/null 2>&1
iptables -t filter -A FORWARD -j web_filter 1>/dev/null 2>&1
iptables -t filter -A FORWARD -j macipport_filter 1>/dev/null 2>&1
iptables -t filter -A FORWARD -j malicious_filter 1>/dev/null 2>&1
iptables -t filter -A malicious_filter -p tcp --syn -j synflood_filter 1>/dev/null 2>&1
iptables -t filter -A INPUT -j malicious_input_filter 1>/dev/null 2>&1
iptables -t filter -A malicious_input_filter -p tcp --syn -j synflood_input_filter 1>/dev/null 2>&1
iptables -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu 1>/dev/null 2>&1
iptables -A malicious_input_filter -i ppp0 -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A malicious_input_filter -i ppp0 -m state -p tcp --dport 80 --state NEW,INVALID -j DROP
/bin/super_dmz -f
sh: /bin/super_dmz: not found
iptables -t nat -F PREROUTING
iptables -t nat -D PREROUTING -j port_forward 1>/dev/null 2>&1
iptables -t nat -F port_forward  1>/dev/null 2>&1; iptables -t nat -X port_forward  1>/dev/null 2>&1
iptables -t nat -D PREROUTING -j DMZ 1>/dev/null 2>&1
iptables -t nat -F DMZ 1>/dev/null 2>&1; iptables -t nat -X DMZ  1>/dev/null 2>&1
iptables -t nat -F POSTROUTING 1>/dev/null 2>&1
iptables -t nat -N port_forward 1>/dev/null 2>&1; iptables -t nat -I PREROUTING 1 -j port_forward 1>/dev/null 2>&1
iptables -t nat -N DMZ 1>/dev/null 2>&1; iptables -t nat -I PREROUTING 2 -j DMZ 1>/dev/null 2>&1
iptables -t nat -A POSTROUTING -o ppp0 -s 192.168.169.1/255.255.255.0 -j MASQUERADE 1>/dev/null 2>&1
ntp.sh
ddns.sh
kill -9 `cat /var/run/wscd.pid.ra0`
pool.ntp.org: Unknown host
cat: can't open '/var/run/wscd.pid.ra0': No such file or directory
kill: you need to specify whom to kill
iwpriv ra0 set WscConfMode=0 1>/dev/null 2>&1
route delete 239.255.255.250 1>/dev/null 2>&1
killall -q klogd
killall -q syslogd
syslogd -C8 1>/dev/null 2>&1
klogd 1>/dev/null 2>&1
killall -q zebra
killall -q ripd
webs: Listening for HTTP requests at address 192.168.169.1
1
firstFlash = 0
wan.sh: unknown wan connection type: SMART



connect_all


switch reg write offset=14, value=405555
switch reg write offset=50, value=2001
switch reg write offset=98, value=7f3f
switch reg write offset=e4, value=3f
switch reg write offset=40, value=1002
switch reg write offset=44, value=1001
switch reg write offset=48, value=1001
switch reg write offset=70, value=ffff417e
done.
[0]Conn_buf=
Current Plat 1000
tmk 9 set askdial=0
tmk 4 set connstatus = 0
tmk 7 set askdial=1
It's T286  system0
interface[1782:0003] = 1
It's T286  system1
It's T286  system2
It's T286  system3
tmk 10 set askdial=1
It's T286  system1 cd-rom
It's T286  system2 cd-rom
sh: back3g.sh: not found
It's T286 modem dev_umts
tmk 3 set connstatus = 0
tmk 11 set askdial=1
3G3G


usbcore: registered new interface driver usbserial
drivers/usb/serial/usb-serial.c: USB Serial support registered for generic
usbserial_generic 1-1:2.0: generic converter detected
usb 1-1: generic converter now attached to ttyUSB0
usbserial_generic 1-1:2.1: generic converter detected
usb 1-1: generic converter now attached to ttyUSB1
usbserial_generic 1-1:2.2: generic converter detected
usb 1-1: generic converter now attached to ttyUSB2
usbserial_generic 1-1:2.3: generic converter detected
usb 1-1: generic converter now attached to ttyUSB3
usbserial_generic 1-1:2.4: generic converter detected
usb 1-1: generic converter now attached to ttyUSB4
usbcore: registered new interface driver usbserial_generic
drivers/usb/serial/usb-serial.c: USB Serial Driver core
```

## Pictures

![Box](https://github.com/hn/hame-mpr-a19-noahs-ark/blob/master/hame-mpr-a19-noahs-ark-box.jpg "Box")
![PCB bottom](https://github.com/hn/hame-mpr-a19-noahs-ark/blob/master/hame-mpr-a19-noahs-ark-circuit-bottom.jpg "PCB bottom")
![PCB](https://github.com/hn/hame-mpr-a19-noahs-ark/blob/master/hame-mpr-a19-noahs-ark-circuit.jpg "PCB")
![Cover](https://github.com/hn/hame-mpr-a19-noahs-ark/blob/master/hame-mpr-a19-noahs-ark-cover.jpg "Cover")
![Label](https://github.com/hn/hame-mpr-a19-noahs-ark/blob/master/hame-mpr-a19-noahs-ark-label.jpg "Label")
![Case](https://github.com/hn/hame-mpr-a19-noahs-ark/blob/master/hame-mpr-a19-noahs-ark-case.jpg "Case")

## Other resources

[OpenWrt Forum Discussion](https://forum.openwrt.org/viewtopic.php?id=61191)

