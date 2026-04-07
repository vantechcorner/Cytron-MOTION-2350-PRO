# Omnidirectional robot via USB joystick (MOTION 2350 Pro)

Build an omnidirectional (e.g. Mecanum) robot with [MOTION 2350 PRO](https://cytron.io/p-motion-2350-pro). The board’s four motor channels drive four wheels; a **USB HID joystick or game controller** plugs into the on-board USB host port.

## How the sketch works

- **Core 1** runs the USB host stack (`USBHost.task()`): TinyUSB + PIO-USB on the RP2040.
- **Core 0** reads global joystick values updated by HID callbacks, applies dead zone and an optional speed multiplier, mixes **forward + strafe** into four wheel speeds, and drives `CytronMD` on PWM pins 8–15.
- HID **report layout is device-specific**. Many Xbox-style dongles use **at least 5 bytes**: left stick at `report[3]` / `report[4]` with **neutral 0x80**; buttons from `report[0]` and `report[1]`. If your pad differs, use Serial dump to change indices in `tuh_hid_report_received_cb`.

There is **no in-place rotation (yaw)** in the mix; only forward/back and lateral strafe. Add a third axis or buttons and extend the kinematics if you need spin.

## Requirements

**Hardware**

* [MOTION 2350 Pro](https://cytron.io/p-motion-2350-pro)
* 4× [TT motor](https://cytron.io/p-3v-6v-dual-axis-tt-gear-motor) (or compatible)
* 4× [Mecanum wheel set for TT motor](https://cytron.io/p-mecanum-wheel-set-for-tt-motor)
* Chassis
* USB joystick or game controller (HID)

**Software**

* Arduino library: **Pico PIO USB** (sekigon-gonnoc)

**Arduino IDE settings**

* `Tools` → **USB Stack** → **Adafruit TinyUSB**
* `Tools` → **CPU Speed** → **120 MHz** (or 240 MHz; PIO-USB needs a multiple of 120 MHz — see `usbh_helper.h`)

## Resources

* [MOTION 2350 Pro — resources](https://cytron.io/p-motion-2350-pro#tab-resource)
