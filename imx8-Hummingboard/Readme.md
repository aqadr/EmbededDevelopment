
### HummingBoard Pulse Layout
You can find the board layout [here](https://www.solid-run.com/nxp-i-mx8m-family/hummingboard-m/)
### Serial connection to Hummingboard-imx8 from linux host
The easiest way is to use minicom for serial communication.

1. Download minicom on your host Linux
2. Connect a USB-microUSB cable to the microUSB port on the board.
3. on a terminal type the command: \
        ```minicom -b 115200 -D /dev/ttyUSB#``` (most of the time it will be 0 but can be 1, 2 ..)

4. Once done, shutdown minicom using command 
         ```Ctl+A and then Q```

### Setting up the RJ45 Ethernet for local networking
1. Log in to the Hummingboard, userid: debian, password: debian
2. go to /etc/network/interface.d/
3. you should see a file named eth0. Create a new file with either nano or vi as enp1s0
          ```sudo vi enp1s0```
4. type the following lines to the file enp1s0
  
  ```
     # eth0 automatic configuration 
     allow-hotplug enp1s0 

     iface enp1s0 inet static 
     address 192.168.168.2 
     netmask 255.255.255.0 
     #iface eth0 inet6 auto
  ```
  5. restart networking 
      ```  sudo systemctl restart networking```
      
  6. get the device up
     ``` sudo ifup enp1s0```
#### Useful networking commands for debian image
1. networkctl — Query the status of network links. details can be found [here](https://www.freedesktop.org/software/systemd/man/networkctl.html) and [here](https://www.tecmint.com/networkctl-check-linux-network-interface-status/)
2. ip addr

### CrossCompile for Hummingboard 
