# CircuitPython
This repository will actually serve as a aid to help you get started with your own template.  You should copy the raw form of this readme into your own, and use this template to write your own.  If you want to draw inspiration from other classmates, feel free to check [this directory of all students!](https://github.com/chssigma/Class_Accounts).
## Table of Contents
* [Hello Circuit Python](#Hello_CircuitPython)
* [Servo](#Servo)
* [Distance Sensor](#Distance_Sensor)
* [LCD](#LCD)
* [Motor](#Motor)
---

## Hello CircuitPython

### Description
This assignment makes the neopixel on the Metro change between colors of the rainbow.

### Code

```python
import board
import neopixel

dot = neopixel.NeoPixel(board.NEOPIXEL,1)
dot.brightness = .25

print("Hello Circuit Python")
print("Make it change colors")

r = 225 # rgb values
g = 0
b = 0

while True:
    if (r > 0 and g == 225): # decreases red
        r -= 1
    if (r < 225 and b == 225 and g == 0): # increases red
        r += 1
    if (g < 225 and r == 225 and b == 0): # increases green
        g += 1
    if (g > 0 and b == 225): # decreases green
        g -= 1
    if (b < 225 and r == 0 and g == 225): # increases blue
        b += 1
    if (b > 0 and r == 225 and g == 0): # decreases bue
        b -= 1
    print("red")
    print(r)
    print("green")
    print(g)
    print("blue")
    print(b)
    dot.fill((r,g,b)) # fills
```


### Evidence


![HelloCircuitPython]




### Reflection
The most challenging part of this assignment was figuring out how to connect the Metro to my computer and how to move the libraries into the correct places to be able to run the code. I had tried to use a Mac to do these assignments at home, but I discovered that the method for running code pretty much only works on a PC. Also, I found that for libraries that need to be added to the CIRCUITPY folder, you should put the files in the lib folder within CIRCUITPY, not just under the folder. Sometimes the Metro will already have some of the files added, but they may be the wrong version of Circuit Python.






## Servo

### Description
This assignment makes a servo change between 0 and 180 degrees using capacitive touch.

### Code

```python
import board
from adafruit_motor import servo
import touchio
import pwmio
import time

pwm = pwmio.PWMOut(board.D13,duty_cycle=2**15,frequency=100)
servo = servo.Servo(pwm) # servo object

touch1 = touchio.TouchIn(board.A0) # buttons
touch2 = touchio.TouchIn(board.A1)

servo.angle = 90

while True:
    if servo.angle <= 178 and touch1.value: # increases til 180
        servo.angle += 2
        time.sleep(.01)
    elif servo.angle >= 2 and touch2.value: # decreases til 0
        servo.angle -= 2
        time.sleep(.01)
```

### Evidence

Pictures / Gifs of your work should go here.  You need to communicate what your thing does.

### Wiring

### Reflection




## Distance Sensor

### Description & Code

```python
import board
import adafruit_hcsr04
import neopixel
import time

dist = adafruit_hcsr04.HCSR04(trigger_pin = board.D5, echo_pin = board.D6)

neo = neopixel.NeoPixel(board.NEOPIXEL,1)
neo.brightness = 0.2

r = 0 # rgb values
g = 0
b = 0

while True:
    try:
        if (dist.distance <= 5): # red if less than 5
            r = 225
            g = 0
            b = 0
        elif (dist.distance >= 35): # green if greater than 35
            r = 0
            g = 225
            b = 0    
        else:
            if (dist.distance >= 15 and dist.distance <= 25): # blue if 15-25
                r = 0
                g = 0
                b = 225
            elif (dist.distance < 15): # fades to red
                r = 225 - 22.5 * (dist.distance - 5)
                b =  22.5 * (dist.distance - 5)
            else: # fades to green
                g = 22.5 * (dist.distance - 25)
                b = 225 - 22.5 * (dist.distance - 25)
        neo.fill((r,g,b))
    except RuntimeError: # catches exception
        print("retry")
    time.sleep(1)
```

### Evidence

### Wiring

### Reflection




## LCD

### Description & Code

```python
import board
import time
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
from digitalio import DigitalInOut, Direction, Pull

i2c = board.I2C()
lcd = LCD(I2CPCF8574Interface(i2c, 0x3f), num_rows=2, num_cols=16)

up = DigitalInOut(board.D12) # up button
up.pull = Pull.UP
up.direction = Direction.INPUT

down = DigitalInOut(board.D13) # down button
down.pull = Pull.UP
down.direction = Direction.INPUT

count = 0
precount = -1 # placeholder for comparison
direction = " "
space = 0 # lcd spacing

while True:
    precount = count
    if (up.value): # counts up
        print("up")
        while (up.value):
            direction = "Up:"
        count += 1
        space = 4
    elif (down.value): # counts down
        print("down")
        while (down.value):
            direction = "Down:"
        count -= 1
        space = 6
    if (count != precount): # prevents holding counts
        lcd.clear()
        lcd.set_cursor_pos(0,0)
        lcd.print(direction) # print direction
        lcd.set_cursor_pos(0,space)
        lcd.print(str(count)) # print count
    time.sleep(.01)
```

### Evidence

Pictures / Gifs of your work should go here.  You need to communicate what your thing does.

### Wiring

### Reflection





## Motor

### Description & Code

```python
Code goes here

```

### Evidence

### Wiring

### Reflection
