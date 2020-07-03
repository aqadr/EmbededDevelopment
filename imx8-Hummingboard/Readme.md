
### Board Layout
You can find the board layout [here](https://www.solid-run.com/nxp-i-mx8m-family/hummingboard-m/)
### Serial connection to Hummingboard-imx8 from linux host
1. Connect a USB-microUSB cable to the microUSB connector on the board.
2. Download minicom on your host Linux
3. on a terminal type the command: "minicom -b 115200 -D /dev/ttyUSB#(most of the time it will be 0 but can be 1, 2 ..)
4. Once done, shutdown minicom using command "Ctl+A , Q"

### Setting up the RJ45 Ethernet for local networking
1. Log in to the Hummingboard, userid: debian, password: debian
2. go to /etc/network/interface.d/
3. you should see a file named eth0. Create a new file with either nano or vi as enp1s0
          "sudo vi enp1s0"
4. type the following 
  "
     # eth0 automatic configuration
     allow-hotplug enp1s0

     iface enp1s0 inet static
     address 192.168.168.2
     netmask 255.255.255.0
     #iface eth0 inet6 auto
  "
