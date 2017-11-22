Ideas for next projects.

Many of them are somehow related.

Most of them will never happen :D

# Linux 

## [Greybus](https://github.com/projectara/greybus-spec/)
- Multiple transport layers (USB, I2C, SPI, Serial, TCP/IP, etc) -- [gbridge](https://github.com/anobli/gbridge)
- Portable, embedded library for creating greybus devices
  - Usable from Arduino, Fruit-Pi, etc
- Replace Expanduino

## PTY
- [Linux PTY driver doesn't allow changing number of bits and parity](https://bugzilla.kernel.org/show_bug.cgi?id=112821)
- Used to create soft serial port bridges
- Superseeded by greybus?

## LED dithering/PDM
- Not all led drivers support multiple brightness levels -- We could use dithering instead!
- ✓ Arduino: https://github.com/paulo-raca/ArduinoLedDithering
- ✓ Linux: https://github.com/paulo-raca/linux/commit/ledtrig_dither

# Experimental Design
  My HTML/CSS/JS skills were pretty lame when I wrote the current version
  - Make a pretty UI. Probably use Bootstrap and/or React?

# [PluggableUSB](https://github.com/arduino/Arduino/wiki/PluggableUSB-and-PluggableHID-howto)
- Make a version based on [Linux Gadget](https://www.kernel.org/doc/Documentation/usb/gadget_configfs.txt)
- Make a version based on [USBIP](https://lwn.net/Articles/449509/) (For ESP8266/ESP32)
  - Based on Wifi-Direct? (I wish ESP32 had support for Wifi Direct)
  - Support for Discovery? UPNP?

# HID
- Write a pretty/portable/expandable library that implements HID
  - Usable from fruit-Pi, arduinos, etc.
  - Works on USB, [I2C](http://download.microsoft.com/download/7/D/D/7DD44BB7-2A7A-4505-AC1C-7227D3D96D5B/hid-over-i2c-protocol-spec-v1-0.docx), [Bluetooth](https://www.bluetooth.org/docman/handlers/downloaddoc.ashx?doc_id=246761) [LE](https://www.bluetooth.org/docman/handlers/downloaddoc.ashx?doc_id=245141), [Greybus](https://github.com/projectara/greybus-spec/blob/master/source/device_class/hid.rst), [UHID](https://www.kernel.org/doc/Documentation/hid/uhid.txt), etc
  - TCP version of HID?
    - Based on Wifi-Direct? (I wish ESP32 had support for Wifi Direct)
    - Support for Discovery? UPNP?

# [U2F](https://fidoalliance.org/download/)
- A pretty/portable/expandable library that implements U2F
  - Usable from fruit-Pi, arduinos, etc.
  - Compatible with USB, NFC, etc.
- Create a fingerprint-enabled U2F device

# [PyPCB](https://github.com/paulo-raca/PyPcb)
- HDL for discrete components
- Based on Python
- Replaces Schematics
- Supports hierarchy 
  - Submodules can be treated as a standard component
  - Can generate symbols and footprints for complex blocks
  - A 1-element submodule can be used to easily map a "Generic Footprint" to a specific chip name and pins
- Supports single wire, named wire bundles (SCL, SDA, GND, VCC), Buses (Wire[] or Bundle[]), etc
- Works with KiCad, maybe others
- Very similar to [Skidl](https://github.com/xesscorp/skidl)

# Android Logcat
Write a CLI that replaces logcat, adding:
- Package name, Process name, user name
- Filterable by package, user name, user id, process name, PID, TID, tag, message, Regex or wildcard
- Uses colors to group messages from same user/package/process/thread
- Also backup logs on ELK stack?

# Lightsaber
- Widely based on [BUILDING THE BRIGHTEST LIGHT SABRE IN THE WORLD](http://hackaday.com/2016/10/25/building-the-brightest-light-sabre-in-the-world/)
- Hardware:
  - Brains: ESP32
  - Sensors: MPU9250 + BMP280/BME280
  - Light blade: WS2812 strip
  - Sound: [MAX98357](https://learn.adafruit.com/adafruit-max98357-i2s-class-d-mono-amp/overview)
  - Removable blade
  - 1000mAh LiPo battery
  - Big DC-DC converter with 5V output (UBEC)
  - Wishlist: Piezo sensors for detecting blade collisions
  - Wishlist: [Valve lighthouse sensor](https://github.com/ashtuchkin/vive-diy-position-sensor)
- Software:
  - Can act either as:
    - Stand-alone lightsaber, light and sound effects
    - HID input device, with:
      - accel/gyro/magnetometer/orientation/pressure sensors
      - Buttons:
        - On/Off button, with rotation encoder
        - Triggers:
          - One Analog Trigger for index finger -- Mix between like Ezra's and Xbox RT
          - One digital trigger for middle finger
        - HAT directional and/or mini joystick
        - 4 buttons in a diamond, like a standard game controller
        - Tiny LCD screen?
        
# Argus
- greybus-based IO Board
- Use OrangePi Zero as host
- New hardware variations
  - smaller, cheaper, better layout, better documented, more features, etc
  - 2* ATMEGA328PB instead of 1x ATMEGA2560?
- New firmware
  - Communication protocol based on "Modern web technologies"
    - AJAX? WebSocket? MQTT?
    - Fully documented -- Easier than writing a bunch of SDKs
  - Able to upload stand-alone modules
    - Turnstile is only a logic function that connects inputs and outputs
    - Completely stand-alone turnstile possible (including biometry!)
  - Is there any existing standard that fits? HID? Greybus?

# CRUD
- Generic tool for creating CRUDs, similar to SERFcli:
- Multiple, configurable backends
- Configurate schema, support for joins

# Scuba
- Scuba-like tool to visualize aggregated data.
- Maybe Integrated with CRUD / Kibana / Keen.io, etc

# Android Containers
- Use Android's multiuser support for implementing containers
- A special permission lets an app act as a container manager, which can:
  - Manage virtual users (Which internally are just 'regular' android users, but only visible to this app)
  - Manage apps for tne virtual users
  - Launch apps as the virtual user
  - Instrument those apps to provide hooking
  - Requires forking Android :(
- Actually, that's very similar to [Managed Profiles](https://source.android.com/devices/tech/admin/managed-profiles)
  - It should be possible to have many Managed Profiles per user (Seems to be possible, but not supported by the frontend app that handles the "CreateManagedProfile Intent")
  - It should be possible (With a explicit user permission) to inject code into the process before the app starts up
  
