#### Table of contents

* [Overview](#overview)
* [Prerequisites](#prerequisites)
* [Creation of the VM](#creationOfTheVm)
* [VM configuration](#vmConfiguration)
  * [Reference documents](#referenceDocuments)
  * [Prerequisites](#prerequisites)
    * [Python](#python)
    * [Eclipse](#eclipse)
    * [Git](#git)
  * [Eclipse IDF plugin](#eclipseIdfPlugin)
  * [ESP-IDF installation](#espIdfInstallation)
  * [Tools installation](#toolsInstallation)
* [ESP32-DevKitC connection](#esp32devkitcConnection)
* [Sample application](#sampleApplication)
* [Upgrade](#upgrade)

<a name="overview"></a>
# Overview

This short tutorial describes a way to make a virtual machine configured for ESP32 software development with Eclipse, and explains how to use it. The virtualization environment is VirtualBox, and the guest machine runs Linux Mint 20.1.

<a name="prerequisites"></a>
# Prerequisites

* hardware: a 64-bit computer with enough memory so that the VM can be granted 16 GB, with a few tens of GB available on the disk, and one free USB A port
* hardware (bis): an [Espressif ESP32-DevKitC](https://www.espressif.com/en/products/devkits/esp32-devkitc/overview) with an USB A / micro USB B cable - any similar development board can be used
* developer: 
  * basic knowledge of Linux (knowing the most common commands...)
  * basic knowledge of VirtualBox (knowing how to create a virtual machine...)

<a name="creationOfTheVm"></a>
# Creation of the VM

Check [this guide](https://github.com/PascalBod/lm20.1-vm).

<a name="vmConfiguration"></a>
# VM configuration

<a name="referenceDocuments"></a>
## Reference documents

* [Espressif documentation](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md)

<a name="prerequisite"></a>
## Prerequisites

<a name="python"></a>
### Python

Linux Mint 20.1 comes with python3. Define the **python** command so that it runs python3 by installing the **python-is-python3** package:

```shell
$ sudo apt-get install python-is-python3
```

Install a few additional python packages and ccache:

```shell
$ sudo apt-get install python3-pip python3-setuptools python3-wheel python3-virtualenv python3-venv python3-serial ccache
```

<a name="eclipse"></a>
### Eclipse

[Download Eclipse Installer 2021‑03 R](https://www.eclipse.org/downloads/packages/release/2021-03/r) (x86_64). **Important**: do not use Eclipse 2021-06, as IDF plugin is [currently incompatible with this version](https://github.com/espressif/idf-eclipse-plugin/issues/299).

Check the integrity of the downloaded file.

Create the **~/DevTools** directory, and extract the contents of the downloaded file into it.

Run `~/DevTools/eclipse-installer/eclipse-inst`. Choose **Eclipse IDE for C/C++ Developers**. Keep the default path values.

<a name="git"></a>
### Git

Install git:

```shell
$ sudo apt-get install git
```

<a name="eclipseIdfPlugin"></a>
## Eclipse IDF plugin

Start Eclipse, if not yet done from the installer. Keep the proposed workspace. Close the **Welcome** tab and then the **Donate** tab.

[Install the IDF plugin](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#installing-idf-plugin-using-update-site-url). At time of writing, this is version 2.1.0.

Restart Eclipse.

<a name="espIdfInstallation"></a>
## ESP-IDF installation

[Install ESP-IDF from Eclipse](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#installing-esp-idf). Choose `v4.3` for the version, and select the `DevTools` directory for download directory.

<a name="toolsInstallation"></a>
## Tools installation

A message box offers to download the tools. Click on the **Yes** button. In the **Install Tools** dialog box that appears, specify the git path: `/usr/bin/git`. Click on **Install Tools** button.

<a name="esp32devkitcConnection"></a>
# ESP32-DevKitC connection

Connect the DevKitC board to a USB port of the computer. Check that the virtual machine can see it, with **Devices > USB**. A new USB device should be visible: *Silicon Labs CP2102N USB to UART Bridge Controller*. Tick the associated checkbox.

You can assign the board to the virtual machine on a permanent basis with **Devices > USB > USB Settings...**.

<a name="sampleApplication"></a>
# Sample application

[Create a new project](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#create-a-new-project), choosing the *hello_world* template.

If you check the source code of the application, you will see that eclipse displays errors for the `#include` lines. Next step will make them disappear.

[Configure a launch target](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#configuring-launch-target) for the board. Build the project, as explained [here](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#compiling-the-project).

Flash the project, as explained [here](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#flashing-the-project). If the console shows that the flashing operation does not start right after having requested it, i.e. the console waits on `Connecting........_____...`, hold down the board BOOT button until the flashing operation starts (a little bit more than 1 s). A way to permanently fix this problem can be to modify the virtual machine settings, as follows:
* shutdown the virtual machine
* start VirtualBox, and select **Settings** for the virtual machine
* in the **USB** category, select **USB 3.0 (xHCI) Controller** 
* click on the **OK** button to save

To display trace messages printed by the application, [start a terminal](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#viewing-serial-output).

<a name="upgrade"></a>
# Upgrade

To upgrade ESP-IDF, select **Help > Download and Configure ESP-IDF** and select the ESP-IDF version to download. Once downloaded, the ESP-IDF plugin might ask you to install a new set of tools. Accept, provide the path to git as above and install the tools. 

To upgrade the Eclipse IDF plugin, check [this](https://github.com/espressif/idf-eclipse-plugin/blob/master/README.md#how-do-i-upgrade-my-existing-idf-eclipse-plugin).
