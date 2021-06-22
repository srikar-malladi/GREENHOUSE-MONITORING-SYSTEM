# GREENHOUSE-MONITORING-SYSTEM
MAJOR PROJECT CODE


#include <Arduino.h>
#include<Servo.h>
int servoPin = 3;
Servo Servo1;
#include "MQ135.h"
MQ135 gasSensor = MQ135(A0);
int val;
int sensorPin = A1;
int sensorValue = 0;
#define moist_sense A0
int output_val;
int pump = 2;
#include "dht11.h"
#define dht_apin A2 // Analog Pin sensor is connected to
dht11 DHT;
int fan=4;
#define ldr A3
int light = 5;
void setup() {
// put your setup code here, to run once:
Serial.begin(9600);
pinMode(sensorPin,INPUT);
Servo1.attach(servoPin);
pinMode(moist_sense,OUTPUT);
pinMode(pump,OUTPUT);
pinMode(fan, OUTPUT);
pinMode(ldr , INPUT);
pinMode(light , OUTPUT);
}
void loop() {
// put your main code here, to run repeatedly:
val = analogRead(A0);
Serial.print ("raw = ");
Serial.println (val);
float ppm = gasSensor.getPPM();
Serial.print ("ppm: ");
Serial.println (ppm);
if(ppm < 100)
{
Servo1.write(90);
delay(1000);
Servo1.write(0);
delay(1000);
}
else if(ppm > 130)
{
Servo1.write(0);
delay(1000);
}
int moisture= analogRead(moist_sense);
Serial.println(moisture);
int moisture_p = map(moisture,550,0,0,100);
Serial.print("Mositure : ");
Serial.print(moisture_p);
Serial.println("%");
delay(1000);
if(moisture_p <=30)
{
digitalWrite(pump,HIGH);
delay(500);
}
else if(moisture_p >= 70)
{
digitalWrite(pump,LOW);
delay(500);
}
DHT.read(dht_apin);
Serial.print("Current humidity = ");
Serial.print(DHT.humidity);
Serial.print("% ");
Serial.print("temperature = ");
Serial.print(DHT.temperature);
Serial.println("C ");
int temperature = DHT.temperature;
if(temperature > 20)
{
digitalWrite(fan,HIGH);
Serial.println("FAN HIGH");
delay(2000);
}
else if(temperature < 30)
{
digitalWrite(fan,LOW);
}
delay(2000);
int day_light = analogRead(ldr);
Serial.println(day_light);
if(day_light <1000)
{
digitalWrite(light,HIGH);
}
else if(day_light > 1000)
{
digitalWrite(light,LOW);
}
}
