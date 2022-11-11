# UBT Controller

This repository contains all instructions and configurations to install the Ultimate Blind Test (UBT) software onto a Raspberry PI. The UBT controller allows the following features :
* build players teams
* playing blind test songs
* listening to any buzzer input
* provide interface to drive the blindtest : validate/refuse answer, reveal, pause, next...
* count and display scores

## Hardware
The setup instructions are provided (and have been tested) for at least Raspberry Pi 4.

## Setup instructions

The UBT controller is based on Raspberry OS. Setup requires to properly flash a Raspberry Pi OS image. Once the Raspberry PI OS is booted and accessible on network, the remaining steps are executed automatically by an Ansible playbook.

### Raspberry PI OS Setup

In order to prepare your UBT controller with the appropriate firmware, execute the following steps
* get `rpi-imager`. This is the prefered way to install Raspberry Pi OS, as advised by [official site](https://www.raspberrypi.com/software/)
* get a micro SD card of at least 8GB and insert it in your computer
* Launch `rpi-imager` and set the following configuration
    * OS : choose Raspberry Pi OS (other) -> Raspberry Pi OS (64 bit)
    * select the SD card you have inserted in computer
    * open additionnal settings :
        * set hostname : "ubt.local"
        * enable SSH, select public key as preferred authentification method. Your public key will be automatically filled. 
        * set a username : "ubt"
        * set a password (choose a password)
        * do not configure wireless : since UBT controller will manage a wireless access point, ethernet connection must be preferred during the setup
        * Set Locale : timezone and keyboard
* start SD card write.

When the SD card is fully written, plug it onto the Raspberry PI, provide an internet connection to Raspberry PI through ethernet, and plug power source.

### UBT controller configuration

You now need to execute the Ansible playbook which will install and configure all mandatory software for UBT controller

* check that your UBT controller is reachable on network : `ping ubt.local`. If you can not reach your UBT controller with its mdns hostname but you can ping its IP address, simply update inventory.yaml with the IP address
* check that your UBT controller can be driven with Ansible : `ansible -i inventory.yaml ubt -m ping -u ubt`
* execute the playbook to configure the controller : `ansible-playbook -K -i inventory.yaml playbook.yaml`. You will be prompted for a password, enter the one you chose when you have prepared the Raspberry Pi OS image programming onto the SD card

### UBT controller usage

Once all configuration has been properly executed, plug a mouse and a screen onto the UBT controller. Click on "UBT" on the desktop, it will open the Blind Test.