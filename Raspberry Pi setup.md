# Raspberry pi setup
A setup of raspberry pi using arm64 image of debianOS lite.

## Flash raspberry image
- Download raspbian os lite 64 bit [here](https://downloads.raspberrypi.org/raspios_lite_arm64/images/raspios_lite_arm64-2020-08-24/)
- disk util to find the sd card disk number
```bash
diskutil list
```
- unmount
```bash
diskutil unmountDisk /dev/diskN
```

- copy the image
```bash
sudo dd bs=1m if=path_of_your_image.img of=/dev/rdiskN; sync
```

- enable ssh
```bash
touch /Volumes/boot/ssh
```
- eject
```bash
sudo diskutil eject /dev/rdiskN
```

## Set up raspberry hostname and ssh
- Find raspberrypi ip
```
 nmap -sn 192.168.0.1/24
```

### Setup SSH keys
- Check existing keys
```bash
ls ~/.ssh
```

If not existing create a new key
```bash
ssh-keygen
```

look at the key 
```bash
cat ~/.ssh/id_rsa.pub
```

copy your ssh key to the raspberry. Default username and password are pi:raspberry
```bash
ssh-copy-id <USERNAME>@<IP-ADDRESS>
```

### Change hostname
SSH into the pi and change the hostname in the file
```
sudo nano /etc/hostname
```

restart the pis

---
#raspberrypi 
 [raspberrypi.org](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md) 