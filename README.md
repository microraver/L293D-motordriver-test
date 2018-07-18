//  L293D motordriver test 2 motor bi-directional, variable speed
//  with LCD readout

//  This sketch was written July 16 microraverBUILDS(2018)
//  with lots of help from the Arduino community.
//  This code is completely free for any use.


// Use the Software Serial library to create a new "soft" serial port
// for the display. This prevents display corruption when uploading code.
#include <SoftwareSerial.h>

// Attach the serial enabld LCD's RX line to digital pin 11
SoftwareSerial LCD(12,11); // Arduino SS_RX = pin 12 (unused), Arduino SS_TX = pin 11

//define the input pins
const int switchPin1 = 3;
const int switchPin2 = 6;
const int switchPin3 = 9;
const int switchPin4 = 13;
const int potPin1 = 0;
const int potPin2 = 1;

// if you need PWM, just use the PWM outputs on the Arduino
// and instead of digitalWrite, you should use the analogWrite command

const int PWM1 = 5; 
const int PWM2 = 10;

// --------------------------------------------------------------------------- Motors
int motor1[] = {2, 4};
int motor2[] = {7, 8};

// --------------------------------------------------------------------------- Setup
void setup() {
Serial.begin(9600);

// Setup motors
int i;
for(i = 0; i < 2; i++){
pinMode(motor1[i], OUTPUT);
pinMode(motor2[i], OUTPUT);
}

//set the PWM1 and PWM2 as OUTPUT

 pinMode(PWM1, OUTPUT);
 pinMode(PWM2, OUTPUT);
 
  //set the switchPins as INPUT
  
  pinMode(switchPin1, INPUT);
  pinMode(switchPin2, INPUT);
  
  LCD.begin(9600); // set up serial port for 9600 baud
  delay(1000); // wait for display to boot up

  // move cursor to beginning of first line
  LCD.write(254); 
  LCD.write(128);

  // clear display
  LCD.write(0x0FE); 
  LCD.write(0x01);

  // blinking cursor toggle on
  LCD.write(0xFE);
  LCD.write(0x0D);
  LCD.write("L293D driver");
  delay(500);
  // move cursor to beginning of second line
  LCD.write(254);
  LCD.write(192);
  LCD.write("control test");
  
  delay(2500);
  
  LCD.write(254);
  LCD.write(192);
  LCD.write("FWD-RVS-STP-SPD");
  
  delay(2500);
  
// blinking cursor toggle off
  LCD.write(0xFE);
  LCD.write(0x0C);
}

// --------------------------------------------------------------------------- Loop
void loop() { 

//read the value from the potentiometer and divide
//it by 4 to get a 0-255 range. Store the value in
//the speed variable

int speed1 = analogRead(potPin1) / 4;
int speed2 = analogRead(potPin2) / 4;

  LCD.write(254); 
  LCD.write(128);
  LCD.write(0xFE);
  LCD.write(0x01);
  LCD.write("MOTOR 1  MOTOR 2");
  LCD.write(254);
  LCD.write(196);
  LCD.print(speed1);
  
  LCD.write(254);
  LCD.write(205);
  LCD.print(speed2);

// read direction for MOTOR1

if (digitalRead(switchPin3) == LOW)
{
    LCD.write(254);
    LCD.write(192);
    LCD.write("FWD");
}
else
 {
    LCD.write(254);
    LCD.write(192);
    LCD.write("REV");
 }

// read direction for MOTOR2

if (digitalRead(switchPin4) == LOW)
{
    LCD.write(254);
    LCD.write(201);
    LCD.write("FWD");
}
else
 {
    LCD.write(254);
    LCD.write(201);
    LCD.write("REV");
 }
 
  delay(500);

//read the value of the switchPin3 and switchPin4 and store it in the
//direction variable for MOTOR1 and MOTOR2
//if the value of direction is HIGH drive forward at
//a speed set by the speed variable, else drive reverse
//at a speed set by the speed variable.

if (digitalRead(switchPin3) == LOW)
{
  forward1(speed1);
}
else
 {
 reverse1(speed1);
 }

 if (digitalRead(switchPin4) == LOW)
{
  forward2(speed2);
}
else
 {
 reverse2(speed2);
 }

// read the value of switchPin1 and switchPin2
// if the value is LOW stop either motor

if (digitalRead(switchPin1) == LOW)
{
  stop1();

    LCD.write(254);
    LCD.write(192);
    LCD.write("STP");
    delay(500);
    
}

 if (digitalRead(switchPin2) == LOW)
{
  stop2();
  
    LCD.write(254);
    LCD.write(201);
    LCD.write("STP");
    delay(500);
}

}

  
// --------------------------------------------------------------------------- Drive
//create a custom function for motor1 that defines moving forward
//the forward() function accepts one parameter and that is
//the speed at which you want to drive forward (0-255)

 void forward1(int spd)
 {
 //motor contoller direction pins set to forward
 digitalWrite(motor1[0], HIGH);
 digitalWrite(motor1[1], LOW);

  //write the speed by using the parameter of spd
 analogWrite(PWM1, spd);
  }

//create a custom function for motor1 that defines moving in reverse
//the reverse() function accepts one parameter and that is
//the speed at which you want to drive in reverse (0-255)

void reverse1(int spd)
{ 
//set motor controller pins to reverse
digitalWrite(motor1[0], LOW);
digitalWrite(motor1[1], HIGH);

 //write the speed by using the parameter of spd
 analogWrite(PWM1, spd);
 }
 
//create a custom function for motor2 that defines moving forward
//the forward() function accepts one parameter and that is
//the speed at which you want to drive forward (0-255)

 void forward2(int spd)
 {
 //motor contoller direction pins set to forward
 digitalWrite(motor2[0], HIGH);
 digitalWrite(motor2[1], LOW);

  //write the speed by using the parameter of spd
 analogWrite(PWM2, spd);
  }

//create a custom function for motor1 that defines moving in reverse
//the reverse() function accepts one parameter and that is
//the speed at which you want to drive in reverse (0-255)

void reverse2(int spd)
{ 
//set motor controller pins to reverse
digitalWrite(motor2[0], LOW);
digitalWrite(motor2[1], HIGH);

 //write the speed by using the parameter of spd
 analogWrite(PWM2, spd);
 }

 //create a custom function for motor1 that defines stop

void stop1()
{ 
//set motor controller pins to stop
digitalWrite(motor1[0], LOW);
digitalWrite(motor1[1], LOW);


 }
 
void stop2()
{ 
//set motor controller pins to stop
digitalWrite(motor2[0], LOW);
digitalWrite(motor2[1], LOW);


 }

