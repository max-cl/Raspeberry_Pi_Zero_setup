# Setup Raspberry Pi Zero (Raspbian OS) with an static IP #

1.- Install Raspian OS in MicroSD with "balenaEtcher" tool 

- You can dowload "balenaEtchet" from this link https://www.balena.io/etcher/

Run balenaEtcher
```sh
$ ./balenaEtcher-1.5.53-x64.AppImage
```
When it already started select the "Raspbian OS Image", the Device (microSD card) that you want to install the OS and after choose the option "Flash!".

###### NOTE: The device will be formated, before to install the OS.
\
2.- After to install the OS, you have to go to the partition called "BOOT" into the microSD card and create an empty file called "SSH", after that you will be able to connect by SSH. Use the next command to create the file:

```sh
$ touch /media/USER/boot/SSH
```

###### NOTE: Replace "USER" word for the your user.
\
3.- Go into the partition called "ROOTFS" and edit the file in the path "/media/USER/rootfs/etc/dhcpcd.conf" and uncomment (remove #) the static IP:

```sh
$ nano /media/USER/rootfs/etc/dhcpcd.conf 
```

\# Example static IP configuration: (by default is 192.168.0.10, but you can put what you want 192.168.x.x)
interface wlan0
static ip_address=192.168.0.10/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8 8.8.4.4

4.-Setup WIFI connection, go into the next path "/media/USER/rootfs/etc/wpa_supplicant/wpa_supplicant.conf", using the next command :
```sh
$ nano /media/USER/rootfs/etc/wpa_supplicant/wpa_supplicant.conf
```
Copy and paste the next configuration, and change the values of the variables according your WIFI:
```sh
# wpa_supplicant conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
ap_scan=1
update_config=1
country=DK
 
network={
     ssid="YOUR_SSID"
     bssid=YOUR_BSSID
     psk="YOUR_PASSWORD"
     key_mgmt=WPA-PSK
     scan_ssid=1
}
```

###### NOTE: The BSSID is without quotes ("")
\
5.- Insert the MicroSD card into the Raspberry, and connect the device to the electricity.

6.- Connect via SSH to the Raspberry (192.168.0.X):
```sh
$ ssh pi@192.168.0.X
```
###### NOTE: The default password is "raspberry"
\
7.- Set the TimeZone
```sh
$ sudo ln -fs /usr/share/zoneinfo/Europe/Copenhagen /etc/localtime
$ sudo dpkg-reconfigure -f noninteractive tzdata
$ date (check the date)
```

8.- Change the password:
Run the command "passwd", and after you willsee the next: (fill the fields)

```sh
$ passwd
(current) UNIX password: raspberry
Enter new UNIX password: "YOUR_PASSWORD"
Retype new UNIX password: "YOUR_PASSWORD"
```

9.- Update and upgrade:

```sh
$ sudo apt-get update && sudo apt-get upgrade
```
