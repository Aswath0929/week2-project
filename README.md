# week2-project
PROJECT-ROBOTIC ARM
BILL OF MATERIALS
| Item           | Cost / Notes        |
| -------------- | ------------------- |
| SERVO          | Rs 150              | 2
| MPU6050        | Rs 400              |
| Arms and Table | To be printed in TL |

TOTAL- 700
Basic visualisation of our robotic arm
![WhatsApp Image 2025-05-25 at 05 33 43_9b663ca4](https://github.com/user-attachments/assets/203e494f-7578-4e78-b83a-a799d16ff5f6)

CIRCUIT

![image](https://github.com/user-attachments/assets/82ea6451-0f95-43a2-bf41-7e486dfc49a6)


SIMULATION

CODE
<pre>'''cpp 
#include <Wire.h>
#include <Servo.h>

Servo rollServo;
Servo pitchServo;

const int MPU = 0x68;
int16_t AcX, AcY, AcZ;

void setup() {
  Wire.begin();
  Wire.beginTransmission(MPU);
  Wire.write(0x6B);  // Power management register
  Wire.write(0);
  Wire.endTransmission(true);

  Serial.begin(9600);
  rollServo.attach(9);    // X-axis servo
  pitchServo.attach(10);  // Y-axis servo
}

void loop() {
  // Request accelerometer data
  Wire.beginTransmission(MPU);
  Wire.write(0x3B);  // Starting with Accel X-out
  Wire.endTransmission(false);
  Wire.requestFrom(MPU, 6, true);

  AcX = Wire.read() << 8 | Wire.read();
  AcY = Wire.read() << 8 | Wire.read();
  AcZ = Wire.read() << 8 | Wire.read();

  float Ax = AcX / 16384.0;
  float Ay = AcY / 16384.0;
  float Az = AcZ / 16384.0;

  // Compute roll and pitch in degrees
  float roll  = atan2(Ay, Az) * 180.0 / PI;
  float pitch = atan2(-Ax, sqrt(Ay * Ay + Az * Az)) * 180.0 / PI;

  // Map angles to servo values
  int rollAngle  = map(roll,  -90, 90, 0, 180);
  int pitchAngle = map(pitch, -90, 90, 0, 180);

  // Set servo positions
  rollServo.write(rollAngle);
  pitchServo.write(pitchAngle);

  delay(20);  // Smooth motion
}
</pre>







