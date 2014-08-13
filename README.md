GY85PRO :: ITG3205, ADXL345, and HMC5883L
=======

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

