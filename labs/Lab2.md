---
title: Lab 2
nav_order: 2
layout: default
---

# Lab 2
{: .fs-9 }

The purpose of this lab is to get introduced to the IMU and to learn how to access and read the accelerometer and gyroscope data and send it to the computer from the Arduino. Additionally, this lab is also to help learn how to interpret and analyze the signals from the IMU as well as apply filters to reduce noise and drift.

# IMU Setup
{: .fs-7 }

# IMU Connection
{: .fs-5 }
We can see the connected Arduino to the IMU in the image below.
![IMU](lab2pics/IMU_Connection.jpg)

# IMU Example
{: .fs-5 }

After setting up the IMU, I ran the Example1_Basics code in the IMU library, which can be seen in the video below:

[![IMU Example](https://img.youtube.com/vi/IkeaU4ozztM/0.jpg)](https://www.youtube.com/watch?v=IkeaU4ozztM)

What can be seen in the video, are the gyroscope and accelerometer readings of their corresponding x, y, z values.

# AD0_VAL Definition
{: .fs-5 }

The AD0_VAL represents the value of the last bit of the I2C address. By default the value is 1, but if the ADR jumper is closed then the value is 0. The value that worked for me was the default value 1.

# Blink on Start-Up
{: .fs-5 }

Below is the visual blinking indicator on start-up.

[![IMU Example](https://img.youtube.com/vi/0KBPiSol0o0/0.jpg)](https://www.youtube.com/watch?v=0KBPiSol0o0)

# Acceleration and Gyroscope data discussion
{: .fs-5 }

Below is a video showing what happens when I accelerate, rotate, and flip the IMU with its corresponding gyroscope, accelerometer, and magnetometer readings.

[![IMU Demo](https://img.youtube.com/vi/7rHA0MWJKgs/0.jpg)](https://www.youtube.com/watch?v=7rHA0MWJKgs)

As I briefly explained earlier, from this demonstration, we can see how when I accelerate the IMU in the according direction, the reading goes up correspondingly. 
Similarly, since the gyroscope only measures the angular velocity in degrees per second, we see that it only goes up when I try to rotate the IMU. For the magnetometer, I noticed thet the readings got increasingly bigger as the IMU approached my laptop. Furthermore, one thing I noticed is that only for acceleration in the z direction does the IMU hover around 1000 mg, which is likely due to the gravitational acceleration. 

# Accelerometer
{: .fs-7 }

# Accelerometer Data
{: .fs-5 }

From lecture, we know that the accelerometer equations to convert into roll and pitch are the following:

![](lab2pics/aeq.png)

I then implemented these equations in the following code:

![](lab2pics/acode.png)

The following pictures show the roll and pitch values at {-90, 0, 90}

Roll = 0 and Pitch = 0

![](lab2picsrp0.png)

Pitch = -90

![](lab2pics/p-90.png)

Pitch = 90

![](lab2pics/p90.png)

Roll = -90

![](lab2pics/r-90.png)

Roll = 90

![](lab2pics/r90.png)

# Accelerometer Accuracy
{: .fs-5 }

Given the above screenshots, we can see that the accelerometer is actually very accurate for the given range. We can use the two point calibration technique to see how accurate this range is:

Roll:

For the roll values, for the range between -90 and 90, the actual values are -88.83 and 89.30. We can calculate the two point calibration for a range of 180 degrees by taking (89.30 - (-88.83) - 180)/180 = -0.0103. Making the error for the roll about -1%, which is very good.

Pitch:

For the pitch values, for the range between -90 and 90, the actual values are 89.05 and -89.61. We then find the two point calibration error by finding (89.05 - (-89.61) - 180)/180 = -.0074. This makes the error for pitch less than negative 1 percent, which is also very accurate.

# Noise in the frequency spectrum analysis
{: .fs-5 }

Since the accelerometer has a lot of noise within the sensor, we can do a fourier transformation analysis to find a cutoff frequency we can use in order to make a low-pass filter, which will help reduce the noise. 

First, we can see the pitch and roll graphs over time without any filters.

![](lab2pics/roll.png)

![](lab2pics/pitch.png)

This was implemented in blearduino.ino with the following command code:

![](lab2pics/imucode.png)

In the Jupyter library, I received this data using a notification handler similar to what we did in the previous lab:

![](lab2pics/jupimu.png)

After this, I then performed a Fourier Transform in Python on the data in order to get the roll and pitch data with respect to their frequencies rather than over time. 

![](lab2pics/fftcode.png)

The following graphs show the FFT transformed roll and pitch data:

![](lab2pics/fftpitch.png)

![](lab2pics/fftroll.png)

Given the equations:

![](lab2pics/eq.png)

As well as the T value of around 357 Hz according the above data, we find that the alpha value is equal to 0.157.

We then implement the low pass filter as follows:

![](lab2pics/lpfcode.png)

Then, in the following graphs of roll and pitch, we see that there is a decrease in the overall noise of the data:

![](lab2pics/lpfroll.png)

![](lab2pics/lpfpitch.png)

This is even more noticeable in the Fourier Transformation of this filtered data:

![](lab2pics/lpfftroll.png)

![](lab2pics/lpfftpitch.png)

# Gyroscope
{: .fs-7 }

# Gyroscope Data
{: .fs-5 }

Rather than the accelerometer, we can also use the gyroscope in the IMU to calculate the roll, pitch, and now the yaw. This is implemented in the Arduino IDE through the following code:

![](lab2pics/gyrocode.png)

Below we see what the roll, pitch, and yaw data values look like for different positions of the IMU:

![](lab2pics/1.jpg)

![](lab2pics/2.jpg)

![](lab2pics/3.jpg)

One noticeable difference between the data from the gyroscope and the accelerometer is that there is is less noise but there is a very significant drift. For example if when I started the IMU flush with the table and then rotated. When I returned it back to being flush with the table, the angle for the pitch was very different than it was previously. By increasing the sampling frequency, I saw that the values were more accurate for the gyroscope than they were previously.

# Gyroscope Complementary Filter
{: .fs-5 }

For the gyroscope complementary filter, I implemented it as follows:

![](lab2pics/compfcode.png)

[![](https://img.youtube.com/vi/6BWCI2gKimM/0.jpg)](https://www.youtube.com/watch?v=6BWCI2gKimM)

The above video shows a more stabilized version of the gyroscope data using the complementary filter. We see in the video that there is less drift and also less overall noise, making it more accurate and less noisy compared to the non-filtered data.

# Sample Data
{: .fs-7 }

In order to speed up the data collection, I removed/commented out all the Serial print statements. Additionally, according to the outputted data we found, we can sample the data at around every 3 milliseconds because our sampling frequency was approximately 357. However, now after we optimized the code to increase the sampling rate, we find that we now have a rate of 368 Hz, which is closer to every 2.7 milliseconds. The main loop does run faster than the IMU can get data, which is why we only get data when it is ready in the code. 

The following image shows the time stamped arrays:

![](lab2pics/data.png)

I stored each of the data in separate arrays because it made indexing much simpler as well as sending code to the computer over bluetooth. Additionally, since high precision is not too necessary, we can use floats rather than doubles to store the data. Since the Artemis has 384000 bytes of memory available in the best case, and if each float in the array takes 4 bytes, we can take a total of 32000 floats of data. Since we access 6 floats of data or 24 bytes in total for the time, roll_a, pitch_a, roll_g, pitch_g, yaw_g roughly every 3 milliseconds, this makes it about 16 seconds in total data collecting.

We then have the over 5 seconds of IMU data:

![](lab2pics/5roll.png)

![](lab2pics/5pitch.png)

# Record a Stunt
{: .fs-7 }

[![](https://img.youtube.com/vi/XcVR9ekMiic/0.jpg)](https://www.youtube.com/watch?v=XcVR9ekMiic)

One observation I noticed while driving the car around is that it is extremely easy to flip it over by just going back and forth due to the high speeds that the car goes at by default.