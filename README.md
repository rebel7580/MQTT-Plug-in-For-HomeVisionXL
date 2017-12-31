**This is Beta Software. Please report issues via the homevision users group**

# Overview

The MQTT Client Plug-in provides an client interface to MQTT for HomeVision. The MQTT interface has has three distinct functions:

* "External" non-X10 MQTT devices can be controlled and status can be received from them. Actions such as setting Flags or Variables and running macros can be taken based on that status. The plug-in PUBLISHES command topics to these devices and SUBSCRIBES to status topics from these devices.
* X-10 devices defined in HomeVision can be controlled by external entities via MQTT. The plug-in PUBLISHES status topics from these devices and SUBSCRIBES to command topics to these devices.
* *Any* arbitrary generic MQTT message can be sent from your schedule via serial commands or from NetIO, independent of any configured devices.

The client plug-in is designed to work easily with [Tasmota based devices](https://github.com/arendst/Sonoff-Tasmota), using a similar basic topic and LWT structure. Other devices that follow different topic structures likely can be accommodated as well.

# Installing

The HomeVisionXL MQTT Plug-in package consists of four files: 
* mqtt.hap - the plug-in itself, 
* mqtt.hlp - the help file,
* mqtt-1.0.tm - MQTT library (courtesy of Schelte Bron!),
* uuid-1.0.5.tm - support library.

Click the "Clone or Download" button, then select "Download ZIP".
Extract the above four files into your plugin folder.
You can delete README.md and LICENSE.

The plug-in should be enabled via the Plug-in manager.

# Help

Help is available through the plug-in. Also, a version of the help file can be found in this Project's [Wiki page](https://github.com/rebel7580/MQTT-Plug-in-For-HomeVisionXL/wiki).
The help file is very detailed. Please read it throughly to properly set up your devices.
