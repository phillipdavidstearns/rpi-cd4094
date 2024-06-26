# rpi-cd4094

A class that simplifies controlling CD4094 CMOS 8-bit latched shift registers from a Raspberry Pi

## Installation

1. `$ sudo apt-get update; sudo apt-get install git python3-pip python3-pigpio pigpiod`
1. `$ git clone https://github.com/phillipdavidstearns/rpi-cd4094.git`
1. `$ cd rpi-cd4094`
1. `$ sudo python3 -m pip install -e ./`

## Usage

```python
#-----------------------------------------------------------------
# import the CD4094 class
from CD4094 import CD4094

#-----------------------------------------------------------------
# Quick initialization
# start new controller for CD4094
# defaults to a single chip w/ 8 output channels
# uses default pins for controlling the chip

controller = CD4094()

#-----------------------------------------------------------------
#Detailed initialization

# setup pins using BCM GPIO pin numbers (values below are default)
STROBE = 17 # latch strobe GPIO pin
DATA = 27 # data GPIO pin
CLOCK = 22 # clock GPIO pin
ENABLE = 23 # enable GPIO pin

#specify number of channels in the shift register chain (default 8)
channels = 8 # 

#bundle pins
pins = [STROBE, DATA, CLOCK, ENABLE]

#initializes a new controller for CD4094
controller = CD4094(pins=pins, channels=channels)

# toggle output
controller.enable()
controller.disable()

# flush register
controller.reset()

# pre-exit cleanup. use before terminating parent application to flush output
controller.stop()

# load data into register
pin_states = [ 1, 0, 1, 0, 1, 0, 1, 0 ] #list of int values 0-1

data = 0

for i in range(len(pin_states)):
	data |= pin_states[i] << i

controller.update(data)
```
