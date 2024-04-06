## Codesys Control Runtime on Raspberry Pi
This project supports the installation of the Codesys Control Runtime on a Raspberry Pi to enable the development and running of Industrial applications according to the IEC 61131-3 standard. Codesys already has a dedicated Runtime package for Raspberry pi called the ```CODESYS Control for Raspberry Pi SL``` but there is no free or trial version unlike the rest of the Runtime packages, meaning that you would have to buy it before using it.

However, there is also a another Linux based Runtime package called ```CODESYS Control for Linux SL``` that can be installed and used on a Raspberry Pi which does not require a licence to develop applications (Unlicensed version allows you to test your application for two hours). This however does not come with the features that are contained in the dedicated package for Raspberry PI. Such features include ready to use devices for different buses and IO devices such SPI, I2C and many others 

So, this project focuses on the installation and usage of the ```CODESYS Control for Linux SL```. The instrutions in this project cover the manual installation method, meaning that files are transfered and installed manuaaly on the Raspberry Pi. We will also attempted to write our own device description files for deifferent IO devices

## Installation process

### Download the Codesys Control Runtime
Download the Control Runtime Package file form here: [https://store.codesys.com/en/codesys-control-for-linux-arm-sl-1.html](https://store.codesys.com/en/codesys-control-for-linux-arm-sl-1.html)


### Install the Runtime package files on your local machine
Use the Codesys installer to install the package file on your local computer. (Watch video below for help)

### Transfer Installation file to your Rapsberry pi
Locate the Folders whee the pacake files have been installed and transfer the following files to your Raspberry Pi. The ```x.x.x.x``` represent the Runtime version and will depend on the version you have downloaded. 

* The ```codesyscontrol_linuxarm_4.1.0.0_armhf.deb``` located in the \\\<USER>\CODESYS Control for Linux ARM SL\Delivery folder
* The ```codemeter-lite_7.20.4402.501_armhf.deb``` located in the \\\<USER>\CODESYS Control for Linux ARM SL\Dependency folder
*  The ```codesysedge_edgearmhf_4.11.0.0_armhf.deb``` located in the \\\<USER>\CODESYS Edge Gateway for Linux\Delivery\edgearmhf folder

### Install the Codesys Control Runtime
From your Raspberry, follow the following steps

* Unpack the Debian package by running the following command:  ```$ ar -x codesyscontrol_linuxarm_4.1.0.0_armhf.deb```. This will create three files namely:
    1. ```control.tar.gz```
    2. ```data.tar.gz```
    3. ```debian-binary```

    We are only interested in the ```data.tar.gz```

* Unpack the data.tar.gz file. Create a new folder for this beforehand with the following commands:
    - $ ```mkdir data```
    - $ ```tar -xf data.tar.gz -C data```
    
    In the data folder, you should have three new folders inside namely, ```ect```, ```opt```, ```usr``` and ```var```

* Copy the individual directory contents to the respective root directories of the Rapsberry Pi:
    
    Navigate to the ```data/``` folder and run the following commands
    1. ```$ sudo cp -r etc/* /etc```
    2. ```$ sudo cp -r opt/* /opt```
    3. ```$ sudo cp -r usr/* /usr```
    4. ```$ sudo cp -r var/* /var```

* Change config file previllages and add the ```codesysuser``` group to the system

    1. ```$ sudo chmod a+rw /etc/CODESYSControl.cfg```
    2. ```$ sudo chmod a+rw /etc/CODESYSControl_User.cfg```
    3. ```$ groupadd codesysuser```


### Install the Codesys Codemeter

* Unpack the Debian Codemeter package by running the following command:  ```$ codemeter-lite_7.20.4402.501_armhf.deb```. This will create three files namely:
    1. ```control.tar.gz```
    2. ```data.tar.gz```
    3. ```debian-binary```

    We are only interested in the ```data.tar.gz```

* Unpack the data.tar.gz file. Create a new folder for this beforehand with the following commands:
    - $ ```mkdir data```
    - $ ```tar -xf data.tar.gz -C data```
    
    In the data folder, you should have three new folders inside namely, ```ect```, ```lib```, ```usr``` and ```var```

* Copy the individual directory contents to the respective root directories of the Rapsberry Pi:
    
    Navigate to the ```data/``` folder and run the following commands
    1. ```$ sudo cp -r etc/* /etc```
    2. ```$ sudo cp -r lib/* /lib```
    3. ```$ sudo cp -r usr/* /usr```
    4. ```$ sudo cp -r var/* /var```

* Run the following post installation commands
    1. ```$ sudo udevadm trigger -vn --subsystem-match=usb --attr-match=idVendor=064f | xargs -rn1 -d\\n udevadm trigger -b```
    2. ```$ sudo mkdir -p "/etc/systemd/system/multi-user.target.wants/"```
    3. ```$ sudo ln -sT /lib/systemd/system/codemeter.service /etc/systemd/system/multi-user.target.wants/codemeter.service```


### Install the Codesys Edge Gateway

* Unpack the Debian edge gateway package by running the following command:  ```$ ar -x codesysedge_edgearmhf_4.11.0.0_armhf.deb```. This will create three files namely:
    1. ```control.tar.gz```
    2. ```data.tar.gz```
    3. ```debian-binary```

    We are only interested in the ```data.tar.gz```

* Unpack the data.tar.gz file. Create a new folder for this beforehand with the following commands:
    - $ ```mkdir data```
    - $ ```tar -xf data.tar.gz -C data```
    
    In the data folder, you should have three new folders inside namely, ```ect```, ```opt```, ```usr``` and ```var```

* Copy the individual directory contents to the respective root directories of the Rapsberry Pi:
    
    Navigate to the ```data/``` folder and run the following commands
    1. ```$ sudo cp -r etc/* /etc```
    2. ```$ sudo cp -r opt/* /opt```
    3. ```$ sudo cp -r usr/* /usr```
    4. ```$ sudo cp -r var/* /var```

* Change previllages for the gateway config file```
    1. ```$ chmod a+rw /etc/Gateway.cfg```


### Start Codemeter, Codesys Runtime and Edge gateway

1. Codemeter: ```$ sudo /usr/sbin/CodeMeterLin```
2. Runtime: ```$ sudo /etc/init.d/codesyscontrol start```
3. Edge gateway: ```$ sudo /etc/init.d/codesysedge start```


### Watch the Video Tutorial for more instructions


[![IMAGE ALT TEXT HERE](https://i9.ytimg.com/vi_webp/8GcxFJbOXW8/mqdefault.webp?v=66114a1f&sqp=CIz4xbAG&rs=AOn4CLAalqIqECOyQJayZhuyXFcm6Pvvkw)](https://www.youtube.com/watch?v=8GcxFJbOXW8)
