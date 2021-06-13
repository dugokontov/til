## Setting raspberry-pi as a printing server

Raspberry pi can be configured to act as local network printing server. For that, two things are required:

 * Installing printer on raspberry pi
 * Setting samba

### Installing printer on raspberry pi
Source for steps described bellow can be [here](https://www.brainbytez.nl/applications/raspberry-pi/install-hp-laserjet-1018-on-raspbian/) and [here](https://web.archive.org/web/20150916044833/http://andrum99.blogspot.co.uk/2013/06/getting-hp-laserjet-1018-printer.html)
1. Install pre-requisite packages:
```sh
sudo apt-get update
sudo apt-get install tix groff cups
```
2. Add user pi to lpadmin group:
```sh
sudo usermod -a -G lpadmin pi
```
3. Download and build foo2zjs:
```sh
wget -O foo2zjs.tar.gz http://foo2zjs.rkkda.com/foo2zjs.tar.gz
tar zxf foo2zjs.tar.gz
cd foo2zjs
make
```

> **[Alternative](https://askubuntu.com/questions/1345155/how-to-install-foo2zjs-tar-gz-when-website-doesnt-work/1345564#1345564)**: If there is an error with downloading `.tar` file (eg. http://foo2zjs.rkkda.com is down), then download source code from [github](https://github.com/koenkooi/foo2zjs) and run `make` on that code. Note that in this case you don't need to download image for the printer (`./getweb 1018`) as it is already present in repository. So, just skip step 4 in that case.

```sh
git clone https://github.com/koenkooi/foo2zjs.git
cd foo2zjs
make
```

4. Get the printer firmware:
```sh
./getweb 1018
```
5. Install foo2zjs and tool to download firmware to printer. Also, restart CUPS:
```sh
sudo make install install-hotplug cups
```

If you get error message `Error: system-config-printer-udev is installed`, remove it with the suggested command (eg. `sudo apt-get remove system-config-printer-udev`).

6. Plug in the HP Laserjet 1018 printer, and switch it on.
Orange light should flash and the printer motor will run for a few seconds, indicating firmware has been downloaded successfully.
Verify it has with:
```sh
usb_printerid /dev/usb/lp0
```
The output of this command should include FWVER:20051028 or similar.
If no FWVER shown, firmware download has not worked.

7. Manually create cups printer (do not use the web interface):
```sh
sudo lpadmin -p hp1018 -v "usb://HP/LaserJet%201018" -E -P /usr/share/cups/model/HP-LaserJet_1018.ppd.gz
```

8. Set system default printer to the one we just created:
```sh
sudo lpadmin -d "hp1018"
```
9. Set default page size to A4:
```sh
sudo lpoptions -o media=A4
```

10. Test if printer is installed correctly by using either `lpr` or `lp`, depending on which one was installed with cups
```sh
lp text.txt
```
### Configure samba
1. Install samba package:
```sh
sudo apt-get install samba
```
If asked, select that you don't want to use DHCP to get info for SAMBA.

2. Enable printer sharing
```sh
sudo vi /etc/samba/smb.conf
```
Edit section `[printers]`
```
guest ok = yes
```
Restart raspberry.

3. Install printer as network printer from Linux or Windows.
