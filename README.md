# Qubes Keyboard Project

The plan is to make a USB keyboard, for use within Qubes, that's end-to-end
encrypted between the keyboard and `dom0`, so that the untrusted `sys-usb`
can't log keystrokes. Also, the backlight color of the keyboard will change
match the color of the AppVM that you're currently highlighting.

Idea:

First, I'll need a USB keyboard. Then I'll need an embedded device (maybe a
Raspberry Pi), to act as a USB man-in-the-middle: plug the keyboard into the
device, plug the device into the computer. The device will listen for keystrokes
from the keyboard, encrypt them, and forward them, over a USB OTG port, to a
service running in `dom0`. That service will then decrypt them, and type them
for the user. On first setup, the user can install software in `dom0`, and run
it to see a random passphrase. Then they can press a button on the device to put
it in program mode, and type the passphrase, to set a shared key between the two
ends.

Components:

* Custom qrexec service, like `qubes.KeyboardInput` but `qubes.EncryptedKeyboardInput`
* Software to run on embedded device
* Software to run in `sys-usb`
* Software to run in `dom0`

Research:

* Logitech G213 Prodigy Keyboard, costs ~$47, is a gaming keyboard with
  adjustable backlight colors
* [glight](https://github.com/sgdw/glight) is a small open source project with
  sample code for controlling this Logitech keyboard's backlight colors
* Simple [guide](https://gist.github.com/gbaman/50b6cca61dd1c3f88f41) for
  setting up OTG modes on the Raspberry Pi Zero
* [libusb](https://pypi.org/project/libusb/) is a python module for communicating
  with USB devices

Considerations:

* Am I really comfortable with a Raspberry Pi MITMing all my keystrokes? An
  attacker with physical access could easily pop out the SD card and modify to
  log keystrokes, and even connect to wifi and send them over a network. On the
  other hand, attackers can do this with other embedded hardware anyway, like an
  arduino, or even with the keyboard firmware itself. A Raspberry Pi just makes
  their job simpler.
* That said, I can get a Raspberry Pi prototype up in less time, and I already
  understand how to do it. I think I should start with that.

Parts:

* [Logitech G213 Prodigy Keyboard](https://www.amazon.com/Logitech-Keyboard-Dedicated-Controls-Spill-Resistant/dp/B01K48R5V4), $46.78
* [Micro-USB to USB-A cable](https://www.amazon.com/AmazonBasics-Male-Micro-Cable-Black/dp/B0711PVX6Z/ref=sr_1_3_acs_sk_pb_1_sl_1?ie=UTF8&qid=1542049211&sr=8-3-acs), $4.99
* [Raspberry Pi Zero](https://www.adafruit.com/product/2885), $5.00
* [USB Mini Hub with Power Switch - OTG Micro-USB](https://www.adafruit.com/product/2991), $4.95
* [Adafruit Raspberry Pi Zero Case](https://www.adafruit.com/product/3252), $4.75

Total: $66.47 + shipping
