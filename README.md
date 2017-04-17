# ideas
Ideas for next projects.
Many of them are somehow related :D

# Highcharts
- 3D
  - Proper support for rotating charts 360º -- Right now, the axis labels and ticks only work properly if the angle is +- 90º
  - Perspective-transform axis labels, so that it doesn't different labels don't overlap
- Color Axis
  - Find a way to bind color axis and Y axis ticks / scale
  - Automatically create gradient bands around ticks (Like I use on my samples and on Experimental Design)

# Experimental Design
  My HTML/CSS/JS skills were pretty lame when I wrote the current version
  - Make a pretty UI. Probably use Bootstrap and/or React?

# Hardware
## [Greybus](https://github.com/projectara/greybus-spec/)
- Multiple transport layers (USB, I2C, SPI, Serial, TCP/IP, etc) -- [gbridge](https://github.com/anobli/gbridge)
- Portable, embedded library for creating greybus devices
  - Usable from Arduino, Fruit-Pi, etc
- Replace Expanduino

## [PluggableUSB](https://github.com/arduino/Arduino/wiki/PluggableUSB-and-PluggableHID-howto)
- Make a version based on [Linux Gadget](https://www.kernel.org/doc/Documentation/usb/gadget_configfs.txt)
- Make a version based on [USBIP](https://lwn.net/Articles/449509/) (For ESP8266/ESP32)
  - Based on Wifi-Direct? (I wish ESP32 had support for Wifi Direct)
  - Support for Discovery? UPNP?

## HID
- Write a pretty/portable/expandable library that implements HID
  - Usable from fruit-Pi, arduinos, etc.
  - Works on USB, [I2C](http://download.microsoft.com/download/7/D/D/7DD44BB7-2A7A-4505-AC1C-7227D3D96D5B/hid-over-i2c-protocol-spec-v1-0.docx), [Bluetooth](https://www.bluetooth.org/docman/handlers/downloaddoc.ashx?doc_id=246761) [LE](https://www.bluetooth.org/docman/handlers/downloaddoc.ashx?doc_id=245141), [Greybus](https://github.com/projectara/greybus-spec/blob/master/source/device_class/hid.rst), etc
  - TCP version of HID?
    - Based on Wifi-Direct? (I wish ESP32 had support for Wifi Direct)
    - Support for Discovery? UPNP?

## [U2F](https://fidoalliance.org/download/)
- A pretty/portable/expandable library that implements U2F
  - Usable from fruit-Pi, arduinos, etc.
  - Compatible with USB, NFC, etc.
- Create a fingerprint-enabled U2F device

# Android
## Logcat
- Write a CLI that replaces logcat, adding:
  - Package name, Process name, user name
  - Filterable by package, user name, user id, process name, PID, TID, tag, message, Regex or wildcard
  - Uses colors to group messages from same user/package/process/thread
  - Also backup logs on ELK stack?

## Containers
- Use Android's multiuser support for implementing containers
- A special permission lets an app act as a container manager, which can:
  - Manage virtual users (Which internally are just 'regular' android users, but only visible to this app)
  - Manage apps for it's users
  - Launch apps as the virtual user
  - Instrument those apps to provide hooking
  - Requires forking Android :(

# CRUD
- Generic tool for creating CRUDs, similar to SERFcli:
- Multiple, configurable backends
- Configurate schema, support for joins

# Scuba
- Scuba-like tool to visualize aggregated data.
- Maybe Integrated with CRUD / Kibana / Keen.io, etc
