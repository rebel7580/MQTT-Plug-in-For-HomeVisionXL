**This is Beta Software. Please report issues via the homevision users group**

# Overview

The MQTT Client Plug-in provides a client interface to MQTT for HomeVision. The MQTT interface has three distinct functions:

* For "External" MQTT-enabled devices (e.g., Sonoff switches with Tasmota SW),
the MQTT Plug-in acts as an MQTT controller.
The plug-in PUBLISHES <i>command</i> topics to these devices
to control them
and SUBSCRIBES to <i>status</i> topics from these devices to track state changes.
Actions such as setting Flags or Variables and running macros can be taken based on that status.
* For "Internal" objects defined in HomeVision,
the MQTT Plug-in acts as a "proxy" for them, essentially making them appear to be MQTT-enabled.
For each selected device, the plug-in 
SUBSCRIBES to a <i>command</i> topic to it
so it can be controlled
and
PUBLISHES a <i>status</i> topic from it when it changes state.
* *Any* arbitrary generic MQTT message can be sent from your schedule via serial commands or from NetIO, independent of any configured devices.

The client plug-in is designed to work easily with [Tasmota based devices](https://github.com/arendst/Sonoff-Tasmota), using a similar basic topic and LWT structure. Other devices that follow different topic structures likely can be accommodated as well.

# Installing

The HomeVisionXL MQTT Plug-in package consists of the following files: 
* mqtt.hap - the plug-in itself, 
* mqtt.hlp - the help file,
* mqtt-1.0.tm - MQTT library (courtesy of Schelte Bron!),
* uuid-1.0.5.tm - support library.
* md5-2.0.4.tm - support library (required by uuid-1.0.5.tm).

Click the "Clone or Download" button, then select "Download ZIP".
Extract the above four files into your plugin folder.
You can delete README.md and LICENSE.

The plug-in should be enabled via the Plug-in manager.

# Change Log

The Change Log can be found in the Wiki section [Here](https://github.com/rebel7580/MQTT-Plug-in-For-HomeVisionXL/wiki/Change-Log).

# Help

Help is available through the plug-in. Also, a version of the help file can be found in this Project's [Wiki page](https://github.com/rebel7580/MQTT-Plug-in-For-HomeVisionXL/wiki/Help).
The help file is very detailed. Please read it throughly to properly set up your devices.
