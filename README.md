# CarND-Term3-P1-Path-Planning-Project  
Self-Driving Car Engineer Nanodegree Program Term 3 Project 1  
## Overview  
In this project you'll implement a path planning algorithm to drive the ego car around the track safely with other traffic is driving +-10 MPH of the 50 MPH speed limit. You will need to read the localization and sensor fusion data, then determine your ego car's behavior (lane keeping, lane shifting). Your ego car should pass the slower traffic when possible, avoid collision and drive inside of the marked road lanes except shifting lane. Finally, your car should be able to make one complete loop around the 6946m highway. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop.  
There is a simulator provided by Udacity ([Term 3 Simulator Release](https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2)) which will sends your ego car's telemetry information and other cars' sensor fusion data to your path planning algorithm. You will be using those data to determine your ego car's behaviour and generate your path then send it back to the simulator to drive the vehicle. It expects a set of points spaced in time at 0.02 seconds representing the car's trajectory.  
Here is the link to the [orginal repository](https://github.com/udacity/CarND-Path-Planning-Project) provided by Udaciy. This repository contains all the code needed to complete the path planning project for the path planning course in Udacity's Self-Driving Car Nanodegree.  
## Prerequisites/Dependencies  
* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
## Setup Instructions (abbreviated)  
1. Meet the `Prerequisites/Dependencies`  
2. Intall `uWebSocketIO ` on your system  
  2.1 Windows Installation  
  2.1.1 Use latest version of Ubuntu Bash 16.04 on Windows 10, here is the [step-by-step guide](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) for setting up the utility.  
  2.1.2 (Optional) Check your version of Ubuntu Bash [here](https://www.howtogeek.com/278152/how-to-update-the-windows-bash-shell/).  
3. Open Ubuntu Bash and clone the project repository  
4. On the command line execute `./install-ubuntu.sh`  
5. Build and run your code.  
Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)  
## Project Description
- [main.cpp](./src/main.cpp): Setup a target velocity and max acceleration for ego car to run, reads in sensor fusion data, determine and predict each car's future location, decide behavior (lane keeping, lane shifting) of ego car, generate and interpolate waypoints with a spline, push and plot on the road for ego car to follow.  
- [spline.h](./src/spline.h): simple cubic spline interpolation library without external dependencies.  
- [README.md](./README.md): Writeup for this project, including setup, running instructions and project rubric addressing.  
- [CMakeLists.txt](./CMakeLists.txt): `CMakeLists.txt` file that will be used when compiling your code (you do not need to change this file).  
- [highway_map.txt](./data/highway_map.txt): The map of the highway, each waypoint in the list contains [x, y, s, dx, dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop. The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.  
## Run the project  
1. Clone this repo.  
2. Make a build directory: `mkdir build && cd build`  
3. Compile: `cmake .. && make`  
4. Run it: `./path_planning`.  
## Tips
A really helpful resource for doing this project and creating smooth trajectories was using http://kluge.in-chemnitz.de/opensource/spline/, the spline function is in a single hearder file is really easy to use.  
## Code Style  
Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).
* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)    
## Project Rubric  
### 1. Compilation  
#### 1.1 The code compiles correctly..  
Yes, it does.  
### 2. Valid Trajectories  
#### 2.1 The car is able to drive at least 4.32 miles without incident..  
Yes, it does.  
#### 2.2 The car drives according to the speed limit.  
Yes, it does.  
#### 2.3 Max Acceleration and Jerk are not Exceeded.  
Yes, it does.  
#### 2.4 Car does not have collisions.  
Yes, it does.  
#### 2.5 The car stays in its lane, except for the time between changing lanes.  
Yes, it does.  
#### 2.6 The car is able to change lanes  
Yes, it does.  
### 3. Reflection  
#### 3.1 There is a reflection on how to generate paths.  
Path generation is handled at [`main.cpp` Line407-516](./src/main.cpp#L407-L516).  
1. We will use the last two points from previous trajectory as starting point at [`main.cpp` Line416-449](./src/main.cpp#L416-L449).  
2. We will add another three points(30m, 60m, 90m) ahead of the current ego car location at [`main.cpp` Line451-462](./src/main.cpp#L451-L462).  
3. We will convert those points from Frenet coordinates to local car coordinates at [`main.cpp` Line464-471](./src/main.cpp#L464-L471).  
4. We will push last two points to the new path for smooth changing at [`main.cpp` Line483-487](./src/main.cpp#L483-L487).  
5. We will calculate the spline points based on the given target x distance at [`main.cpp` Line489-492](./src/main.cpp#L489-L492).  
6. We will generate 50 points and rotate those points back to normal then push them to the new path at [`main.cpp` Line496-516](./src/main.cpp#L496-L516).  
## Videos
Video recordings for success cases.  
Success to run a full track.  
![Success to run a full track](./videos/Success_to_run_a_full_track.gif)  