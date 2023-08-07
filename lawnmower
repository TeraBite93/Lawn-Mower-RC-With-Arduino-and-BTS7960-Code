// pins for motor 1
#define RPWM_1 5 // define pin 3 for RPWM pin (output)
#define R_EN_1 14 // define pin 2 for R_EN pin (input)
#define R_IS_1 15 // define pin 5 for R_IS pin (output)

#define LPWM_1 6 // define pin 6 for LPWM pin (output)
#define L_EN_1 16 // define pin 7 for L_EN pin (input)
#define L_IS_1 17 // define pin 8 for L_IS pin (output)
// motor 1 pins end here

// pins for motor 2
#define RPWM_2 8 // define pin 9 for RPWM pin (output)
#define R_EN_2 18 // define pin 10 for R_EN pin (input)
#define R_IS_2 19 // define pin 12 for R_IS pin (output)

#define LPWM_2 9 // define pin 11 for LPWM pin (output)
#define L_EN_2 A0 // define pin 7 for L_EN pin (input)
#define L_IS_2 A1 // define pin 8 for L_IS pin (output)
// motor 2 pins end here

#define CW 1 //
#define CCW 0 //
#define debug 1 //

const int channelPin = 2;
const int channelPinSteering = 3; // center pin of the potentiometer for motor control

#include <RobojaxBTS7960.h>
RobojaxBTS7960 motor1(R_EN_1,RPWM_1,R_IS_1, L_EN_1,LPWM_1,L_IS_1,debug);//define motor 1 object
RobojaxBTS7960 motor2(R_EN_2,RPWM_2,R_IS_2, L_EN_2,LPWM_2,L_IS_2,debug);//define motor 2 object and the same way for other motors

void setup() {
  Serial.begin(9600);// setup Serial Monitor to display information
  motor1.begin();
  motor2.begin();   
}

void loop() {
  int channelValue = pulseIn(channelPin, HIGH);
  int channelValueSteering = pulseIn(channelPinSteering, HIGH);

  // Map the channelValue to the range of motor power (0-100%)
  int powerForward = map(channelValue, 1500, 1890, 0, 100);
  int powerReverse = map(channelValue, 1400, 1020, 0, 100);

  // Constrain the power values to stay within the valid range
  powerForward = constrain(powerForward, 0, 100);
  powerReverse = constrain(powerReverse, 0, 100);

  // Controllo della sterzata in base al valore letto nel canale 3
  if (channelValueSteering >= 1030 && channelValueSteering < 1400) {
    // Sterzata a sinistra
    int steeringPowerLeft = map(channelValueSteering, 1400, 1030, 0, 100);
    steeringPowerLeft = constrain(steeringPowerLeft, 0, 100);

    // Riduci la velocità del motore di sinistra e incrementa la velocità del motore di destra
    motor1.rotate(powerForward - steeringPowerLeft, CW);
    motor2.rotate(powerForward + steeringPowerLeft, CW);
  } else if (channelValueSteering >= 1500 && channelValueSteering <= 1890) {
    // Sterzata a destra
    int steeringPowerRight = map(channelValueSteering, 1500, 1890, 0, 100);
    steeringPowerRight = constrain(steeringPowerRight, 0, 100);

    // Riduci la velocità del motore di destra e incrementa la velocità del motore di sinistra
    motor1.rotate(powerForward + steeringPowerRight, CW);
    motor2.rotate(powerForward - steeringPowerRight, CW);
  } else {
    // Nessuna sterzata, entrambi i motori vanno alla stessa velocità (avanti o indietro)
    motor1.rotate(powerForward, CW);
    motor2.rotate(powerForward, CW);
  }

  Serial.print("Channel Value: ");
  Serial.print(channelValue);
  Serial.print(" | Power Forward: ");
  Serial.print(powerForward);
  Serial.print(" | Power Reverse: ");
  Serial.println(powerReverse);
  delay(10);
}


