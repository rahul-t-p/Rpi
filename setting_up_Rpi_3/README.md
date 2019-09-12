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

#### Enable SSH
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
