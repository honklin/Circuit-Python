![Servo](https://user-images.githubusercontent.com/121810694/222765854-081e89f1-702e-4609-8001-016bb00b7615.png)
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


![HelloCircuitPython](https://user-images.githubusercontent.com/121810694/222486897-bb95fb8c-843e-433a-9f74-883e043ee726.mov)




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

![Servo](https://user-images.githubusercontent.com/121810694/222765854-081e89f1-702e-4609-8001-016bb00b7615.png)

### Reflection
The hardest part of this assignment was learning how capacitive touch works and how to wire it on the Metro. It uses a pretty understandable library called touchio that establishes pins as touch sensors. The wiring was a bit more difficult, because the touch wires need a grounded resistor to restrict some of the voltage from going straight into the board.




## Distance Sensor

### Description
This assignment makes the neopixel red if an object is within 5 cm, green if it's more than 35 cm, blue in between, and fades between the different colors.

### Code

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

https://user-images.githubusercontent.com/121810694/217868476-e565f3ca-da63-456b-a99f-a72157fd72f2.GIF

### Wiring

![Distance Sensor](https://user-images.githubusercontent.com/121810694/217875442-86b62b5d-6482-46b9-9239-192163d9f427.png)

### Reflection
The hardest part of this assignment was figuring out how to get the neopixel to fade between colors. I eventually figured out how much each rgb value would need to increase and decrease between each pin and used that to find the value for each rgb value with an equation using the distance. I also realized that I had to find some way to catch the runtime errors that kept being thrown so I used a try except statement that would only run the code if it wasn't going to throw a runtime exception.





## LCD

### Description
This assignment prints the total of buttons adding or subtracting on an LCD screen.

### Code

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

https://user-images.githubusercontent.com/121810694/217867370-0524ab2c-ccad-4789-a559-7e34115aeb3d.GIF

### Wiring

![LCD](https://user-images.githubusercontent.com/121810694/217872669-8773a2da-35cf-47a8-aa9f-639396797319.png)

### Reflection
This assignment's most challenging part was connecting it to Metro with the LCD imports and pin connections. Because I was a bit unfamiliar with the Circuit Python libraries, I accidentally put the files in the wrong drive before discovering that my code files need to go on the Circuit Python drive on my computer, not the one that links to the actually drive on the Metro, but the libraries need to go on the Metro drive. I also didn't realize that the Metros have built in SCL and SDA pins, so you don't have to declare them, Circuit Python just expects them to be plugged in there.





## Motor

### Description & Code

```python
Code goes here

```

### Evidence

### Wiring

### Reflection
