# UBT Controller

This repository contains all instructions and configurations to install the
Ultimate Blind Test (UBT) software onto a Raspberry PI. The UBT controller
allows the following features :
* build players teams
* playing blind test songs
* listening to any buzzer input
* provide interface to drive the blindtest : validate/refuse answer,
  reveal, pause, next...
* count and display scores

## Hardware

The setup instructions are provided (and have been tested) for at least
Raspberry Pi 4.

## Setup instructions

The UBT controller is based on Raspberry OS. Setup requires to properly
flash a Raspberry Pi OS image. Once the Raspberry PI OS is booted and
accessible on network, the remaining steps are executed automatically by an
Ansible playbook.

### Raspberry PI OS Setup

In order to prepare your UBT controller with the appropriate firmware,
execute the following steps
* get `rpi-imager`. This is the prefered way to install Raspberry Pi OS, as
  advised by [official site](https://www.raspberrypi.com/software/)
* get a micro SD card of at least 8GB and insert it in your computer
* Launch `rpi-imager` and set the following configuration
    * Raspberry PI Device: select the hardware version you have
    * OS : choose Raspberry Pi OS (other) -> Raspberry Pi OS Lite (64 bit)
    * Storage: select the SD card you have inserted in computer
    * Click next. It will open a confirmation popup, from there do not
      click "Yes" but "Edit settings" :
      * in the General tab:
        * Set hostname : "ubt.local"
        * Set a username and password : use "ubt" as user, and choose a
          password (eg: "ubt" as well)
        * do not configure wireless : since UBT controller will manage a
          wireless access point, ethernet connection must be preferred
          during the setup
      * in the Services tab
        * enable SSH
          * select public key as preferred authentification method (so you
            won't have to type your password each time). Paste your ssh
            public key (ie: the content of your `.ssh/<key_type>.pub`
    * click "Save", and then "Yes" to actually write the SD card

When the SD card is fully written, plug it onto the Raspberry PI, provide
an internet connection to Raspberry PI through ethernet, and plug power
source.

### UBT controller configuration

You now need to execute the Ansible playbook which will install and
configure all mandatory software for UBT controller

* check that your UBT controller is reachable on network : `ping
  ubt.local`. If you can not reach your UBT controller with its mdns
  hostname but you can ping its IP address, simply update inventory.yaml
  with the IP address
* check that your UBT controller can be driven with Ansible : `ansible -i
  inventory.yaml ubt -m ping -u ubt`
* execute the playbook to configure the controller : `ansible-playbook -K
  -i inventory.yaml playbook.yaml`. You will be prompted for a password,
  enter the one you chose when you have prepared the Raspberry Pi OS image
  programming onto the SD card

### UBT controller usage

Once all configuration has been properly executed, plug a mouse and a
screen onto the UBT controller. Click on "UBT" on the desktop, it will open
the Blind Test.
