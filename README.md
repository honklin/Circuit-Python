# CircuitPython
This repository will actually serve as a aid to help you get started with your own template.  You should copy the raw form of this readme into your own, and use this template to write your own.  If you want to draw inspiration from other classmates, feel free to check [this directory of all students!](https://github.com/chssigma/Class_Accounts).
## Table of Contents
* [Hello_CircuitPython](#Hello_CircuitPython)
* [Servo](#Servo)
* [Distance Sensor](#Distance_Sensor)
* [LCD](#LCD)
* [Motor](#Motor)
---

## Hello_CircuitPython

### Description & Code
This assignment makes the neopixel on the Metro change between colors of the rainbow.

### Code

```python
import board
import neopixel

dot = neopixel.NeoPixel(board.NEOPIXEL,1)
dot.brightness = .25

print("Hello Circuit Python")
print("Make it change colors")

r = 225
g = 0
b = 0

while True:
    if (r > 0 and g == 225):
        r -= 1
    if (r < 225 and b == 225 and g == 0):
        r += 1
    if (g < 225 and r == 225 and b == 0):
        g += 1
    if (g > 0 and b == 225):
        g -= 1
    if (b < 225 and r == 0 and g == 225):
        b += 1
    if (b > 0 and r == 225 and g == 0):
        b -= 1
    print("red")
    print(r)
    print("green")
    print(g)
    print("blue")
    print(b)
    dot.fill((r,g,b))
```


### Evidence


![spinningMetro_Optimized](https://user-images.githubusercontent.com/54641488/192549584-18285130-2e3b-4631-8005-0792c2942f73.gif)




### Reflection
The most challenging part of this assignment was figuring out how to connect the Metro to my computer and how to move the libraries into the correct places to be able to run the code. I had tried to use a Mac to do these assignments at home, but I discovered that the method for running code pretty much only works on a PC. Also, I found that for libraries that need to be added to the CIRCUITPY folder, you should put the files in the lib folder within CIRCUITPY, not just under the folder. Sometimes the Metro will already have some of the files added, but they may be the wrong version of Circuit Python.




## Servo

### Description & Code

```python
Code goes here

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




## LCD

### Description & Code

```python
Code goes here

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
