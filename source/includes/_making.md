# Making Stuff
CHIP is more than cool, small, inexpensive computer. It's a complete system for building projects that require remote control, network connectivity, and physical interfacing with people and the environment.
CHIP's pin headers have all the connections to make this happen. An annotated diagram of the pin headers can be found in the [hardware section](#pin-headers) of this manual.

## GPIO
GPIO provides basic digital connections to the physical world to create physical products with CHIP. These pins can act as 'reads' or 'writes', for example, to sense switch positions or turn an LED on or off.

### Read and Write From Command Line
CHIP has several General Purpose Input/Output (GPIO) pins available for you to build around. If you want to access them in a very primitive way, just to confirm their existence, here's some things to try.

### Requirements
  * CHIP
  * Jumper Wire
  * LED
  * SSH or serial connection to CHIP or
  * Monitor and keyboard

### How You See GPIO
There are eight (8) GPIO pins always available for connecting CHIP to the sense-able world. If you orient CHIP with the USB connector pointed up, you'll find the GPIO pins in the middle of the right header, U14, Pins 13-20, labeled XIO-P0 to P7: 

![Pinout diagram for CHIP](images/chip_pinouts.jpg)

### How The System Sees GPIO
There is a `sysfs` interface available for the GPIO. This just means you can access the GPIO states in a file-system-like manner. For example, you can reference XIO-P0 using this path:

```shell
  /sys/class/gpio/gpio408/
```

The number is somewhat unfortunate, since the `sysfs` names do not match the labels on our diagram! But is not too hard to translate. PinsXIO-P0 to P7 linearly map to gpio408 to gpio415.

### Some GPIO Switch Action
These lines of code will let us read values on pin XIO-P7. First, we tell the system we want to listen to this pin:

```shell
  sudo sh -c 'echo 415 > /sys/class/gpio/export'
```

View the mode of the pin. It should return “in”:

```shell
  cat /sys/class/gpio/gpio415/direction
```

Connect a jumper wire between Pin 20 (XIO-P7) and Pin 39 (GND). Now use this line of code to read the value:

```shell
  cat /sys/class/gpio/gpio415/value
```

### Some GPIO Output
You could also change the mode of a pin from “in” to “out”

```shell
  sudo sh -c 'echo out > /sys/class/gpio/gpio415/direction'
```

Now that it's in output mode, you can write a value to the pin:

```shell
  sudo sh -c 'echo 1 > /sys/class/gpio/gpio415/value'
```

If you attach an LED to the pin and ground, the LED will illuminate according to your control messages.

### Enough IO
When you are done experimenting, you can tell the system to stop listening to the gpio pin:

```shell
  sudo sh -c 'echo 415 > /sys/class/gpio/unexport'
```

### Learn More
You can learn more about GPIO and Linux [here:](https://www.kernel.org/doc/Documentation/gpio/sysfs.txt)

## Python Library
A Python-based library for accessing GPIO data is in development.

## GPIO Types
There are many types of sensors that can be used with GPIO:

### Switches
Switches provide on/off state input from the physical world to your computer. You can [use the commandline interface](#some-gpio-switch-action) to listen to switch values. A python library was created for the [ChippyRuxpin project](https://github.com/NextThingCo/ChippyRuxpin) if you need a higher-level example in python. 

### LEDs
LEDs can be illuminated and turned off using the [commandline interface](#some-gpio-output). Refer to the [ChippyRuxpin project](https://github.com/NextThingCo/ChippyRuxpin) on a good example on how to manipulate the commandline using python.

### Relays
Relays are special hardware bridges used to switch higher voltage electronics, protecting CHIP from the high voltages that would destroy it.  Using a relay board is programmatically no different from using LEDs.

## Expanding GPIO
If you don't need to drive an LCD, you can use those pins for more, faster GPIO if you want to. 
These are the pins numbered 18-40 on U13 and 27-40 on U14 to act as GPIO to increase the number of available GPIO pins. 
Documentation on this process is forthcoming!

## Analog to Digital Conversion
Pin 9 on header U14 provides a link for low resolution analog to digital conversion (ADC). 
There is no driver for this link yet. ADC is used to read continuous sensors (temperature, pots, FSR, photoresistor, etc)


## 1 Wire
The 1 Wire serial protocol is not yet implemented for CHIP. 

## UART
UART connections can be made using the UART connections on header U14. 

## PWM
Pulse Width Modulation is used to control motors and other devices. 
It is possible to use GPIO pins to drive motors, but they generally are not fast enough for robust and smooth control.
PWM can be accessed through an `sysfs` protocol.

## I2C
I2C can be accessed through a `sysfs` protocol using the debian i2c-tools. In the terminal, use

```shell
sudo apt-get install i2c-tools
```

## LCD Monitor Support
Using the numerous LCD header pins, a color touchscreen panel can be directly implemented on CHIP.

## Project Examples
Projects coming soon!
