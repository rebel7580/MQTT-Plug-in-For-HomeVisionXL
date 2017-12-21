

See this Project's [Wiki page](https://github.com/rebel7580/ESP8266-Wemo-and-HomeVisionXL-Plug-in/wiki/ESP8266-Wemo-and-HomeVisionXL-Plug-in) for more details.

# Overview

The MQTT Client Plug-in provides an client interface to MQTT for HomeVision. The MQTT interface has two distinct areas:

* "External" non-X10 MQTT devices can be controlled and status can be received from them. Actions such as setting Flags or Variables and running macros can be taken based on that status. The plug-in PUBLISHES command topics to these devices and SUBSCRIBES to status topics from these devices.
* X-10 devices defined in HomeVision can be controlled by external entities via MQTT. The plug-in PUBLISHES status topics from these devices and SUBSCRIBES to command topics to these devices.

The client plug-in is designed to work easily with Tasmota based devices, using a similar basic topic and LWT structure. Other devices that follow different topic structures likely can be accommodated as well.

# Supported MQTT Topics

## Standard and Custom Topics
Topics can be assigned to devices either as abbreviated ("Standard") topics or full topics. Standard topics follow the Tasmota structure, so only the unique sub-topic portion need be entered. Standard topics are indicated by enclosing the sub-topic with "<" and ">". If a topic starts with a "<", the appropriate prefix is automatically added. If a topic ends with a ">", the appropriate postfix is automatically added.
In fact, if a topic has only a "<" OR a ">", then only the appropriate prefix OR postfix is automatically added. Otherwise, the topic is used as-is without any additional portions prepended/appended to it. Thus by not putting "<" and ">" around your topic, you can use any topic structure you want.

## Standard Command Topics
The following command topics are supported:


         Full Topic     Payload
     cmnd/{topic}/POWER  ON
     cmnd/{topic}/POWER  OFF
     cmnd/{topic}/POWER  TOGGLE
     cmnd/{topic}/POWER  {empty}

{topic} is defined during device configuration.
The plug-in **publishes** commands to external devices. Usually, after receiving one of these commands, an external device will publish its state. The plug-in receives this report by subscribing to the state topic. (See next.) An empty payload requests the device to publish its state without changing the state of the device.
HomeVision X-10 devices **subscribe** to commands so that external entities can control them. Currently, only ON, OFF and TOGGLE are supported.

## Standard State Topic
For each external device, the plug-in **subscribes** to a state topic as follows:


         Full Topic
     stat/{topic}/POWER

If received, the plug-in looks for a message of "on" or "off" and takes action according to the device's configuration. <br /><br /> When an X-10 device changes state, the plug-in will *publish* a state command to indicate the device's new state.
## Last Will and Testament Topic
For each external device *with a standard topic*, the plug-in *subscribes* to a *Last Will and Testament* topic as follows:


         Full Topic
     tele/{topic}/LWT

If received, the plug-in looks for a message of "online" or "offline". <br /><br /> LWT is not supported for X-10 devices.

# Configuring Devices

## Ext Devices Tab
This tab contains a list of supported external devices.
* *Device Name* is the name of the device for use by serial and NetIO commands. It must be unique among both external and X-10 device names. It can be the same as the topic (but cannot contain "<", ">", "/" or spaces).
* *Topic* is the topic used for publishing and subscribing. See below for Topic rules.
* The *Flag/Var* column shows the Flag (f#) or Variable (v#) assigned to the device. If none is assigned, a "-" will show.
* The *Macro* column shows the Macro(s) assigned to the device in the form {on macro}#/{off macro}#. If no macro is assigned, a "-" will show.
* The *State* column will show "On" or "Off", depending on the reported state of the device. If the payload was an number, its value, truncated to an integer and masked by 0xFF, will be displayed. If a variable is assigned, then this is the value that will be written to the variable. When the state has not (yet) been reported, a "-" will show.
* For each device, its row will be displayed in RED text if the device is reported as "off line" by an LWT message. However, if an "on Line" LWT or a POWER state messages is received, the row will display in BLACK text.
* Rows can be sorted in ascending or descending order by *Device Name* or *Topic* by clicking on the column header.

### New/Edit/Delete External Devices

* To enter a new device, click the "New" button, and start with the *Topic*.
  * A "Standard" topic, for which substitution is performed, is indicated by enclosing it in "<" and ">". E.g., "<{topic}>". When such a device is commanded to turn on, it will be "published" by:
     cmnd/{topic}/POWER  ON
  * Topics can contain multiple levels, indicated by "/", but should not start or end with a "/".
  * Standard topics should not contain a "#" wildcard, since it must be the last character in a full topic, which isn't possible if substitution is done. The "#" wildcard is allowed if the topic does not end with a ">".
  * Standard and custom topics can contain "+" wildcards, once per level. Examples:
    * "+"      - Matches all topics!
    * "+/a"    - Matches all two level topics with "a" as the second level.
    * "a/+"    - Matches all two level topics with "a" as the first level.
    * "+/a/+"  - Matches all three level topics with "a" as the middle level.
  * *Device Names*, not topics, are used by NetIO and serial commands to publish commands to MQTT devices. If the "topic" is simple and descriptive, it can be copied to the *Name* field. If the "topic" is multi-level, *Name* must be simpler.
  * There is some validation of the *Topic* and *Device Name* fields to enforce the above rules and to avoid duplication, but it may not be perfect.
  * For each device, an HV Flag or Variable can be assigned. Assigning a Flag or Variable is optional. There is no checking to make sure a Flag or Variable is not used more than once.
  * Macros can be assigned for "On" and "Off" states.
  * Devices can be edited by clicking the "Edit" button. This brings up the same window used for adding a new device.
* Use the "Delete" button to delete a device. When a device is deleted, the Plug-in un-subscribes to any topics related to that device.
* When "Done" is clicked, devices with standard topics are subscribed to their state and LWT full topics. Devices with custom topics are subscribed only to the topic as-is.
## Int (X-10) Device Tab

This tab contains a list of supported X-10 devices.

* *ID* is the X-10 device ID.
* *Device Name* is the name of the device for use by serial and NetIO commands. It must be unique. It can be the same as the topic (but cannot contain "<", ">", "/" or spaces). Alphanumeric and the underscore are the only allowed characters.
* *Topic* is the topic used for publishing and subscribing. Topic rules are the same as External devices.
* The *State* column will show "On" or "Off", depending on the reported state of the device. The auto reporting feature for X-10 devices must be turned on, or you schedule must explicitly send X-10 updates for this to work. When the state has not (yet) been reported, a "-" will show.
* *Level* shows the reported X-10 device level.
* Rows can be sorted in ascending or descending order by *ID*, *Device Name* or *Topic* by clicking on the column header.

### New/Edit/Delete X-10 Devices

* To enter a new device, Click the "New" button and start by selecting the X-10 device.
* Clicking "Copy X-10 to Topic, Name" will populate the Topic and Name fields with an appropriate version of the X-10 name. You can modify these fields to whatever you want, keeping in mind the rules above for topics and device names.
* Devices can be edited by clicking the "Edit" button. This brings up the same window used for adding a new device.
* Use the "Delete" button to delete a device. When a device is deleted, the Plug-in un-subscribes to any topics related to that device.
* When "Done" is clicked, devices with standard topics are subscribed to their command full topics. Devices with custom topics are subscribed only to the topic as-is.

## Settings Tab

* Prefixes and Postfixes can be set for "standard" topics. The defaults conform to the Tasmota structure. However, they can be changed if necessary.
* "MQTT Broker web/IP Address" defaults to "localhost" which should work if your MQTT broker is on the same computer as HomeVisionXL. If it doesn't work, or the broker and HomeVisionXL are on different computers, change it to the MQTT broker's domain or explicit IP address.
* "MQTT Broker Port" defaults to "1883" and probably won't need to change.
* If a username and password is used to log into the broker, check the "Use Username/Password" box and then fill in the username and password.
* "Netio string", "Serial string prefix string", and "Serial string terminator character(s)" are set to reasonable defaults and probably don't need to be changed.

# Responding to External Device State Changes

## Flags and Variables
If a Flag is assigned to a device, it will be SET when the device reports an "On" state and CLEARed when the device reports an "Off" state.
If a Variable is assigned to a device, it will be set to "1" when the device reports an "On" state and set to "0" when the device reports an "Off" state.
If the payload contains an integer or floating-point number (instead of "On" or "Off"), the value is truncated to an integer and is masked with 0x01 for Flags (so even numbers result in "0" or CLEAR and odd numbers result in "1" or SET) or 0xFF for Variables, making sure the result is within the Variable range of 0-255. Note this Variable masking is essentially a modulo-256 operation; it does not truncate numbers greater than 255 to 255. E.g., a received value of 256 will be converted to 0.

## Macros

Macros can be assigned to the "On" and "Off" states. The same macro can be assigned to both states, or different macros can be assigned. You can also assign a macro to just one of the states, leaving the other set to "None".
Note that an assigned Flag or Variable is always updated before the macro is run.

# Controlling Devices

## Device Display Area
Right-clicking on a Device line in the Device Display Area will bring up a menu from which you can select "On", "Off", "Toggle" or "State". When selected, the corresponding command topic is published. (See **Command Topics** above). This presumably will make its way through the MQTT broker to the external device which would then act on the command.
When selecting a right-click command on an X-10 device, the corresponding command topic is published in the same manner as external devices. No action is taken directly on the X-10 device. However, since the X-10 device has *subscribed* to the command topic, the MQTT broker will send it back to the Plug-in, which will then set the X-10 device to the requested state. If the command topic causes a X-10 state change, then the Plug-in completes the sequence by publishing a state topic showing the new state. Here's a sequence chart example that might make it more clear:


     HomeVision              Plug-in                   MQTT Broker

                      Select device "Den"
                       and click "Toggle"

                      cmnd/den/POWER TOGGLE --->
                                            <--- cmnd/den/POWER TOGGLE
                  <--- HV cmd to toggle Den
    sends x-10 cmd
     to toggle Den

    Auto reports
     X-10 state     --->
                        stat/den/POWER ON     --->


## Serial Control

MQTT devices can be controlled within a schedule via serial commands and take the form:

     mqtt: {device name} {command};

For example, to toggle a device *named* "sonoff1", a serial command would be set to:

     mqtt: sonoff1 toggle;

"mqtt:" is whatever the "Serial string prefix" is defined as. ":' is whatever the "Serial string terminator character(s)" is defined as. Besides "toggle" or "2", "on" or "1", "off" or "0", and "state" are allowed.
The device *name* is case-insensitive when matching a device **name** in the Device list. The *command* portion is converted to lower case before being sent.

Note: Device *names* are limited to alphas, numbers and the underscore.

Note: While serial commands from the schedule work for X-10 devices as well as external devices, it makes more sense to just set the X-10 device directly in the schedule. In either case, the Plug-in will publish a state change topic if the state actually changed.

##  NetIO

 MQTT devices can be controlled via NetIO using a "netioaction" command in the NetIO app. For example, to have a button set up to toggle a device *named* "sonoff1", the button's *sends* attribute would be set to:


     sends:  netioaction mqtt sonoff1 toggle

"mqtt" is whatever the "Netio string" is defined as. Besides "toggle" or "2", "on" or "1", "off" or "0", and "state" are allowed.

The device *name* is case-insensitive when matching a device **name** in the Device list. The *command* portion is converted to lower case before being sent.

The state of the device can get retrieved by:


     reads:  get mqtt sonoff1

The *get* command will return either "On" or "Off", depending on the state of the device. <br /><br /> Note: This is a "convenience" command, since it actually just reads the state of the Flag or Variable associated with an external device, or the state of an X-10 device. For external devices, if no flag or variable are associated, an empty string is returned. If the device's state has not yet been reported, the last value of the Flag or Variable will be returned. <br /><br /> Also note that this command is essentially the same as the more direct gets:

     reads:  get var state var#

or

     reads:  get flag state flag#

or

     reads:  get x10 state id#

except that no NetIO Custom Returns processing is done. So the direct object gets are probably better to use then the MQTT versions.
