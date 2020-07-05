
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
3. ip link show
4. nmcli device status
5. nmcli connection show. Detailed descriptioncan can be found [here](https://www.cyberciti.biz/faq/linux-list-network-interfaces-names-command/)

### CrossCompile for Hummingboard 
Two things are needed to cross compile a program
1. A tool-chain on the host linux machine 
2. The Sysroot- the file system of the target machine- mostly /usr and /lib directories of the target machine

For Hummingboard, the pre-built toolchain and sysroots can be found [here](https://releases.linaro.org/components/toolchain/binaries/). 

#### Cross Compile "Hello World" for Hummingboard
a. Create a directory where you want to keep the source file. 
b. Go to the directory and create the source file "HelloWorld.cpp"
c. Open "HelloWorld.cpp" with your favorite text editor and type
    
```
    #include <iostream>
    using namespace std;
    
    int main()
    {
        std::cout<<"check if cross compile works..."<<std::endl;
        return 0;
    }   
```

Once done save the file.
    
d. On the terminal type 

```${pathToToolchain}/bin/aarch64-linux-gnu-g++ -o hello helloWorld.cpp```
   
e. If you try to run this prgram on your host machine, the following error will pop up

```
bash: ./hello: cannot execute binary file: Exec format error
```
#### Cross Compile a Real Program
The previous example was a tay example that didn't have any dependencies which will not be the case for real life applications and we need to setup the cross-compile environment. 

#### Source Environment
environment variables such as ARCH/CROSS_COMPILE are needed for U-Boot and Linux makefiles to configure and call the compiler correctly. They need to be exported in a shell instance that will run configure/compile commands to build U-Boot or Linux for the target machine. 
An example script has been uploaded. run it from the terminal

```source ${PATHTO}/export_compiler```
     
#### Cross Compile with configure and make
TODO
#### Cross Compile with CMake 
Cross compile with CMake requires defining a cmake "toolchain file" that sets thing such as paths to compilers, name and so on. Then the "toolchain file" needs to be included in the cmake call
##### Steps

1. Create a toolchain.cmake file with the following items

```
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

set(CMAKE_SYSROOT $PATH_TO_SYSROOT)
set(CMAKE_STAGING_PREFIX $PATH_TO_STAGING)

set(CMAKE_C_COMPILER $PATH_TO_TOOLCHAIN_DIR/bin/arm-linux-gnueabihf-gcc(aarch64-linux-gnu-gcc))
set(CMAKE_CXX_COMPILER ${tools}/bin/arm-linux-gnueabihf-g++(aarch64-linux-gnu-g++))

set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```
2. Add the directory to the toolchain.cmake file in the CMakeLists.txt file
3. Create a build directory 
4. call cmake with the toolchain.cmake file 

```cmake -DCMAKE_TOOLCHAIN_FILE=/path/to/toolchain/$toolchainname.cmake ..```


#### Cross Compile with Docker






