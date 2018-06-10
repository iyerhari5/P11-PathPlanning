## Running the Code
This project involves the Term 3 Simulator which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2)

This repository includes two files that can be used to set up and install uWebSocketIO for either Linux or Mac systems. For windows you can use either Docker, VMware, or even Windows 10 Bash on Ubuntu to install uWebSocketIO.

Once the install for uWebSocketIO is complete, the main program can be built and ran by doing the following from the project top directory.

1. mkdir build
2. cd build
3. cmake ..
4. make
5. ./path_planning

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

[image1]: ./Images/Miles-463.png
[image2]: ./Images/Miles-1026.png
[image3]: ./Images/Miles-1405.png
[image4]: ./Images/Miles-1668.png
[image5]: ./Images/Miles-1846.png
[image6]: ./Images/Miles-2003.png

# Path Planning
The crux of the path planning project implementation is in the main.cpp file. The important sections are Prediction, Behavior, 
and Trajectory Generation.


# Prediction
The prediction part of the algorithm happens in lines 257 to 304 of main.cpp. In this section, we iterate through each of the other cars that we
have information about (from sensor fusion) and then make a decision about whether there is a car in front of our car, whether there is a car that is
nearby and to the left of our car (which will prevent any left lane changes) and if there is a car that is nearby and to the right of our car (which 
would prevent any right lane changes)

# Behavior
The behavior part of the code is in lines 307 to 334 of main.cpp. In this section, we decide what is the next action our car will take based on the
prediction module. The three behaviors of interest are if a car is ahead of us and if it is safe to do so, make a lane change either to the left or the
right. If there is a car ahead of us, but it is not safe to make a lane change, we will reduce the velocity so as to not crash into the vehicle
in front. If there is no car in front of us, and we are going slower than the allowed maximum speed, then we accelerate so as to reach the target
max velocity. But this is done in small steps, so that the jerk is within safe limits. So the maximum acceleration allowed is 5 m/s2.

# Trajectory Generation
In this module based on the selected behavior, we generate the trajectory of how the car needs to move. This is done in lines 338 to 446 of main.cpp.
The main inputs here are the target lane, the target speed and the past path points of the car.  We first include the last two points of the previous
trajectory are added to the current path. This is done so that there is continuity in the trajectories. Then we get three points at 30,60 and 90 m
from the current position. These points are then used as input to a spline fitting to generate the trajectory points. In order to make the spline
calculation simpler, we also transform the coordinates of the generated points to the local car coordinates.

The simulator does not use all the points from the past trajectory at each step. So in order to maintain the continuity we first add all the 
past points from the previous step then we fill the result of the points (for a maximum of 50 points) using the spline calculation as described in the
previous paragraph. The points calculated from the spline are transformed back to the global coordinates before being added to the path points.

# Results
The car was able to successfully drive around the track for many miles. There were no accidents or out of lane events detected by the simulator.
The following images show snapshots of the car at various points in time.

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


