# ZyXEL NBG-419N - OpenWrt installation problems

## steps and problems

- The router runs the stock firmware fine but I needed the tagged VLANs option so I check in the net and found out that OpenWrt supports this.
- I tried to follow the guide here: https://openwrt.org/toh/zyxel/nbg419n.
- For the flash, I used the Serial interface inside the router. I didn't have a USB/Serial converter so I used one of my ESP8622 with a simple code that simply diactivate TX and RX and runs an infinite loop. This will bridge the Serial interface of the ESP to the mini USB port.
- I connected everything together as shown in the link above and checked the devices. The router was detected at `/dev/ttyUSB0`. To get access to the Serial Console I use: `screen /dev/ttyUSB0 57600 cs8 -parenb -cstopb`.
- The PC should be connected to the router with LAN and runs a TFTP server to serve the flash later, I used atftpd.
- I downloaded OpenWrt 19.07.7 and flashed it
- Once done, I experimented with the interface and got no WLAN interface.
- I tried different versions of OpenWrt that can be found here https://downloads.openwrt.org/releases/
- I tried 18.06.9, 17.01.7 and 19.07.9, they all have the WLAN interface however:
- 19.07.9 had some weird interface problems: bad request, loading, no CSS etc.
- 18.06.9 resets the settings after each reboot even with the squashfs version.
- Currently I am using 17.01.7 which seems to work the best and has the same features as the newer versions. I might check with the other 19.07.x versions.

 ## other issues
 - I bricked the router multiple times, example:
 - The PC doesn't get an IP address thus cannot run TFTP to upload the flash. Solution: configure a static IP address
 - For flashing, I sometimes used the TFTP method (selecting 2 at the boot menu through the Serial interface) or if I have ssh access, I used SCP to upload the flash and later use `mtd -r write <firmware.bin> firmware`, this worked better then the first method but only if you get the ssh access (which you typically get after the first OpenWRt flash).

## remarks
- You should first start by flashing the kernel version of the firmware and later the squashfs version
- If the router is really bricked and nothing works, try flashing the stock firmware again, this happened to me once and fixed the infinit boot loop problem.

 More to follow as I'll try to upgrade to 19.x later.
