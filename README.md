# CircuitPython
## Table of Contents
* [Hello CircuitPython](#Hello_CircuitPython)
* [Servo](#Servo)
* [Distance Sensor](#Distance_Sensor)
* [LCD](#LCD)
* [Motor](#Motor)
* [Temperature Sensor](#Temperature_Sensor)
* [Rotary Encoder](#Rotary_Encoder)
* [Photointerrupter](#Photointerrupter)
---

## Hello_CircuitPython

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

https://user-images.githubusercontent.com/121810694/223172323-18e369b9-536b-4069-99a7-a207225567c3.mov

Video of neopixel changing colors

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

https://user-images.githubusercontent.com/121810694/223171944-63d5da51-4c83-4317-a7be-14b21c320523.mp4

Video of servo changing degrees using capacative touch

### Wiring

![Servo](https://user-images.githubusercontent.com/121810694/222766965-4637e587-510e-40ff-85f8-080561421d7e.png)
Wiring for Servo

### Reflection
The hardest part of this assignment was learning how capacitive touch works and how to wire it on the Metro. It uses a pretty understandable library called touchio that establishes pins as touch sensors. The wiring was a bit more difficult, because the touch wires need a grounded resistor to restrict some of the voltage from going straight into the board.




## Distance_Sensor

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

Video of distance sensor changing neopixel based on distance of object

### Wiring

![Distance Sensor](https://user-images.githubusercontent.com/121810694/217875442-86b62b5d-6482-46b9-9239-192163d9f427.png)
Wiring for Distance Sensor

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

Video of LCD printing number of presses with adding and subtracting buttons

### Wiring

![LCD](https://user-images.githubusercontent.com/121810694/223163649-a5ce9926-c3f3-4120-be41-5f3053a36834.png)
Wiring for LCD


### Reflection
This assignment's most challenging part was connecting it to Metro with the LCD imports and pin connections. Because I was a bit unfamiliar with the Circuit Python libraries, I accidentally put the files in the wrong drive before discovering that my code files need to go on the Circuit Python drive on my computer, not the one that links to the actually drive on the Metro, but the libraries need to go on the Metro drive. I also didn't realize that the Metros have built in SCL and SDA pins, so you don't have to declare them, Circuit Python just expects them to be plugged in there.





## Motor

### Description
This assignment speeds up and slows down a motor using a potentiometer.

### Code

```python
import board
import time
import simpleio
import analogio
import pwmio

pot = analogio.AnalogIn(board.A1) # potentiometer
motor = pwmio.PWMOut(board.D8,duty_cycle = 65535,frequency=5000) # motor

while True: # keeps motor running
    potValue = pot.value # potentiometer values
    map = int(simpleio.map_range(potValue,0,65535,0,255)) # converts potentiometer values to speed
    motor.duty_cycle = potValue # motor speed
    time.sleep(.01)
```

### Evidence

https://user-images.githubusercontent.com/121810694/224369479-56896006-b3df-4313-82f4-8b000a13970e.MOV

Video of motor changing speed based on potentiometer position

### Wiring

![Motor](https://user-images.githubusercontent.com/121810694/224090082-2510f995-268c-48f4-ba89-bab9ef72522d.png)
Wiring for Motor

### Reflection
It was very important in this assignment to convert the values from the potentiometer value range (0-255), to the motor value range (0-65535). I used the map function to convert the potentiometer values to a 0-65535 range so that it matches the motor speed values. I had thought that the map function would find the new values from the variables given and store them in a new variable, but it instead stores the mapped values in one of the given variables. The function maps the potentiometer values and stores the new values in the same potentiometer variable.





## Temperature_Sensor

### Description
This assignment shows the temperature on an LCD using a temperature sensor.

### Code

```python
import board
from lcd.lcd import LCD # lcd libraries
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
import analogio
import simpleio # map library
import time

i2c = board.I2C() # lcd declaration
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

tempSensor = analogio.AnalogIn(board.A0) # temperature sensor

temp = 74 # temperature
oldTemp = 0 # refresh variable
message = " "

while True:
    temp = int(simpleio.map_range(tempSensor.value,0,65535,32,212)) # maps values to Fahrenheit
    if (oldTemp != temp): # checks if needs to reprint lcd text
        if (temp <= 70): # higher than 70
         message = "Too cold!"
        elif (temp >= 78): # higher than 78
            message = "Too hot!"
        else: # 70-78
            message = "Just right"
        lcd.clear()
        lcd.set_cursor_pos(0,0)
        lcd.print(str(temp)) # prints temp
        lcd.set_cursor_pos(0,3)
        lcd.print("deg F")
        lcd.set_cursor_pos(1,0)
        lcd.print(message)
    oldTemp = temp
    time.sleep(1)
```

### Evidence

https://user-images.githubusercontent.com/121810694/225058207-99a6e2d3-5c1a-41ee-8983-57bb746485ee.mp4

Video of LCD displaying changing temperature sensor values

### Wiring

![Temperature Sensor](https://user-images.githubusercontent.com/121810694/225062286-b8dbbc86-3c23-40e8-9475-0bb9638b4500.png)
Wiring for Temperature Sensor

### Reflection
The hardest part of this assignment was finding the temperature values from the sensor. There isn't a library for the temperature sensor so I declared the sensor as an analog pin and found the values using analogio. The values I was getting were much too high so I mapped the values from 0 - 65535 to 32 - 212, which is the range for Fahrenheit. It was also important to remember that the SDA and SCL pins on the LCD screen go to the SDA and SCL pins on the Metro.






## Rotary_Encoder

### Description
This assignment changes traffic lights using a rotary encoder and displays the state on an LCD.

### Code
```python
import board
from lcd.lcd import LCD # lcd libraries
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
import rotaryio # rotary encoder library
import digitalio # led library
import time

i2c = board.I2C() # lcd declaration
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

encoder = rotaryio.IncrementalEncoder(board.D3,board.D4) # rotary encoder potentiometer

button = digitalio.DigitalInOut(board.D2) # rotary encoder button
button.pull = digitalio.Pull.UP
button.direction = digitalio.Direction.INPUT

stop = digitalio.DigitalInOut(board.D13) # red led
stop.direction = digitalio.Direction.OUTPUT

caution = digitalio.DigitalInOut(board.D12) # yellow led
caution.direction = digitalio.Direction.OUTPUT

go = digitalio.DigitalInOut(board.D11) # green led
go.direction = digitalio.Direction.OUTPUT

position = 0 # modified position
states = ["stop", "caution", "go"] # states
state = " " # lcd print state
x = 0 # array selection

while True:
    prestate = state # uses for reprinting
    position = encoder.position % 20 # finds ticks from 0
    if (position < 7): # if stop
        x = 0
    elif (position > 12): # if go
        x = 2
    else: # if caution
        x = 1
    state = states[x] # sets state
    if (button.value == False): # if button is pressed
        stop.value = False
        caution.value = False
        go.value = False
        if (state == "stop"): # turns on correct light
            stop.value = True
        elif (state == "caution"):
            caution.value = True
        else:
            go.value = True
    if (state != prestate): # reprints lcd data
        lcd.clear()
        lcd.set_cursor_pos(0,0)
        lcd.print("Push for ")
        lcd.set_cursor_pos(0,9)
        lcd.print(state) # prints state
    time.sleep(0.1)
```

### Evidence

https://user-images.githubusercontent.com/121810694/226646755-eea054a9-9ec1-4bef-9fad-6f0f25bfa037.MOV

Video of rotary encoder selecting traffic states displayed on an LCD and turning on the correct LED

### Wiring

![Rotary Encoder](https://user-images.githubusercontent.com/121810694/226651057-d4c9f24b-7374-41de-8f0a-3488ab28b840.png)
Wiring for Rotary Encoder

### Reflection
In this assignment, it was very important to find the library for rotary encoders. Rotaryio lets the Metro communicate with both the button and the potentiometer on the rotary encoder, but they must be declared separately. The hardest part of this assignment was modifying the encoder position so that it could be used for the LEDs. I used % to find the remainder of 20 in order to fix the continuous rotation position, then I checked which third of the circle the encoder was in.







## Photointerrupter

### Description
This assignment displays the number of times a photointerrupter is interrupted on an LCD screen every 4 seconds.

### Code
```python
import board
from lcd.lcd import LCD # lcd liraries
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
import digitalio # use photointerrupter was digital input
import time

i2c = board.I2C() # lcd declaration
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

photo = digitalio.DigitalInOut(board.D2) # photointerrupter
photo.direction = digitalio.Direction.INPUT

interrupts = 0
now = time.monotonic() # keeps time without sleep()
increase = False

while True:
    while (photo.value == True): # loops while being interrupted
        increase = True
    if (increase == True): # only counts one per interrupt
        interrupts += 1
        increase = False
    if (time.monotonic() - now >= 4): # prints every 4 seconds
        lcd.clear()
        lcd.set_cursor_pos(0,0)
        lcd.print("The number of")
        lcd.set_cursor_pos(1,0)
        lcd.print("interrupts is ")
        lcd.set_cursor_pos(1,14)
        lcd.print(str(interrupts)) # prints number of interrupts
        now = time.monotonic() # restarts counting
```

### Evidence

https://user-images.githubusercontent.com/121810694/227567106-54802ce9-0e27-498e-9d23-e5254b33d241.mp4

Video of LCD displaying the number of interrupts every 4 seconds

### Wiring

![Photointerrupter](https://user-images.githubusercontent.com/121810694/226665078-655c64fd-0e29-41fc-a4ef-7e928f467bf7.png)
Wiring for Photointerrupter

### Reflection
I couldn't find a library for the photointerrupter so I declared it as a digital input that would send input similar to how a button would. I also needed to use monotonic() instead of sleep() to delay reprinting the LCD because sleep() stops the code and therefore wouldn't recognize when it is interrupted. However, monotonic() records the current time, so comparing the time back to the old time throughout the code will allow it to reprint the LCD every 4 seconds and also recognize any interruptions. I also found that I needed to check the photointerrupter in a loop and only increase the total after it had turned false because if I do if interrupted, the code keeps counting until it is uninterrupted.
