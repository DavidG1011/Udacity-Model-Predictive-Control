[//]: # (Image References)

[image1]: ./imgs/equ.PNG


# Udacity Model Predictive Control Project.

Self-Driving Car Engineer Nanodegree Program

The rubric for this project is located --> [Here](https://review.udacity.com/#!/rubrics/896/view).

---

## Model Type and Equations

The goal is to navigate a car around a track using the Udacity term 2 simulator. The model must keep the car within the drivable portion of the track while accounting for a 100 ms delay between model outputs and simulator response. This delay is to emulate a more real-world scenario.

This program uses a kinematic model to calculate the t + 1 state vector from the current state vector. The model state uses x and y for vehicle coordinates, psi for orientation angle, v for velocity, cte for cross track error, and epsi for psi error. 

The actuators for the model (steering - acceleration/braking) are respectively delta and a. 
This model uses the IPOPT and CPPAD libraries to calculate the lowest error trajectory and return the actuations needed to accomplish it.

The update equations for state variables are as follows:

![alt text][image1]


## N and dt

The duration of future predictions in the model can be chosen by adjusting the N and dt variables. The prediction horizon-- or T, is simply the product of N * dt. My model in an optimized state uses:

N = 7
dt = 0.15

T = 7 * 0.15 = 1.05

I arrived at these paramaters by trial and error, the most scientific of all methods. The initial values chosen were 15 and 0.25, based on lecture exercise numbers that I halved to be more responsive for a real time model. This proved to be too unresponsive, likely due to latency compensation compounding itself into instability. Simply put, a T of 3.75 was too high. I slowly widdled the number down from there to be more responsive with the model:

* 15, 0.25  T =  3.75 -  Too high - Oscillated out of control.
* 12, 0.2   T =  2.4  -  Better - A bit less wobbly, still dangerous.
* 10, 0.2   T =  1.5  -  Good - Would have kept, but sometimes got close to curb.
* 7,  0.15  T =  1.05 -  Best - Drives a clean line with no issues.


## Preprocessing

The waypoint coordinates are transformed from global to vehicle perspective to simplify polyfitting.

## Latency

The main way latency is curbed is by keeping a low T value, which allows the system to still give relevant actuator input for the time frame needed. Supporting this are scalars, which highly penalizes cte and epsi to overcompensate for bigger errors. Even with delay, these keep the car in the optimal trajectory.  


---


## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
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
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Project Instructions and Rubric

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.
