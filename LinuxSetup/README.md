# Linux Setup

## Overview

These are some of my notes for Ubuntu.


## SSH

Needs to be installed on Ubuntu so...

```
sudo apt-get install openssh-server

# put some ssh keys on the machine, it can make login much less painfull.
# I have a bash scrip to do some of the grunt work
[mkdir bin]
cd bin
wget https://raw.githubusercontent.com/epccs/RPUpi/master/Hardware/Testing/mkeys
chmod u+x mkeys
# note if you have a private key (e.g. id_dsa or id_rsa file) 
# and you want to use it then place it in the .ssh folder now
~/bin/mkeys localhost
# that should have built the public (and if missing a new private) key 
# and added the public key to the authorized file 
# now try to log in, and it should not ask for a password since it used keys
ssh localhost
# if that works one of the putty tools can convert the private key for use on Windows.
# mkeys can also place the public key on the authorized file of other Linux machines. Both   
# Ubuntu 16.04 LTS and 17.10 have zeroconf that seems to work (try with a ping first).
~/bin/mkeys leek.local
ssh leek.local
```

## Samba

Samba is for Windows file sharing.

```
sudo apt-get update
sudo apt-get install samba samba-common-bin
sudo smbpasswd -a rsutherland

#Folder to share
mkdir /home/rsutherland/Samba

sudo nano /etc/samba/smb.conf
```

Add the share to the /etc/samba/smb.conf file.

```
# add this to the very end of the file
[Samba]
path = /home/rsutherland/Samba
valid users = rsutherland
read only = no
```

Restart Samba and check for errors.

```
sudo service smbd restart

#Check for errors
testparm

# my user name (rsutherland) on Windows can now map 
# to the share on the Ubuntu computer (named conversion)
\\conversion\Samba
```

## Other stuff I use

```
sudo apt-get update
sudo apt-get install make git picocom gcc-avr binutils-avr gdb-avr avr-libc avrdude
sudo usermod -a -G dialout rsutherland
```

I use picocom as command line interface to the serial port (sort of like the Arduino IDE's serial monitor). It can be run from an SSH session act as a sort of terminal server over a secure connection. The user needs to be added to the dialout group.

The other stuff is for working with AVR microcontrollers.