<b>You must have a HomeVision or HomeVision Pro Controller running HomeVisionXL software for this to work.</b>

**Please report issues via the homevision users group**

# Overview

The MQTT Client Plug-in provides a client interface to MQTT for HomeVision.
Its main purposes are to control MQTT enabled devices via the HomeVision Schedule or NetIO and
to control HomeVision objects by MQTT sources.
The MQTT interface has three distinct functions:
* For "external" MQTT-enabled devices (e.g., Sonoff switches with Tasmota SW),
the MQTT Plug-in acts as an MQTT controller.
The plug-in PUBLISHES <i>command</i> topics to these devices
to control them
and SUBSCRIBES to <i>status</i> topics from these devices to track their state changes.
Changes are tracked by Actions such as setting Flags or Variables and running Macros based on that status.

* For "internal" objects defined in HomeVision
(such as X-10 modules, flags, inputs, etc.),
the MQTT Plug-in acts as a "proxy" for them, essentially making them appear to be MQTT-enabled.
For each selected object, the plug-in 
SUBSCRIBES to a <i>command</i> topic
so it can be controlled
and
PUBLISHES a <i>status</i> topic when it changes state.

* Generic MQTT topics can be sent from the HomeVision schedule via serial commands, from NetIO, or from custom plug-ins, independent of any configured devices.

While the client plug-in is designed to work easily with [Tasmota based devices](https://github.com/arendst/Sonoff-Tasmota), using a similar topic and LWT structure,
other devices that follow different topic structures likely can be accommodated as well.
There is a lot of flexibility in the plug-in allowing support for many different situations.

# Installing

The HomeVisionXL MQTT Plug-in package consists of the following files: 
* mqtt.hap - the plug-in itself, 
* mqtt.hlp - the help file,
* mqtt-2.0.tm - MQTT library (courtesy of Schelte Bron!),
* uuid-1.0.5.tm - support library.
* md5-2.0.4.tm - support library (required by uuid-1.0.5.tm).
* mqtt_evlog.hap, netio_evlog.hap - helper plug-ins for HomeVision Log - optional.

Click the "Code" button then select "Download ZIP".
Extract the above files into your plugin folder.

mqtt_evlog.hap, netio_evlog.hap are only needed if you want MQTT access to the HomeVision Event Log.

**Caution!! netio_evlog.hap supplied here is a slight upgrade to the one supplied with version 3.2 of the NetIO plug-in.
This version is compatible with version 3.2 of the NetIO plug-in and will overwrite any existing NetIO verson of this file.
Please be aware:**
* If you are accessing the HomeVision Event Log via NetIO but not via MQTT, DO NOT delete netio_evlog.hap! You'll need it for NetIO. mqtt_evlog.hap CAN be deleted.
* If you are accessing the HomeVision Event Log via MQTT, <i>subsequently</i> downloading the 3.2 version of the NetIO plug-in will revert netio_evlog.hap back to the original NetIO 3.2 version which is not compatible with MQTT. Either save and restore the MQTT version of netio_evlog.hap., or re-download MQTT completely.

You can delete README.md and LICENSE.

The plug-in should be enabled via the Plug-in manager.

# Change Log

The Change Log can be found in the Wiki section [Here](https://github.com/rebel7580/MQTT-Plug-in-For-HomeVisionXL/wiki/Change-Log).

# Help

While Help is available through the plug-in, the most up-to-date version of the help file can be found in this Project's [Documentation pages](https://rebel7580.github.io/MQTT/MQTT_index).
The help file is very detailed. Please read it throughly to properly set up your devices.
