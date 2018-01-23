# Extended Kalman Filter

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

*made by [CJ](https://github.com/vssrcj)*

# Overview.

This project utilizes a kalman filter to estimate the state of a moving object of interest with noisy lidar and radar measurements.

# How to run.

This project involves the Term 2 Simulator which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases)

This repository includes two files that can be used to set up and install [uWebSocketIO](https://github.com/uWebSockets/uWebSockets) for either Linux or Mac systems. For windows you can use either Docker, VMware, or even [Windows 10 Bash on Ubuntu](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) to install uWebSocketIO. Please see [this concept in the classroom](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/16cf4a78-4fc7-49e1-8621-3450ca938b77) for the required version and installation scripts.

Once the install for uWebSocketIO is complete, the main program can be built and run by doing the following from the project top directory.

1. mkdir build
2. cd build
3. cmake ..
4. make
5. ./ExtendedKF

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)


### Other Important Dependencies.

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

### Basic Build Instructions.

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make` 
   * On windows, you may need to run: `cmake .. -G "Unix Makefiles" && make`
4. Run it: `./ExtendedKF `

The following should be the result:
```
Listening to port 4567
Connected!!!
```

# Result.

<div>
  <img src="/images/program.png" height="500">
</div>

The program achieves the following RMSE score on Dataset 1:

||Achieved|Required|
|---|---|---|
|X|0.10|0.11|
|Y|0.09|0.11|
|VX|0.45|0.52|
|VY|0.44|0.52|

# Pipeline.

The program performs the following steps:

### 1. Sensor Measurements.
The simulator runs and provides both RADAR and LASAR measurements.

### 2. Data Collection.

**main.cpp** captures these measurements using *uWebsocketIO*.

### 3. Initialization.

A **FusionEKF** instance is created, and the initialization matrices are set up.

### 4. Predictinon and Updating.

**ProcessMeasurement()** is called within **FusionEKF.cpp** which initializes the Kalman filter as well as calling the prediction and update steps of the Kalman filter, located in **kalman_filter.cpp**.

### 5. Calculate RMSE

**tools.cpp** calculates the RMSE and passes it back to **main.cpp**.

# Measurement Process.

<div>
  <img src="/images/flow.png" height="500">
</div>

*Simply put:*

* The sensors give data about the car's surroundings.  The **RADAR** provides a velocity measurement, while the **LIDAR** has a higher resolution.
* The EKF (Extended Kalman Filter) is an algorithm that uses these series of measurements over time to produce predictions.
* Firstly, the EKF is initialized.
* When new data is received, the sensor type is determined.
* The EKF then predicts where the location of the object will be after time Δt.
* The state of the object is then updated based on the prediction and the measurement.  The EKF will put more weight on either the prediction or the measurement based on their respective uncertainties.