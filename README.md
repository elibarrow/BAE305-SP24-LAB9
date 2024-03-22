# Lab Report 9 - Take Control: The PID Feedback Controller
Eli Barrow & Isaac Stevens

March 22, 2024

## Summary of Lab ##

The main goal of this lab was to learn how to implement PID control in an Arduino circuit to allow the robot to maintain a set distance from a wall. When the wall is moved farther away, the robot will move forward to maintain the distance and vice versa. To achieve this goal, we created a program to allow the program to sense the distance with an ultrasonic sensor and for the robot to automatically adjust the robot's distance. The robot is also the same robot we created in Lab 6 of BAE 310.

For Part 1 of the lab, we used the same robot we assembled in Lab 6. Next, we opened Arduino IDE installed the PID_V2 Library, and included it in the code. Modify the code to define the setpoint, measurement, and output, and initialize the PID control and loop function. Lastly write the setpoint, measurement, and output values to the serial port to verify the operation. For Part 2 of the lab, we will implement the PID controller into the robot movement so the robot will maintain the desired distance from an object. Write a function to indicate the robot to move back or forward depending on the measured distance and tune the system by modifying Kp, Ki, and Kd. We did not complete part 3 of the lab due to it being to difficult.


### Lab Objectives: ###
1. Download the PID and become familiar with it
2. Use the PID to control the robot distance from a vertical surface

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

This lab picks up and uses the robot that had been built in the previous lab. Any circuits or schematics can be found in the corresponding repository on my Github account.

### Part 1 - PID USE ###
First, open the Arduino IDE and install the PID_V2 Library. Open the Library Manager (Tools>ManageLibraries) search for PID_V2 and install the latest library by Brett Beauregard.   
Next, open the robot sketch and modify it according to the following steps:
1. Include the library (#include<PID_V2.h>)
2. Define the setpoint, measurement, output, Kp, Ki, and Kd variables as type double.
3. Before the setup function create the PID instance:
   PID myPID(&MEAS, &OUT, &setpoint, Kp, Ki, Kd, DIRECT);
   NOTE: The measurement output and setpoint have an (&) before because these variables are pointers.
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

4. Within the setup function initialize the PID as follows:
   myPID.SetTunings(Kp, Ki, Kd);
   myPID.SetMode(AUTOMATIC);

    ```c++
   myPID.SetTunings(Kp, Ki, Kd);
   myPID.SetMode(AUTOMATIC);
   ```
* Code Display 2: This code should be inserted in the setup function in your IDE file
    

 5. Within the loop function run the PID after obtaining the distance. This line will compute the output of the PID according to the tuning and inputs, and write the output to its pointer.
   ```c++

    myPID.Compute();

   ```
* Code Display 3: This code should be inserted in the loop function in your IDE file


  6.  Wrtie the setpoint, measurement, and output values to the serial port to verify the operation.
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



## Final IDE Code  ##

```c++

#include <PID_v2.h>


 





//the right motor will be controlled by the motor A pins on the motor driver
const int AIN1 = 13;           //control pin 1 on the motor driver for the right motor
const int AIN2 = 12;            //control pin 2 on the motor driver for the right motor
const int PWMA = 11;            //speed control pin on the motor driver for the right motor

//the left motor will be controlled by the motor B pins on the motor driver
const int PWMB = 10;           //speed control pin on the motor driver for the left motor
const int BIN2 = 9;           //control pin 2 on the motor driver for the left motor
const int BIN1 = 8;           //control pin 1 on the motor driver for the left motor

int switchPin = 7;             //switch to turn the robot on and off

const int driveTime = 20;      //this is the number of milliseconds that it takes the robot to drive 1 inch
                               //it is set so that if you tell the robot to drive forward 25 units, the robot drives about 25 inches

const int turnTime = 8;        //this is the number of milliseconds that it takes to turn the robot 1 degree
                               //it is set so that if you tell the robot to turn right 90 units, the robot turns about 90 degrees

                               //Note: these numbers will vary a little bit based on how you mount your motors, the friction of the
                               //surface that you're driving on, and fluctuations in the power to the motors.
                               //You can change the driveTime and turnTime to make them more accurate

String botDirection;           //the direction that the robot will drive in (this changes which direction the two motors spin in)
String speed;               //the speed to travel in each direction
int motorSpeed = 0;
int stop = 0;

const int trigPin = 2;  
const int echoPin = 3; 

float duration, distance; 


double MEAS = 0;
double OUT = 0;
double setpoint = 15;
double Kp = 10;
double Ki = 1;
double Kd = 0;

PID myPID(&MEAS, &OUT, &setpoint, Kp, Ki, Kd, DIRECT);


/********************************************************************************/
void setup()
{
  pinMode(switchPin, INPUT_PULLUP);   //set this as a pull-up to sense whether the switch is flipped

  //set the motor control pins as outputs
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);

  pinMode(BIN1, OUTPUT);
  pinMode(BIN2, OUTPUT);
  pinMode(PWMB, OUTPUT);

  pinMode(trigPin, OUTPUT);  
	pinMode(echoPin, INPUT);  



  Serial.begin(9600);           //begin serial communication with the computer

  //prompt the user to enter a command
  Serial.println("Enter a direction followed by a speed.");
  Serial.println("f = forward, b = backward, r = turn right, l = turn left");
  Serial.println("Example command: f 50");

myPID.SetTunings(Kp, Ki, Kd);
myPID.SetMode(AUTOMATIC);  



}

/********************************************************************************/
void loop()
{

digitalWrite(trigPin, LOW);  
	delayMicroseconds(2);  
	digitalWrite(trigPin, HIGH);  
	delayMicroseconds(10);  
	digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);

  distance = (duration*.0343)/2;

  MEAS = distance;

  myPID.Compute();

  motorSpeed = map(OUT, 0, 255, 150, 255);


  Serial.print(MEAS);
  Serial.print(",");
  Serial.print(OUT);
  Serial.print(",");
  Serial.println(setpoint);
  


  //Serial.print("Distance: ");  
	//Serial.println(distance);  
	//delay(1000); 
// 0 - 255 is the values that the PID will read


  if (true)
  {                           
                              //if the switch is in the ON position
    if (true)                         //if the user has sent a command to the RedBoard
    {
      //botDirection = Serial.readStringUntil(' ');       //read the characters in the command until you reach the first space
      //speed = Serial.readStringUntil(' ');           //read the characters in the command until you reach the second space
      //motorSpeed = speed.toInt();

      //print the command that was just received in the serial monitor
      //Serial.print(botDirection);
      //Serial.print(" ");
  
     // Serial.println(motorSpeed);

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
      }

      

      
      if (botDirection == "f")                         //if the entered direction is forward
      {
        rightMotor(motorSpeed);                                //drive the right wheel forward
        leftMotor(motorSpeed);                                 //drive the left wheel forwaRD
        //delay( 100000);            //drive the motors long enough travel the entered speed
        //rightMotor(0);                                  //turn the right motor off
        //leftMotor(0);                                   //turn the left motor off
      }
      else if (botDirection == "b")                    //if the entered direction is backward
      {
        rightMotor(-motorSpeed);                               //drive the right wheel forward
        leftMotor(-motorSpeed);                                //drive the left wheel forward
       // delay(driveTime * speed.toInt());            //drive the motors long enough travel the entered speed
        //rightMotor(0);                                  //turn the right motor off
        //leftMotor(0);                                   //turn the left motor off
      }
      else if (botDirection == "r")                     //if the entered direction is right
      {
        rightMotor(-motorSpeed);                               //drive the right wheel forward
        leftMotor(255);                                 //drive the left wheel forward
        //delay(turnTime * speed.toInt());             //drive the motors long enough turn the entered speed
       //rightMotor(0);                                  //turn the right motor off
       // leftMotor(0);                                   //turn the left motor off
      }
      else if (botDirection == "l")                   //if the entered direction is left
      {
        rightMotor(255);                                //drive the right wheel forward
        leftMotor(-motorSpeed);                                //drive the left wheel forward
        //delay(turnTime * speed.toInt());             //drive the motors long enough turn the entered speed
       // rightMotor(0);                                  //turn the right motor off
       // leftMotor(0);                                   //turn the left motor off
      }
      
      else if (botDirection == "s")                   //if the entered direction is left
      {
        rightMotor(0);                                //drive the right wheel forward
        leftMotor(0);                                //drive the left wheel forward
        //delay(turnTime * speed.toInt());             //drive the motors long enough turn the entered speed
       // rightMotor(0);                                  //turn the right motor off
       // leftMotor(0);                                   //turn the left motor off
      }
    }
  }
  else
  {
    rightMotor(0);                                  //turn the right motor off
    leftMotor(0);                                   //turn the left motor off
  }
}


/********************************************************************************/



void rightMotor(int motorSpeed)                       //function for driving the right motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(AIN1, HIGH);                         //set pin 1 to high
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMA, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/



void leftMotor(int motorSpeed)                        //function for driving the left motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(BIN1, HIGH);                         //set pin 1 to high
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMB, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}



```
