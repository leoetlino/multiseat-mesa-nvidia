# Multiseat with two different GL vendors

This repo contains configuration files and scripts needed for multiseat
with two different GL vendors (mesa and nvidia in my case).

## Overview

seat0 is my PC monitor and is powered by the integrated graphics (mesa stack).

seat1 is the TV screen and is handled by the dedicated GPU (proprietary nvidia driver).

## Usage

This is not meant to be used directly. You will very probably have to slightly change
the configuration before it works for you.

## Files

* etc/lightdm/lightdm.conf: configuration for LightDM
  * use a wrapper script for the X server
  * run a custom session setup script to load the correct libraries
  * force a specific configuration for seat0 and seat1
  * enable autologin on seat1
* etc/X11/xorg.conf: Xorg configuration for seat1
  * configure the resoluton
  * provide the BusID so that Xorg can find and use the correct GPU
* etc/X11/xorg.conf.intel: Xorg configuration for seat0
  * configure the resolution
* etc/X11/Xsession.d/00ldlibrary: custom session setup script
  * set and export `LD_LIBRARY_PATH` depending on the seat, so that
    the correct gl stack gets loaded in user sessions
* usr/local/bin/X: wrapper script for X
  * set and export `LD_LIBRARY_PATH` depending on the seat, so that
    the correct gl stack gets loaded for the X server itself
  * include a retry mechanism in case the X server fails to start, which can and
    will happen when the screen associated to a seat is not powered on
    (notably for seat1 which is connected to a TV)
* usr/local/bin/get-seat-displays: script to get display numbers and the associated seats
  (optional)
* usr/local/bin/set-up-multiseat: script to attach devices to seat0/seat1 via logind


## TODO
* The library paths are currently hardcoded in two files, maybe this should be moved
  to a common file?
  * /etc/X11/Xsession.d/00ldlibrary
  * /usr/local/bin/X
* Same for the seat numbers, they're hardcoded in:
  * /etc/lightdm/lightdm.conf
  * /etc/X11/Xsession.d/00ldlibrary
  * /usr/local/bin/X
  * /usr/local/bin/get-seat-displays
  * /usr/local/bin/set-up-multiseat

## License

This is licensed under the MIT license: you can do whatever you want with it.
