# Building an Estimator Project #

## Implement Estimator ##

This project is to develop an estimator to be used by a controller to successfully fly a desired flight path using realistic sensors.  This project is built on top of the control project to use sensor data from GPS, IMU and Magnetometer to estimate the current position of the drone.  Following section explains how each rubric item was addressed and shows the output for each step.

### Determine the standard deviation of the measurement noise of both GPS X data and Accelerometer X data. ###

After running the scenario `06_NoisySensor`, data from the graphs were recorded in the csv files [./config/log/Graph1.txt](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/config/log/Graph1.txt) (GPS X data) and [./config/log/Graph2.txt](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/config/log/Graph2.txt) (Accelerometer X data).  These files were processed to figure out the standard deviation of the GPS X signal and the IMU Accelerometer X signal and these calculated values were plugged in [./config/06_SensorNoise.txt:L3-L4](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/config/06_SensorNoise.txt#L3-L4) for `MeasuredStdDev_GPSPosXY` and `MeasuredStdDev_AccelXY`.  After implementing these values, below image shows that its capturing 68% of the respective measurements.

**Simulation #9 (../config/06_SensorNoise.txt)
PASS: ABS(Quad.GPS.X-Quad.Pos.X) was less than MeasuredStdDev_GPSPosXY for 71% of the time
PASS: ABS(Quad.IMU.AX-0.000000) was less than MeasuredStdDev_AccelXY for 68% of the time**

![./animations/Scenario_6.gif](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/animations/Scenario_6.gif).

### Implement a better rate gyro attitude integration scheme in the UpdateFromIMU() function. ###

After implementing improved integration scheme at [./src/QuadEstimatorEKF.cpp:L100-L109](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp#L100-L109), as shown below the altitude estimator was able to get within 0.1 rad for each of the Euler angles for at least 3 seconds.

**Simulation #3 (../config/07_AttitudeEstimation.txt)
PASS: ABS(Quad.Est.E.MaxEuler) was less than 0.100000 for at least 3.000000 seconds

![./animations/Scenario_7.gif](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/animations/Scenario_7.gif).

### Implement all of the elements of the prediction step for the estimator. ###

1. State prediction step is implemented in the `PredictState()` funtion at [./src/QuadEstimatorEKF.cpp:L172-L180](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp#L172-L180).
2. Partial derivative of the body-to-global rotation matrix is calculated in the function `GetRbgPrime()` at [./src/QuadEstimatorEKF.cpp:L209-L211](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp#L172-L180). 
3. Rest of prediction step is implemented in function `Predict()` at [./src/QuadEstimatorEKF.cpp:L257:L263](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp#L257-L263).

### Implement the magnetometer update. ###

After implementing the magnetometer update in the function `UpdateFromMag()` [./src/QuadEstimatorEKF.cpp:L317-L323](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp#L317-L323) and tuning the `QYawStd` in `QuadEstimatorEKF.txt`, as shown below the estimated standard deviation captures the error and maintains an error less than 0.1rad in heading for at least 10 seconds of the simulation.

**Simulation #2 (../config/10_MagUpdate.txt)
PASS: ABS(Quad.Est.E.Yaw) was less than 0.120000 for at least 10.000000 seconds
PASS: ABS(Quad.Est.E.Yaw-0.000000) was less than Quad.Est.S.Yaw for 59% of the time**

![./animations/Scenario_10.gif](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/animations/Scenario_10.gif).

### Implement the GPS update. ###

After implementing the function `UpdateFromGPS()` [./src/QuadEstimatorEKF.cpp:L291-L295](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/src/QuadEstimatorEKF.cpp#L291-L295) and tuning the process noise model in `QuadEstimatorEKF.txt`, following image shows the simulation was able to pass the test.

**Simulation #5 (../config/11_GPSUpdate.txt)
PASS: ABS(Quad.Est.E.Pos) was less than 1.000000 for at least 20.000000 seconds**

![./animations/Scenario_11.gif](https://github.com/psaravind/FCND-Estimation-CPP/blob/master/animations/Scenario_11.gif).

### Meet the performance criteria of each step. ###

As shown above the estimator was able to successfully meet the performance criteria with the controller code provided for the project.

### De-tune the controller. ###

After replacing `QuadController.cpp` and `QuadControlParams.txt` and re-tuning the parameters, entire siulation cycle was completed with estimated position error of <1m.