# Lab Report 9 - Take Control: The PID Feedback Controller
Eli Barrow & Isaac Stevens

March 22, 2024

## Summary of Lab ##




### Lab Objectives: ###


## Lab Assignment Specific Items ##

* A Computer running Arduino IDE, and a Browser
* A smartphone running Android OS with the MIT AI2 Companion app installed
* SparkFun Inventor's Kit
  - RedBoard
  - Ultrasonic Sensor
  - Two motors
  - Motor Driver
  - Battery Pack
* A cable/adapter to connect the smartphone to the RedBoard
* An HC-05 Bluetooth UART Module


## Methods and Testing ##
### Part 1 - PID USE ###
First open the Arduino IDE and install the PID_V2 Library. Open the Library Manager (Tools>ManageLibraries) and search for PID_V2 and install the latest library by BRett Beauregard.   
Next open the robot sketch and modify it according to the following steps:
1. Include the library (#include<PID_V2.h>)
2. Define the setpoint, measurement, output, Kp, Ki and Kd variables as type double.
3. Before the setup function create the PID instance:
   PID myPID(&MEAS, &OUT, &setpoint, Kp, Ki, Kd, DIRECT);
   NOTE: The measurementm output, and setpoint have an (&) before because these variables are pointers.
   ```c++
    #include <PID_v2.h>
   
   double MEAS = 0;
    double OUT = 0;
    double setpoint = 15;
    double Kp = 10;
    double Ki = 1;
    double Kd = 0;
   
   PID myPID(&MEAS, &OUT, &setpoint, Kp, Ki, Kd, DIRECT);
   ```
   * Code Display 1: This code should be inserted before the setup function in your IDE file.   

4. Within the setup function intialize the PID as follows:
   myPID.SetTunings(Kp, Ki, Kd);
   myPID.SetMode(AUTOMATIC);

    ```c++
   myPID.SetTunings(Kp, Ki, Kd);
   myPID.SetMode(AUTOMATIC);
   ```

 6. Within



### Part 2 - Develop the App ###




## Results ##

## Discussion Questions ##

## Conclusion of Lab 8 ##
