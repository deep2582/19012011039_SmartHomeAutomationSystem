
# Smart Home Automation System

We see many people in our lives facing many problems such as sometimes they can’t use some of 
the systems because they are not familiar with these systems. Sometimes they misunderstand the 
system so they will hate all of these technology because a small part that they have missed.
Specially the oldest people in our society and the disabled ones.
From that point we had the idea of this project which can easily help them going further and further 
using the technology and passing their problems.
Our project is basically taking all commands from these users and apply it immediately. It also 
supported by many things that can be helpful for them to handle their problems.
No more hard work and hard move to catch any electronic power on the house, because our 
application can control and handle these appliances from any smart device.
Home automation systems are basically used to handle the house’s electronic powers such as 
lights,fans and so on. All these appliances will be handled throw the smart device that will be 
connected with the ESP8266 using Wi-Fi connection depends on how the user would like it to be. 


## Acknowledgements

This satisfaction that successful completion of any task would be incomplete without the 
mention of people whose ceaseless cooperation made it possible, whose constant guidance 
and encouragement crown all efforts with success. We are grateful to our guide Prof. 
Pravesh S. Patel for the guidance, inspiration and constructive suggestions that helpful us 
in the preparation of this project. We also thank our colleagues who have helped in successful 
completion of the project.
Regards,
Vatsal Bhavsar
(19012011003)
Dip Patel
(19012011039)
## Reference

[1]How to add DHT library in arduino IDE - https://randomnerdtutorials.com/complete-guide-for-dht11dht22-humidity-and-temperature-sensor-with-arduino/
 [2]How to connect ESP8266 with blynk - https://docs.blynk.io/en/legacy-platform/legacy-articles/nodemcu
[3] Blynk application - https://play.google.com/store/apps/details?id=cloud.blynk&hl=en_IN&gl=US&pli=1## Source code

#include <DHT.h>
#include <DHT_U.h>
#include <Servo.h>
#define DHTPIN D5
#define Led D2
#define FAN D1
//#define GASPIN D3
#define PW D6
#define Lock D4
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <WiFiUdp.h>
#include <NTPClient.h>
#include <BlynkSimpleEsp8266.h>
float humDHT = 0;
float tempDHT = 0;
int val = 32;
#define BLYNK_TEMPLATE_ID "TMPLHV4LUmBv"
#define BLYNK_DEVICE_NAME "Smart Home"
#define BLYNK_AUTH_TOKEN "ciF6757VJ-d2kiuWO9bFCHd0-jcUXnjq"
#define BLYNK_PRINT Serial
Servo servo;
Servo servo1;
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Redmi 9";//Enter your WIFI name
char pass[] = "Vatsal@8811";//Enter your WIFI password
DHT dht(DHTPIN, DHT11);
BlynkTimer timer;
int sth;
int stm;
int sts;
int sh;
int sm;
int ss;
const long utcOffsetInSeconds = 19800;
WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, "pool.ntp.org", utcOffsetInSeconds);
BLYNK_WRITE(V0)
{
 int va = param.asInt();
 digitalWrite(Led,va);
}
BLYNK_WRITE(V7)
{
 int v = param.asInt();
 digitalWrite(FAN,v);
}
BLYNK_WRITE(V11)
{
 if(param.asInt()==1)
 {
 digitalWrite(PW,HIGH);
 }
 else
 {
 digitalWrite(PW,LOW);
 }
}
BLYNK_WRITE(V12)
{
 digitalWrite(Lock,param.asInt());
}
BLYNK_WRITE(V13){
if(param.asInt()==1)
 {
 servo.write(180);
 }
 else
 {
 servo.write(0);
 } 
 
}
BLYNK_WRITE(V14){
 if(param.asInt()==1)
 {
 servo1.write(20);
 }
 else
 {
 servo1.write(200);
 }
}
BLYNK_WRITE(V5) {
 TimeInputParam t(param);
 // Process start time
 if (t.hasStartTime())
 {
 Serial.println(String("Start: ") +
 t.getStartHour() + ":" +
 t.getStartMinute() + ":" +
 t.getStartSecond());
 sth=t.getStartHour();
 stm=t.getStartMinute();
 sts=t.getStartSecond();
 }
 // Process stop time
 if (t.hasStopTime())
 {
 Serial.println(String("Stop: ") +
 t.getStopHour() + ":" +
 t.getStopMinute() + ":" +
 t.getStopSecond());
 sh=t.getStopHour();
 sm=t.getStopMinute();
 ss=t.getStopSecond();
 }
 Serial.println();
}
void setup() {
 Serial.begin(115200);
 servo.attach(D8);
 servo1.attach(D3);
 pinMode(Led, OUTPUT);
 pinMode(FAN, OUTPUT);
 pinMode(Lock, OUTPUT);
 pinMode(PW, OUTPUT);
 //pinMode(GASPIN,INPUT);
 /*pinMode(PIRPIN, INPUT);
 pinMode(FLAME, INPUT);
 pinMode(22, OUTPUT);*/
 digitalWrite(Led,LOW);
 Serial.println(F("DHT11 test!"));
 dht.begin();
 //Initialize the Blynk library
 Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
}
void loop() {
 //Run the Blynk library
 Blynk.run();
 //Timer
 timeClient.update();
 if(timeClient.getHours()==sth && timeClient.getMinutes()==stm && 
timeClient.getSeconds()==sts){
 //Serial.println("Start");
 digitalWrite(Led, HIGH);
 digitalWrite(22, HIGH);
 }
 if(timeClient.getHours()==sh && timeClient.getMinutes()==sm && 
timeClient.getSeconds()==ss){
 //Serial.println("Stop");
 digitalWrite(Led, LOW);
 digitalWrite(22, LOW);
 }
 //Temperature
 humDHT = dht.readHumidity();
 // Read temperature as Celsius (the default)
 tempDHT = dht.readTemperature();
 // Check if any reads failed and exit early (to try again).
 if (isnan(humDHT) || isnan(tempDHT))
 {
 //Serial.println("Failed to read from DHT sensor!");
 return;
 }
 Serial.print(F("Temperature: "));
 Serial.print(tempDHT);
 Serial.print(F("°C "));
 Serial.println();
 Serial.print(F("Humidity: "));
 Serial.print(humDHT);
 Serial.print(F("%"));
 Serial.println();
 
 Serial.println("***********************");
 Serial.println();
 if (tempDHT > val)
 {
 digitalWrite(FAN,HIGH);
 }
 if(tempDHT==32)
 {
 digitalWrite(FAN,LOW);
 }
 //Gas
 //int gas = digitalRead(GASPIN);
 //gas = map(gas, 0,4095,0,100);
 //Serial.println(gas);
 //Flame
 //int out = digitalRead(FLAME);
 
 Blynk.virtualWrite(V1, tempDHT);
 Blynk.virtualWrite(V2, humDHT);
}
