[--- On Windows, use Notepad++ or Wordpad to view this file ---]

Seawolf Dev : Minimal GUI Linux for Developers
-----------------------------------------------------------------------
*** IMPORTANT ***
Kindly take a few minutes to read through this file.
-----------------------------------------------------------------------
The 'Seawolf Dev' is a custom Linux VM. Its based on the modern and
popular Ubuntu 18.04 LTS desktop image (minimal install). Besides the
"usual" utilities and GUI, we have pre-installed several packages,
utilities and apps that will be useful for Linux 'C' application and
kernel (OS)/driver/BSP developers.


===================================
1. Logging In
===================================
username: seawolf
password: welcome

NOTE-
1. By default, the 'seawolf' user is auto-logged in at boot.
2. Recommendation: change the password.

===================================
2. Shutting Down
===================================
Please do *Not* just power-off the virtual machine (just as you wouldn't just
power off a physical one). Doing this could cause data loss and/or corruption
of the disk image file. Shut down cleanly with:

sudo systemctl poweroff
or via the GUI.

===================================
3. Networking Issues
===================================
Check via the 'Network Settings...' menu if the network adapter(s) have been
setup correctly. 
Go into VirtualBox Settings for the Seawolf VM; ensure that two network
adapters are selected:
(1) Attached to ‘NAT’ 
(2) Attached to ‘Bridged Adapter’.

Check they’re Enabled. Another thing to try: shut down the guest, go to
 Network Settings / Adapter / Advanced and reinitialize the MAC Addresses
(by clicking on the ‘refresh’ gearwheel).

Also, in the guest do:
sudo dhclient                   (to [re]assign dynamic IP addresses).

========================================================
4. Updating / Upgrading the Base System
========================================================
3a. Its *important*, especially from a security viewpoint, to
be vigilant about updating (and possibly upgrading) the
base OS. You can do so with the following commands from
the shell:

sudo apt-get update
sudo apt-get upgrade

3b. Alternatively, Seawolf provides a "cleanup" script which
will do several things - cleanup, package installation,
deletion of outdated packages, and of course, updates and
upgrade path. You can manually run it like so:

cd /home/seawolf/kaiwanTECH/.seawolf_prep
sudo ./0cleanup

3c. Note that once the base Ubuntu system's kernel image (and headers)
are updated, you will need to update a GRUB config file in order to
auto-boot from the newly updated kernel.
For example, if the current kernel ver is 4.15.0-22-generic and a kernel
update to ver 4.15.0-23-generic has occured (via the 'Software Updater'
app), do this to auto-boot from 4.15.0-23-generic:

sudo cp /etc/default/grub /etc/default/grub.bkp
sudo vi /etc/default/grub
 [go to the line starting with GRUB_DEFAULT="...";
 change the text to reflect the newly updated kernel version, like so:]
GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 4.15.0-23-generic"
 save and exit
sudo update-grub
Reboot.

========================================
5. Change/Edit bootloader kernel cmdline
========================================
On the Linux console:
sudo cp /etc/default/grub /etc/default/grub.bkp
sudo vi /etc/default/grub
<Edit it; typically, you will want to edit the line>
GRUB_CMDLINE_LINUX_DEFAULT=

Save the file, and then:
sudo update-grub

Reboot to verify.

========================================================
6. Capturing Console Output via the virtual Serial Port
========================================================
Seawolf Dev is configured with console output to a virtual serial port,
such that all console output is written to the host system (to a raw file
on the host (details below); see VirtualBox's "Settings / Serial Port's").

This is very useful for app and especially kernel developers to capture
console output via (virtual) serial port.
The 'raw file' to which all console output is written can be found depending
on the host OS:
Windows:  C:\\Users\\<Username>\\console_vbox_mindev.txt
Linux:    ~/console_vbox_mindev.txt

Tip: before shutting down the VM, please take a backup copy of the file
(console_vbox_mindev.txt) as it (sometimes?) gets truncated on shutdown.

=======================================
7. Capture complete Session Transcript
=======================================
If you'd like to save a transcript of the _entire_ terminal session
(includes all the typing you do and the resulting output that occurs
in the console window), pl run:

 script <logfile.txt>

Now a transcript of the terminal window session is saved in the specified
logfile.

===================================
8. Copying between Host <-> Guest
===================================
One can easily copy content between the host and guest via 
scp (on Linux hosts) or WinSCP (On Window's hosts) in the usual manner.
(Also, filezilla on Linux is an excellent GUI alternative to WinSCP).

Lookup the Seawolf-MinDev guest's IP address with 'ifconfig' (or hostname -I).
We've delibrately configured the VirtualBox virtual networking to have both:
a) a "Bridged" adapter - so that the guest IP address in the same subnet is
applied to the virtual adapter allowing one to ping, connect over ssh, scp, etc).
b) a 'regular' "NAT" adapter for other network traffic.

VirtualBox also provides a means to share the host system's filesystem with the
guest. One can set it up via 'Settings / Shared Folders'.

================================================================
9. Advanced: Using Linux's Netconsole to capture console output
================================================================
Netconsole is a Linux kernel feature that, if enabled, enables one to
capture console output over an IP network via the UDP transport.

For your convenience, Seawolf [Min]Dev has included a few scripts to help
setup netconsole:
~/kaiwanTECH/usefulsnips/netcon_setup.sh
~/kaiwanTECH/usefulsnips/netcon_rcv_win.bat
~/kaiwanTECH/usefulsnips/netcon_rcv_linux.sh

To learn more details and how to setup netconsole on both the sender/receiver
systems, pl read:
https://www.kernel.org/doc/Documentation/networking/netconsole.txt
https://wiki.ubuntu.com/Kernel/Netconsole
https://mraw.org/blog/2010/11/08/Debugging_using_netconsole/

===================================
10. Login via SSH from Host
===================================
* This point only applies when working in non-GUI / console mode *

An FAQ: at times, the guest console window seems too small to work in 
comfortably. (This may be especially the case if the VirtualBox Extension 
Pack isn't installed. Do install it).

A quick workaround: ssh from the host into the guest; you can adjust the host
terminal emulator (or simply the terminal's) window size to your comfort!

Host is Windows : use an ssh client like 'putty'
Host is Linux   : just use ssh

===================================
11. Misc Tips
===================================

- New to the Linux command line interface (CLI)?
Please read through the doc(s) here:
 ~/kaiwanTECH/Linux_CLI_basics/

- If no GUI: how can I view PDF's in the console?
You can't :-). Pl copy them across to your host system (see Q&A above)
and peruse them there.

- When on the Seawolf-[Min]Dev console, one can use 'Alt-<left/right-arrow-key>
to cycle between the tty's !
This way you can login to multiple tty's and work concurrently; for eg.,
edit a file in one tty, while you compile and run it in another.

- If you are a participant for a kaiwanTECH training, the courseware/source
would typically be bundled (as a .tar.xz file) as part of this appliance. 
You would typically find it under the folder:
 ~/kaiwanTECH/<file>.tar.xz   (or under the home folder itself).

Unzip it easily with:
 tar xf ~/kaiwanTECH/<file>.tar.xz
===============================================================================


Ver details:
--------------------------------------
Version: SeawolfVA_1.4.3-dev
Last Updated: 02Jul2018
Type: Minimal GUI Developer Edition
Base OS: Ubuntu 18.04 LTS ('bionic beaver')
--------------------------------------
