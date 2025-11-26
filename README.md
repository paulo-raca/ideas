Ideas for next projects.

Many of them are somehow related.

Most of them will never happen :D

# py-key-value-aio,

- Should support Google Datastore (Cheap)
- Should provide a Dataloader-like Wrapper for batching operations
- Not sure why it was designed with values being `dict[str, Any]` -- Most providers add a JSON serialization layer anyway
- Add HashKeyWrapper
- Preferences object, preferrably based on dataclass/Pydantic
- Should provide a get_or_compute_and_set wrapper, usable with an @cached decorator

# IOT

## Ceiling lights

- Big led light for the ceiling
  - Adjustable brightness and temperature -- Maybe RGB.
- Battery backup, so it can act as an emergency light
- Integrates with SmartHome over Matter
- Also supports a plain-old switch, just in case
- While you have one big thing in the ceilling, also add:
  - Temperature sensor
  - Light sensor
  - Presence sensor (PIR?)
  - IR Remote control (So it can control TV, AC, etc)

## Wall switches
- Integrates with SmartHome over Matter
- Supports existing physical switches
  - On/Off
  - Toggle
  - Rotary
  - Capacitive
- (Optional) Built-in switch for lamps, power sockets.


# Biometrics

## Crypto Fingerprint Scanner

Current scanners just dump a bitmap. There is no cryptographic guarantee for the image source.

- Support for (optional) live view, in-device finger detection
  - Stretch goal: Support for Rolled fingerprints
  - Stretch goal: Measure pulse for liveness detection. E.g.: https://www.instructables.com/DIY-Pulse-Oximeter/
- All events should be signed by the device's own private key
  - which is part of an attestation chain.
- Events should also contain a timestamp and an application-provided nonce, to ensure it was generated in the current session.
- Use [TFLite](https://www.tensorflow.org/lite?hl=pt-br) for on-device finger detection (just using the average image value was a bit crappy)
- If possible, support on-device extraction/matching
- Also Accessible wirelessly (wifi/BT) from mobile device

## New SDK

- Must be [compatible with WebAssembly!](https://web.dev/porting-libusb-to-webusb/)
- Use [TFLite](https://www.tensorflow.org/lite?hl=pt-br) for best performance on image operations


# [FIDO](https://fidoalliance.org/download/)

## [Phone-based authenticator](https://github.com/paulo-raca/android-fido-authenticator)

After a while it became clear that having a separate trinket for FIDO is impratical. They are expensive-ish, most people don't have them and they are usually limited to a minimalistic interface and a small number of resident keys (Necessary for 1-factor authentication)

In the meantime, phones are on everybody's pockets and already contain all the hardware necessary. These should be first class authenticators!

There are some steps in this direction, with Chrome already supporting using devices as authenticators -- However this is a chrome-only thing, UI could be better, and phones don't yet actually behave as NFC/Bluetooth authenticators.

Also, it would be very desirable to sync the FIDO keys between devices, so that I don't lose access to all the things whenever I replace my phones (This did happen to me with a SoloKeys device 😖. I think this is what the whole Passkeys thing is about?

Finally, on Android FIDO is implemented by GMS (Probably a terrible idea?). While it supports NFC/BLE/HID/Platform authenticators, it seems impossible to support my own software authenticator running locally.

## Physical access

While FIDO is great for web/app authentication and has been making great strides in this front, I'm mostly concerned on physical access:

- Carrying keys is annoying,
- Most RFID tags are insecure (Just a number that can be readily copied with proper equipment)
- Biometrics has a number of issues (False positives, false negatives, _quality_ capture equipment is often expensive)

Tapping a FIDO key over NFC reader on the door knob or padlock would be pratical, cheap and secure (And that would fit perfectly with using a phone for authentication).

This could be achieved nicely using FIDO2 resident keys.

BLE would be suitable for Garage doors and cars. In the future, BLE direction-finding could be used to make everything even more secure (e.g., can only unlock within an specific area)

> [CCC Digital Key](https://carconnectivity.org/digital-key/) does that, but for cars.

# [AsyncIO Nettools](https://github.com/paulo-raca/aio-nettools)
- [x] Ping
- [ ] Traceroute
- [x] NDT7 (aka Speedtest)
- [ ] TFTP
- [ ] DNS (Client/Server)
- [ ] DHCP

# Highcharts
- Boost para highchart-contour - https://github.com/paulo-raca/highcharts-contour/issues/12
- Suporte para multiplos highchart-contour - https://github.com/paulo-raca/highcharts-contour/issues/13
- Error bar disaligned -- https://github.com/highcharts/highcharts/issues/5334
- Error bar unpredictable scale -- https://github.com/highcharts/highcharts/issues/5334

# Reverse Proxy
- Like ngrok or [serveo](https://serveo.net/)
- Implemented with [asyncssh](https://asyncssh.readthedocs.io) and [aiohttp](https://aiohttp.readthedocs.io/)
- servers created via `SSH -R`, like serveo
- Clients can access over different protocols:
  - HTTP/HTTPs, with one subdomain per client (Only for HTTP services, duh)
  - Randomly allocated ports for TCP services (specific subdomain/port number can be available on IPv6)
  - WebSockets (For TCP services) that tunnel the raw TCP traffic
  - SSH using port forwarding (`ssh -L`)
- Security -- Optionally, configure so that:
  - HTTP server can only be accessed after passing through Password/Oauth gate
  - TCP can only be accessed via `ssh -L`, using the same certificate
  - Maybe implement IP whitelist/blacklist
- Support for clusters:
  - Stores mapping of tunnels in shared storage and forward between servers
- Custom domain:
  - Configured via DNS SVC, like serveo
- Paying customers:
  - Identified via their SSH certificate
  - Get their own IP address/CNAME, which can be used on their custom domains
  - TCP servers are now bound to customer IP and receive the requested number
- Will have to have strong DNS integration
# [Automatically provision Let's Encrypt SSL certificate in Python](https://github.com/paulo-raca/asgi-acme) 

The whole certificate provisioning thing is boring and painful, even with the great tooling that Let's Encrypt/ACME gave us.

Ideally, in a zero-effort SSL provisioning, things would happen like this:
- A new SSL connection is opened
- The hostname is provided in the SNI
- If we don't yet have a certificate for this hostname, hold this connection and provision on asynchronously
  - Make a certificate request on Let's encrypt
  - Perform an ACME challenge with alpn (So that we don't rely on other layers of the server)
  - Fetch the signed certificate from Let's encrypt
  - Resume the initial connection
- Use the certificate from the cache

I've looked into it and it is mostly possible. However, in python SSL doesn't provide an alpn callback (necessary for the ACME challenge), and, most importantly,
the SNI callback is not async, and therefore cannot be paused until the certificate is generated asynchronously.

However this should work well-enough because the SSL operations likely run in a thread pool.

# CNC
- Large and scalable
- accurate to .1mm
- foldable for easy atorage
- Multi-tool:
  - CNC routerhttps://www.tensorflow.org/lite?hl=pt-br
  - CO2 laser cutter
  - 3D printer
  - pick and place
  - robotic arm

# Libretro
- [Core for Hack CPU from Nand2Tetris](https://github.com/paulo-raca/nand2tetris/tree/master/libretro)
  - Customizable peripherals: Keyboard, Mouse, Screen, Sound, System Calls, Serial, etc
- Core for Virtual Terminal
  - Execute bash, ssh, etc

# Fuse
- Improvements to fusepy
  - Custom encode/decode functions
  - support for libfuse3
- Completion of [fusetree](https://github.com/paulo-raca/fusetree) library
- Use fusetree for [MongoFS](https://github.com/paulo-raca/mongofs) and [SpotifyFS](https://github.com/paulo-raca/spotifyfs)

# MIDI
- MIDI-powered harmonica, pan flute, flute
- Inspiration: https://hackaday.com/2018/03/23/servos-do-the-plucking-in-this-midi-music-box/

# XPY
XHP for Python
- Implement custom python import: https://docs.python.org/3/reference/import.html
- XHP docs: https://docs.hhvm.com/hack/XHP/introduction

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
- ✓ Linux: https://lore.kernel.org/all/20170815223413.GA8886@amd/T/

# Experimental Design
  My HTML/CSS/JS skills were pretty lame when I wrote the current version
  - Make a pretty UI. Maybe Ionic + React?

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

# Lightsaber
- Widely based on [BUILDING THE BRIGHTEST LIGHT SABRE IN THE WORLD](http://hackaday.com/2016/10/25/building-the-brightest-light-sabre-in-the-world/)
- Hardware:
  - Brains: ESP32
  - Sensors: MPU9250 + BMP280/BME280
  - Light blade: WS2812B strip
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
    - OpenAPI? WebSocket? GraphQL? MQTT?
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

# Automation

## PyLC
A micropython/asyncio-based runtime for PLC, based on events and dataflow.

TODO: Details

# QUIC

## VPN 

> There is a standard: https://datatracker.ietf.org/doc/draft-ietf-masque-connect-ip/

## SSH v3

An essential feature of SSH is creating multiple tunnels for various subsystems, TCP channels, etc.
QUIC provides an ideal transport for that.

While we are at it:
[ ] AsyncSSH support
[ ] VHost support
[ ] UDP / VPN tunnel support

> Someone else is on it: https://datatracker.ietf.org/doc/draft-bider-ssh-quic/


# Machine Learning

## Neural Networks
- Rotational Convolutions
- Regression which returns stddev
- While trainning networks for Siamese/Tripplet classifiers, train it to also calculate a FAR and FRR
- Stochastic gradient for crazy nonlinear functions
- Recurrent neural network where instead of having a bazzilion cells and/or truncating the sequence, you propagate the gradients from previous iterations.

# Protobuf
The generated stubs kinda sucks -- Just like the thrift ones did until [nifty](https://github.com/facebookarchive/nifty)/[swift](https://github.com/facebookarchive/swift)/[drift](https://github.com/airlift/drift).

A better stub generator should be created, based on annotations and reflection.

- [ ] Java, based on annotations
- [ ] Python 3, based on @dataclass or Pydantic

