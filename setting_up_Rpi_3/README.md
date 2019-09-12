### Configure Raspbery pi 3

This tutorial deals with configuring Raspberry pi 3 in linux environment.

#### Step 1: Writing image to the memory card
- Use high class memory card with minimum 8Gb of memory. Format the memory
  card to ext4 using softwares like *gparted* in linux.
- [here](https://www.raspberrypi.org/downloads/) is the availablle
  operating systems supported by Rpi. We can use any of these operating
  systems depending on the application. In this tutorial I will be
  configuring [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) os. Download the image from [here](https://www.raspberrypi.org/downloads/raspbian/).
- Connect memory card to your computer/laptop, open up a terminal and type the command. Note the name of the card (usually sdb/sdc) and verify by looking at the size colum corresponding to that name.

 ```
$lsblk
```

- Run the following command in terminal. Replace paths accordinly.

```
$sudo dd bs=1M if=/path/to/your/raspbian/img of=/dev/your_card_name_without_any_number status=progress conv=fsync
```

#### Step 2: Enable SSH
- Mount the sd card and you will see 2 partitions.
- make a blank file and rename it as *ssh* (without any extension) in the *boot* partition.
- Copy & paste the following lines and replace contents according to your wifi.
```
country=fr
update_config=1
ctrl_interface=/var/run/wpa_supplicant GROUP=netdev
network={
 scan_ssid=1
 ssid="RouterName"
 psk="Security"
 key_mgmt=WPA-PSK
}
```
#### Step 3: Set up a static ip

- In order to know the ip that the Raspberry Pi will take, we will give it a static ip. For this we will modify the file ``dhcpd.conf`` located in the ``/etc/folder``. Go to the last line and add the following contend.

```
eth0 
static ip_address = 192.168.1.100 / 24 
static routers = 192.168.1.1 
static domain_name_servers = 192.168.1.1 

wlan0 interface 
static ip_address = 192.168.1.100 / 24 
static routers = 192.168.1.1
static domain_name_servers = 192.168.1.1
```
- Here interface eth0 corresponds to a connection of wired type and interface wlan0 to a Wi-fi connection . So you have to choose the one that corresponds to your setup.
-`` Static ip_address`` is used to indicate the ip that your Raspberry Pi will have once started. Generally the ip is of type 192.168.1.x , replace the x with the value of your choice (x \ne 1), be careful not to conflict with other devices.

#### Step 4: Insert the memory card in pi then power up the device.
- Make sure your pc/laptop and Rpi are connected to the same network.
#### Step 5: Finding ip address

- If you did'nt set up static ip, there is a way we can find the ip of Rpi.
- Open terminal and type

```
$ ip add
```
- note the ``wlp3s0`` ip.
```
$ nmap -sn ip_addr_noted/24
```
- Install nmap if you dont have.
- It may take some time. But it will display all the divices connected to the network including Rpi.

#### Step 6: ssh to Rpi
- Open terminal and type

```
ssh pi@ip_addr_of_Rpi
```
- Default 
    - username : pi
    - password : raspberry

- Grant permission (yes)

Now the initial set up is over...

#### Additional configurations
- **Expand filesystem.**
    - ssh into Rpi
    ```
    $ raspi-config
    ```
    - Go to ``Advanced options`` -> ``Expand filesystem``
    - Then reboot.
- **Change username and password.**
- **[Install and enable vnc](https://www.raspberrypi.org/documentation/remote-access/vnc/). Remember to use vnc viewer your pc/laptop should have real vnc installed.**
- **Configure screen size in vnc viewer.**

#### References:
1. [https://howtoraspberrypi.com/create-sd-card-raspberry-pi-command-line-linux/](https://howtoraspberrypi.com/create-sd-card-raspberry-pi-command-line-linux/)
2. [https://howtoraspberrypi.com/how-to-raspberry-pi-headless-setup/](https://howtoraspberrypi.com/how-to-raspberry-pi-headless-setup/)
3. [https://core-electronics.com.au/tutorials/raspberry-pi-zerow-headless-wifi-setup.html](https://core-electronics.com.au/tutorials/raspberry-pi-zerow-headless-wifi-setup.html)
4. [https://www.raspberrypi.org/learning/networking-lessons/rpi-static-ip-address/](https://www.raspberrypi.org/learning/networking-lessons/rpi-static-ip-address/)

