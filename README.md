# SimplePilot
The most simple autopilot code for multicopters based on arduino


To understand how The Simple Pilot works visit this page:

https://mozanunal.com/2017/02/multikopterler-icin-otopilot-yapmak/

# Autopilot for Multicopter (Copied from the above link)

Hi friends,
I would like to tell you about my experience on making autopilots which I spent a lot of time on in my previous article , but which I have spent a long time on. I would like to thank Bahadir Gokceaslan, who did all of these works before I started writing . 

Yes, autopilot was actually a bit too broad concept compared to the part I'm going to tell. Unfortunately, the project remained in the flight controller stages. Let's start with a question. What is a flight controller? A flight controller is basically a set of sensors and moving mechanisms that ensure that the angles of a vehicle flying in the air are the desired shape. In fact it is not very logical to limit the angles to the location. It is also the job of ensuring that the height of an aircraft is constant or that it remains in the desired position at the desired speed. However, in this project, a quadcopter flight controller was designed as the purpose of the quadcopter in this structure according to the location of the control of the angle was to control the input.

So how are these IMU angles checked? As you all know, a quadcopter is a mechanism consisting of 4 engines that push the ground. But keeping a quadcopteri in the air is not as easy as it seems. The simple operation is to control the speed of the quadcopter motors at the desired angle. For this, 2 basic structures are used. These sensors called **IMU** are sensors and **PID** called control algorithm. At this point, if these concepts did not sound familiar, I suggest you read [this article about the imu](https://mozanunal.com/2014/11/imu-aclarnn-3-boyutlu-olarak/) and read [this article about the pid](https://mozanunal.com/2015/07/multikopterler-icin-pid-kontrol/) . If you examine the code we have divided our project into specific parts. These parts are:

![](https://cloud.githubusercontent.com/assets/13440502/11090387/13c23d1c-887a-11e5-93fd-7ed0b2ac84db.png)


* Receiving inputs from the controller
* Update IMU angles
* Calculating the required engine speeds with pid-picking to achieve input values
* Sending of telemetry data

I'll focus on article 3 more in this article. Let's take control of an axis first. The procedure for checking a axis is as follows:

![](https://raw.githubusercontent.com/imuworks/SimplePilot/master/Documents/PID%20Structure.png)


* The current angle from Imu is taken from the control and the desired angle for that axis is taken. (In an autopilot structure, calculates the desired angle to go to the given coordinate.)
* Current angle error
* Sum of angle error in the past
* Estimation of future angle error
* The top 3 uses **the required angular velocity for the target angle**
* Current angular velocity error
* The sum of the angular velocity error in the past
* Future angular error
* Using the top 3, the **pid output** for that axis is calculated

In this way, it is necessary to apply a 2-pit pid operation to control an axis. So how to use a solution for 3 axes. I would like to add the following 4 lines of code here instead. (Assume that those starting with output_ are pid output related to the relevant axis.)

![](https://raw.githubusercontent.com/imuworks/SimplePilot/master/Documents/Formulas.png)

With these two lines, the calculations generated from the total 6 pid controllers are combined. You have noticed the logic of the signs during the merge. Is the pid effect for that axis negative or positive? In other words, the speed of the engine increases in that direction.

Well, finally, let me talk about our project. There have been open-source autopilots for many years, and we have started this project to improve ourselves even though we are aware of this. Unfortunately, we were unable to devote time to the test setups and the development of the project. But, as I said, I think we have gained valuable experience. Maybe one day. 
Link to all of our work: [https://github.com/mozanunal/SimplePilot](https://github.com/mozanunal/SimplePilot)

Since we usually use arduino libraries, I think that our code is not suitable for performance. But [the report](https://blog.owenson.me/build-your-own-quadcopter-flight-controller/) explaining the basis for the event using ArduPilot libraries have a friend has revealed a great article. I think that if you want to do something similar, you can also reference it as a code. I recommend you to review. See you again.
