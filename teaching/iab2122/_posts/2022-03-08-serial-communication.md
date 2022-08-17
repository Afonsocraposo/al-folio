---
layout: post
title: Serial communication
description: How does serial communication work?
comments: false
---

If you are doing L1. Seeeduino Nano Essentials, you'll need to calculate the ammount of bits sent through the serial port in each main loop iteration.

In your script you can find:
```
Serial.println(<stuff goes here>);
```

Every time that command is executed, the data is sent in a frame like so:

> Each block (usually a byte) of data transmitted is actually sent in a packet or frame of bits. Frames are created by appending synchronization and parity bits to our data.

![Serial communication frame](https://cdn.sparkfun.com/r/700-700/assets/f/9/c/0/2/50d2066fce395fc43b000000.png)

In the Arduino `Serial.begin()` [documentation](https://www.arduino.cc/reference/en/language/functions/communication/serial/begin/), you can find:

> An optional second argument configures the data, parity, and stop bits. The default is 8 data bits, no parity, one stop bit.

I advise you to read more about it at [Serial Communication - Sparkfun](https://learn.sparkfun.com/tutorials/serial-communication).
