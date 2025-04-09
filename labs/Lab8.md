---
title: Lab 8
nav_order: 2
layout: default
---

# Lab 8
{: .fs-9 }

The purpose of this lab was to implement either the drift stunt or the flip stunt onto the robot. I chose to do the drift stunt.

# Drift Implementation
{: .fs-7 }

I implemented the drift stunt using the following below command, which uses open loop instead of any sort of closed loop control, however, I was still able to achieve satisfactory results with just open loop:

![](lab8pics/code1.png)

![](lab8pics/code2.png)

After collecting the data within the previous steps I then sent to data over bluetooth using the BLE string characteristic within the same command. The code is below:

![](lab8pics/code3.png)

On the Jupyter Notebook side, I used a notification handler and send command similar to previous labs, and the code can be found in the below image:

![](lab8pics/ble.png)

Below are the videos of my car performing the stunts three separate times:

[![](https://img.youtube.com/vi/CQQRZ9hs81o/0.jpg)](https://www.youtube.com/watch?v=CQQRZ9hs81o)

[![](https://img.youtube.com/vi/s1sxOK9T_2E/0.jpg)](https://www.youtube.com/watch?v=s1sxOK9T_2E)

[![](https://img.youtube.com/vi/rTmCJ-qvzvQ/0.jpg)](https://www.youtube.com/watch?v=rTmCJ-qvzvQ)

The following are the corresponding graphs associated with the videos. Additionally, since I used open loop PWM control, the PWM inputs are going to the be the same graph for all three videos.

![](lab8pics/dist1.png)

![](lab8pics/dist2.png)

![](lab8pics/dist3.png)

![](lab8pics/PWM.png)