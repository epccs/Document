# Releasing COM ports in Windows

## Overview

The Windows operating system will reserve ports to COM devices that have been attached to the computer. This happens with “virtual COM port drivers” that bridge a USB bus to a serial bus (RS232/RS485). FTDI Chips and others work like this.

When a USB to UART bridge is first connected to the computer, the operating system automatically assigns a port number. When software is written to use a fixed COM port, e.g. an automation program at COM45. The problem that occurs with this automatic assignment is that Windows reservs the port for that specific chips unique serial number. When a different USB to serial bridge is connected it does not use the reserved port and takes another one.


## Solution

```
    It is possible to release Windows-reserved COM ports from previously 
    released hardware that is no longer in use.

        1) Open Command Prompt as Administrator (you may need to research how
        to do this for your version of windows).

        2) (this step is not needed on Windows 10 but is on 7) Set Environment variable 
        “set devmgr_show_nonpresent_devices=1”. At the command prompt, type 
        the following command and then press ENTER: 

            set devmgr_show_nonpresent_devices=1

        3) Run Device Manager with Environment variable. Type the following at 
        command prompt, and then press ENTER:

            start devmgmt.msc

        4) Show hidden devices. Point to View and left click, then point to 
        Show hidden devices and left click. This will display reserved ports.

        5) View hidden COM ports. Go to Ports (COM & LPT) and expand that 
        section.

        6) Remove COM1. Remove COM1 device in the list of ports, and optionally 
        others that are grayed out but have a specific COM port number 
        assigned. Right click on the grayed out device, choose Uninstall, and 
        then OK in the dialog box that appears.

        7) Close Device Manager.

        8) Exit command prompt.
```
