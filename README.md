# Lab Report 9 - Take Control: The PID Feedback Controller
Eli Barrow & Isaac Stevens

March 22, 2024

## Summary of Lab ##




### Lab Objectives: ###
1. Download the PID and become familiar with it
2. Use the PID to contorl the robot distance from a vertical surface

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
* Code Display 2: This code should be inserted in the setup function in your IDE file
    

 6. Within the loop function run the PID after obtaining the distance. This line will compute the output of the PID according to the tuning and inputs, and write the output to its pointer.
   ```c++

    myPID.Compute();

   ```
* Code Display 3: This code should be inserted in the loop function in your IDE file


  7.  Wrtie the setpoint, measurement, and output values to the serial port to verify the operation.
  ```c++
  Serial.print(MEAS);
  Serial.print(",");
  Serial.print(OUT);
  Serial.print(",");
  Serial.println(setpoint);
   ```

  * Code Display 4: This code should be inserted in the loop function in your IDE file and it will display the values in the serial communications.

### Part 2 - Keep your distance ###

In this section we implemented the PID controller into the robot movement so that given an object in front, the robot will move forward or backwards to maintain the desired distance.

1. We wrote a function to tell the robot to move back or forward depending on the measured distance. The PID library will return an output value from 0 to 255. We implement a function that obtains this output and moves forward or backward and with a fast or slow speed.
   ```c++

    duration = pulseIn(echoPin, HIGH);
    distance = (duration*.0343)/2;
    MEAS = distance;
    myPID.Compute();
    motorSpeed = map(OUT, 0, 255, 150, 255)

   
    if(true)
      {
          //motorSpeed = OUT;


          if (MEAS > setpoint)
          {
          botDirection = "f";
          }
          else 
          {
          botDirection = "b";
       }

   ```
 * Code Display 5: This code will read the value of "MEAS" and if it is greater than or less that the "setpoint" it will accuratley move the robot forwards or backwards.

After demonstrating this to Dr. Jarro we then modified Kp, Ki and Kd to test our system even further to make it more accurate.

### Part 3 - Wall Follower ###

We did not do this in lab.

## Results ##



## Conclusion of Lab 8 ##
