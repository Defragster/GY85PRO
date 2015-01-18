GY85PRO :: ITG3205, ADXL345, and HMC5883L
=======

http://digistump.com/wiki/digispark/tutorials/9dof

#include <Wire.h>
#include <DigiKeyboard.h>
#define ADXL345                (0x53) //address of Accelerometer
#define ADXL345_X              (0x32) //register for X value from Accelerometer
#define ADXL345_Y              (0x34) //register for Y value from Accelerometer
#define ADXL345_Z              (0x36) //register for Z value from Accelerometer
#define HMC5883                (0x1E) //address of Magnetometer
#define ITG3200                (0x68) //address of Gyro
int16_t magX = 0; //X value from Magnetometer
int16_t magY = 0; //Y value from Magnetometer
int16_t magZ = 0; //Z value from Magnetometer
int gyroHX = 0; //HX value from Gyro
int gyroHY = 0; //HY value from Gyro
int gyroHZ = 0; //HZ value from Gyro
void setup()
{
    Wire.begin();
 
    initAccelerometer();
 
    initMagnetometer(); 
 
    initGyro();
 
    DigiKeyboard.delay(1000);    // Open Notepad (or similar) and make sure it has the focus
 
}
 
 
void loop()
{
 
  DigiKeyboard.println(readAccelerometer(ADXL345_X));
  DigiKeyboard.println(readAccelerometer(ADXL345_Y));
  DigiKeyboard.println(readAccelerometer(ADXL345_Z));
  DigiKeyboard.delay(1000);
  readMagnetometer();
  DigiKeyboard.println(magX);
  DigiKeyboard.println(magY);
  DigiKeyboard.println(magZ);
  DigiKeyboard.println(getHeading());
  DigiKeyboard.delay(1000);
  readGyro();
  DigiKeyboard.println(gyroHX);
  DigiKeyboard.println(gyroHY);
  DigiKeyboard.println(gyroHZ);
  DigiKeyboard.delay(1000);
 
 
 
}
 
 
 
void initGyro(){
  writeRegister(ITG3200, 0x3E, 0x00); //enable
  writeRegister(ITG3200, 0x15, 0x07); // EB, 50, 80, 7F, DE, 23, 20, FF
  writeRegister(ITG3200, 0x16, 0x1E); // +/- 2000 dgrs/sec, 1KHz, 1E, 19
  writeRegister(ITG3200, 0x17, 0x00);
}
 
void readGyro(){
 Wire.beginTransmission(ITG3200);
 Wire.write(0x1B);  //format x y z temp
 Wire.endTransmission();
 Wire.requestFrom(ITG3200, (byte)8);
 
  // Wait around until enough data is available
 while (Wire.available() < 8);
 uint8_t lo = Wire.read();
 uint8_t hi = Wire.read();
 //throw out first two - don't seem accurate
 //gyroTemp = (lo << 8) | hi; // temperature
 //gyroTemp = ((double) (gyroTemp + 13200)) / 280;
 lo = Wire.read();
 hi = Wire.read();
 gyroHX = (((lo << 8) | hi) + 120 )/ 14.375;
 lo = Wire.read();
 hi = Wire.read();
 gyroHY = (((lo << 8) | hi) + 20 )/ 14.375;
 lo = Wire.read();
 hi = Wire.read();
 gyroHZ = (((lo << 8) | hi) + 93 )/ 14.375;
 
}
 
void initMagnetometer(){
  writeRegister(HMC5883, 0x02, 0x00); //enable
}
 
 
void readMagnetometer(){
  Wire.beginTransmission(HMC5883);
  Wire.write(0x03);  //format X_H_M 
  Wire.endTransmission();
  Wire.requestFrom(HMC5883, (byte)6);
 
  // Wait around until enough data is available
  while (Wire.available() < 6);
 
  // Note high before low (different than accel)  
  uint8_t hi = Wire.read();
  uint8_t lo = Wire.read();
  // Shift values to create properly formed integer (low byte first)
  magX = (int16_t)(hi | ((int16_t)lo << 8));
  // Note high before low (different than accel)  
  hi = Wire.read();
  lo = Wire.read();
  // Shift values to create properly formed integer (low byte first)
  magY = (int16_t)(hi | ((int16_t)lo << 8));
  // Note high before low (different than accel)  
  hi = Wire.read();
  lo = Wire.read();
  // Shift values to create properly formed integer (low byte first)
  magZ = (int16_t)(hi | ((int16_t)lo << 8));  
}
 
void initAccelerometer(){
  //if(readRegister(ADXL345,0x00) != 0xE5)
  writeRegister(ADXL345, 0x2D, 0x08); //power up and enable measurements
  writeRegister(ADXL345, 0x31, 0x01);//set range to +/-4G
}
 
uint16_t readAccelerometer(uint8_t reg){
    Wire.beginTransmission(ADXL345);
    Wire.write(reg);
    Wire.endTransmission();
    Wire.requestFrom(ADXL345, 2);
    return (uint16_t)(Wire.read() | (Wire.read() << 8));  
}
 
uint8_t readRegister(uint8_t address, uint8_t reg){
      Wire.beginTransmission(address);
      Wire.write(reg);
      Wire.endTransmission();
      Wire.requestFrom(address, 1);
      return Wire.read();
    }
void writeRegister(uint8_t address, uint8_t reg, uint8_t val){
      Wire.beginTransmission(address);
      Wire.write(reg);
      Wire.write(val);
      Wire.endTransmission();
    }
 
float getHeading(){
    // Hold the module so that Z is pointing 'up' and you can measure the heading with x&y
  // Calculate heading when the magnetometer is level, then correct for signs of axis.
  float heading = atan2(magY, magX);
  // Correct for when signs are reversed.
  if(heading < 0)
    heading += 2*PI;
  // Check for wrap due to addition of declination.
  if(heading > 2*PI)
    heading -= 2*PI;
  // Convert radians to degrees for readability.
  heading = heading * 180/M_PI; 
  return heading;
}


DigiSpark Pro and GY85 Library

From: Erik On Behalf Of Digistump Support
Sent: Thursday, July 31, 2014 2:18 AM
To: Defragster   Cc: Digistump Support ( support@digistump.com )
No library in mind yet for the 9 DOF - ITG3205, ADXL345, and HMC5883L will be the chips on it.


Since the GY-85 uses practically the same three chips as the Sparkfun SEN-10724 
https://www.sparkfun.com/products/10724
(except ITG-3205 for GY-85 vs. ITG-3200 for SEN-10724), 
I'd think the code can be adopted straight away or with minor modifications.
  9 Degrees of Freedom - Sensor Stick
  SEN-10724
Description: The 9DOF Sensor stick is a very small sensor board with 9 degrees of freedom. 
It includes the ADXL345 accelerometer, the HMC5883L magnetometer, and the ITG-3200 MEMS gyro.
The ‘stick’ has a simple I2C interface and a mounting hole for attaching it to your project. 
Also, the board is a mere 0.036" thick (0.093" overall), allowing it to be easily mounted in just about any application.

https://github.com/ptrbrtz/razor-9dof-ahrs/wiki/Tutorial
•Clone or download and unzip the latest Razor AHRS Firmware from GitHub.
  https://github.com/ptrbrtz/razor-9dof-ahrs
•Download and install the Arduino Software. 
  http://arduino.cc/en/Main/Software
  http://forum.arduino.cc/index.php/topic,164222.0.html : Arduino Nano V3.0 + GY-85, reading, combining, outputting 
  http://forum.arduino.cc/index.php/topic,156436.0.html : GY_85 9DOF ITG3205 + ADXL345 + HMC5883L Help! 
We will use it to upload the firmware and calibrate the sensors. Any Arduino versions 1.x should work fine, tested with version 1.0.5.

I read that the ITG3200 and the ITG3205 can be used the same way. :: « Reply #1 on: March 25, 2013, 03:14:59 pm »
 from :   http://forum.arduino.cc/index.php/topic,156436.0.html : GY_85 9DOF ITG3205 + ADXL345 + HMC5883L Help!
If they are alike, you can use the libraries by Jeff Rowberg, http://www.i2cdevlib.com/
I'm sure his libraries will work together.
  http://www.i2cdevlib.com/devices/adxl345  : ADXL345 3-axis accelerometer
  http://www.i2cdevlib.com/devices/hmc5883l : HMC5883L 3-axis magnetometer
  http://www.i2cdevlib.com/devices/itg3200  : ITG-3200 3-axis gyroscope

