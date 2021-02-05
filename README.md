# Overview

This short tutorial describes a way to make a virtual machine configured for ESP32 software development with Eclipse, and explains how to use it. The virtualization environment is VirtualBox, and the guest machine runs Linux Mint 20.1.

# Prerequisites

* hardware: a 64-bit computer with enough memory so that the VM can be granted 16 GB, with a few tens of GB available on the disk, and one free USB A port
* hardware (bis): an [Espressif ESP32-DevKitC](https://www.espressif.com/en/products/devkits/esp32-devkitc/overview) with an USB A / micro USB B cable - any similar development board can be used
* developer: 
  * basic knowledge of Linux (knowing the most common commands...)
  * basic knowledge of VirtualBox (knowing how to create a virtual machine...)

# VirtualBox installation

[Download the VirtualBox binary package for your platform and install it](https://www.virtualbox.org/wiki/Downloads). Version at time of writing is 6.1.18.

Install the Extension Pack: it provides support for USB 2.0 and 3.0 devices.

# Creation of the VM

[Download Linux Mint 20.1, MATE edition](https://linuxmint.com/download.php).

[Verify the integrity of the downloaded file](https://linuxmint.com/verify.php).

Start VirtualBox, and create a new virtual machine, using the Linux Mint ISO file previously downloaded. Set memory size to 16 GB and file size to 30 GB.

Wait for Linux desktop to be displayed.

If your keyboard layout is not QWERTY, click on the main-menu icon (the small icon displaying Linux Mint logo, in the lower left-hand corner), select **Control Center**, click on **Keyboard** icon, select **Layouts** tab, add the layout for your keyboard, and remove the existing **English (US)** layout. Close the Control Center windows.

Double click on the **Install Linux Mint** icon. Following information or selections can be provided, when required for:
* language: English
* keyboard layout: the one for your keyboard
* install multimedia codecs
* erase disk and install Linux Mint
* name: Developer
* computer's name: ESP32LM
* username: developer
* password: choose one

At the end of the installation, restart. When you get the message **Please remove the installation medium, then press ENTER:**, click on Enter key.

Log in as *developer* user. Close the **Welcome to Linux Mint** window.

In the VirtualBox menu, select **Devices > Insert Guest Additions CD image...**. Double-click on the CD icon that appeared on the desktop. In the file explorer window, right-click on the **VBoxLinuxAdditions.run** file and select **Run as Administrator**. The password you are then asked for is the one you chose at installation time.

Once the guest additions are installed, you can right-click on the CD icon and select **Eject**.

Click on the system report icon, in the lower right-hand corner: ![icon](images/systemReportIcon.png). In the **System Reports** window that appears, select **System reports > Install language packs**, click on **Install the Language Packs** button, and accept the installation. The requested password is the one you chose at installation time.

For the system restore utility, click on **Ignore this report**. Close the window.

Click on the update manager icon, in the lower right-hand corner: ![icon](images/updateManagerIcon.png). In the welcome screen of the **Update Manager** window that appears, click on **OK** button. Click on the **No** button of the **Do you want to switch to a local mirror?** banner (you'll be able to choose one later on). If you are told that a new version of the update manager is available, click on **Apply the Update** button. Again, the requested password is the one you chose above (I won't say it anymore :-) ). Click on **Install Updates** button, and accept additional changes.

Reboot, if you are required to: main menu and **Quit > Restart**.

You can resize the VirtualBox window: the Linux Mint desktop will resize accordingly.

# VM configuration

## Reference documents

* [Espressif documentation](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md)

## Prerequisites

### Python

Linux Mint comes with python3. Define the **python** command so that it runs python3 by installing the **python-is-python3** package:

```shell
$ sudo apt-get install python-is-python3
```

Install a few additional python packages:

```shell
$ sudo apt-get install python3-pip python3-setuptools python3-wheel python3-virtualenv python3-venv
```

### Eclipse

[Download Eclipse CDT](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2020-12/R/eclipse-cpp-2020-12-R-linux-gtk-x86_64.tar.gz). This is version 2020-12, at time of writing. Check the integrity of the downloaded file.

Create the **~/DevTools** directory, and extract the contents of the downloaded file into it. Add an item to the main menu (right click on the main-menu icon and **Edit menu**) that runs **~/DevTools/eclipse/eclipse**.

### Git

Install git:

```shell
$ sudo apt-get install git
```

### ESP-IDF

Install ESP-IDF. Version at time of writing is 4.1.1.

```shell
$ mkdir ~/esp
$ cd ~/esp
$ git clone -b v4.1.1 --recursive https://github.com/espressif/esp-idf.git esp-idf-v4.1.1
```

### Serial link

To grant access to the virtual serial link that will be used to program the ESP32, add the user to the `dialout` group:

```shell
$ sudo adduser developer dialout
```

Close the session and reopen it.

## Eclipse IDF plugin

Start eclipse. Keep the proposed workspace. Close the **Welcome** tab and then the **Donate** tab.

[Install the IDF plugin](https://github.com/espressif/idf-eclipse-plugin#installing-idf-plugin-using-update-site-url).

Restart eclipse.

## ESP-IDF configuration

In eclipse, select **Help > Download and Configure ESP-IDF**. Check **Use an existing ESP-IDF directory from the file system**. Choose the `/home/developer/esp/esp-idf-v4.1.1` directory. Click on **Finish** button.

## Tools installation

A message box offers to download the tools. Click on the **Yes** button. In the **Install Tools** dialog box that appears, specify the git path: `/usr/bin/git`. Click on **Install Tools** button.

# ESP32-DevKitC connection

Connect the DevKitC board to a USB port of the computer. Check that the virtual machine can see it, with **Devices > USB**. A new USB device should be visible: *Silicon Labs CP2102N USB to UART Bridge Controller*. Tick the associated checkbox.

You can assign the board to the virtual machine on a permanent basis with **Devices > USB > USB Settings...**.

# Sample application

[Create a new project](https://github.com/espressif/idf-eclipse-plugin#create-a-new-project-using-esp-idf-templates), choosing the *hello_world* template.

If you check the source code of the application, you will see that eclipse displays errors for the `#include` lines. Next step will make them disappear.

[Configure a launch target](https://github.com/espressif/idf-eclipse-plugin#configuring-launch-target) for the board. Build the project, as explained [here](https://github.com/espressif/idf-eclipse-plugin#compiling-the-project).

Flash the project, as explained [here](https://github.com/espressif/idf-eclipse-plugin#flashing-the-project). If the console shows that the flashing operation does not start right after having requested it, i.e. the console waits on `Connecting........_____...`, hold down the board BOOT button until the flashing operation starts (a little bit more than 1 s).

To display trace messages printed by the application, [start a terminal](https://github.com/espressif/idf-eclipse-plugin#viewing-serial-output).