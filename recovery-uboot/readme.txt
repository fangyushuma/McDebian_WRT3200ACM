Recovering U-Boot routers WRT3200ACM and WRT32X

------------------------------------------------------------------------
uboot-wrt3200.bin	- The file U-boot is intended for the router WRT3200ACM
uboot-wrt32.bin		- The file U-boot is intended for the router WRT32X 
------------------------------------------------------------------------

1. To recovery the loader U-boot, copy the kwboot and file U-boot
to any folder in your PC running Linux.

2. Log into the PC terminal and navigate to folder files kwboot and U-boot.

3. Connect the USB-TTL adapter to the USB port on your computer and
to the router's serial port. In the terminal execute the command - 
	dmesg | grep -i ttyusb
to determine the sequence number of the initialized adapter
ttyUSB<N> (N = 0,1, ...) and set the access rights of -
	chmod 666 /dev/ttyUSB<N>

4. Set the wired LAN-port of your PC to static IP 
(for example, 192.168.1.99).

5. Connect the Ethernet cable between your computer and any LAN
router port.

6. Run TFPT-Server on the PC.

7. Execute the command -
    for WRT3200ACM
	sudo ./kwboot -f -t /dev/ttyUSB<N> -b uboot-wrt3200.bin -B 115200 -p
    for WRT32X
	sudo ./kwboot -f -t /dev/ttyUSB<N> -b uboot-wrt32.bin -B 115200 -p
and turn on the power router.
After the power is on the router and before the load starts,
it may take more than one minute.
When the download is complete, the router will be automatically
overloaded. To log on to the U-boot system, press any key on your
computer for a 3-second pause.

8. In the terminal, follow these commands:
	setenv ipaddr 192.168.1.1; setenv serverip 192.168.1.99
	nand device 0
    for WRT3200ACM
	tftp 8000000 uboot-wrt3200.bin
    for WRT32X
	tftp 8000000 uboot-wrt32.bin

	nand erase.spread 0 200000
	nand write 8000000 0 f3000
	resetenv
	reset

/ "kwboot" taken here - https://forum.armbian.com/index.php?/topic/4444-solved-kwboot-on-armada-38x/  /






