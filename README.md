# Building an Estimator Project #

## Implement Estimator ##

This project is to develop an estimator to be used by a controller to successfully fly a desired flight path using realistic sensors.  This project is built on top of the control project to use sensor data from GPS, IMU and Magnetometer to estimate the current position of the drone.  Following section explains how each rubric item was addressed and shows the output for each step.

### Determine the standard deviation of the measurement noise of both GPS X data and Accelerometer X data. ###

After running the scenario `06_NoisySensor`, data from the graphs were recorded in the csv files [./config/log/Graph1.txt](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/config/log/Graph1.txt) (GPS X data) and [./config/log/Graph2.txt](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/config/log/Graph2.txt) (Accelerometer X data).  These files were processed to figure out the standard deviation of the GPS X signal and the IMU Accelerometer X signal and these calculated values were plugged in [./config/06_SensorNoise.txt:L3-L4](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/config/06_SensorNoise.txt#L3-L4) for `MeasuredStdDev_GPSPosXY` and `MeasuredStdDev_AccelXY`.  After implementing these values, below image shows that its capturing 68% of the respective measurements.


![./animations/Scenario_1.gif](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/animations/Scenario_1.gif).

### Implement a better rate gyro attitude integration scheme in the UpdateFromIMU() function. ###

`Simulation #3 (../config/07_AttitudeEstimation.txt)
PASS: ABS(Quad.Est.E.MaxEuler) was less than 0.100000 for at least 3.000000 seconds
Simulation #4 (../config/07_AttitudeEstimation.txt)
PASS: ABS(Quad.Est.E.MaxEuler) was less than 0.100000 for at least 3.000000 seconds`

### Implement all of the elements of the prediction step for the estimator. ###

### Implement the magnetometer update. ###

### Implement the GPS update. ###
