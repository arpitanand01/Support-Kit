![image](https://github.com/user-attachments/assets/96dd11b2-7b17-4d02-a943-6f9f3f93c1df)<br/>



#include<LiquidCrystal.h><br/>

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;<br/>
LiquidCrystal lcd1(rs, en, d4, d5, d6, d7); <br/>
float celsius;<br/>
int temp = A1;<br/>
int sensorReading = 0;<br/>
// alarm<br/>
int h=0,m=0,s=0;<br/>
int alarmH = 0, alarmMin = 1, alarmSec = 1;<br/>
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);<br/>
const int buzzer = 10;<br/>

//code for button and buzzer<br/>
int buttonPin = 7; //inform ide button pada pin 2<br/>
int buzPin = 8; //inform ide buzzer pada pin 8<br/>
int buttonState = 0;<br/>


void setup(){<br/>
pinMode(temp,INPUT);<br/>
// for flex sensor and buzzer<br/>
pinMode(A0, INPUT);<br/>
Serial.begin(9600);<br/>
pinMode(9, OUTPUT);<br/>
//button and buzzer<br/>
pinMode(buzPin , OUTPUT); // buzzer pin 8 OUTPUT<br/>
pinMode (buttonPin , INPUT); // Button pin 7 INPUT
//alarm<br/>
lcd.begin(16, 2);<br/>
pinMode(buzzer, OUTPUT);<br/>
}<br/>


void loop(){<br/>
celsius = analogRead(temp)*0.004882814;<br/>
celsius = (celsius - 0.5) * 100.0;<br/>
lcd.setCursor(0,1);<br/>
lcd.print("Temp: ");<br/>
lcd.print(celsius);<br/>
lcd.print(" C");<br/>
delay(1000);<br/>
lcd.clear();<br/>
// read the sensor for flex sensor<br/>
sensorReading = analogRead(A0);<br/>


// map the sensor reading to a range for the<br/>
// flex speaker<br/>
  if(sensorReading<139){<br/>
  tone(9, 440 * pow(2.0, (constrain(int(map(sensorReading, 0, 1023, 36, 84)), 35, 127) - 57) / 12.0), 1000);<br/>
  delay(10); // Delay a little bit to improve simulation performance<br/>
  }<br/>

// call button buzzer <br/>
    buttonState = digitalRead (buttonPin); <br/>
  if ( buttonState ==HIGH ){ //when button = HIGH<br/>
  digitalWrite(buzPin , HIGH); //OUTPUT = HIGH<br/>
   tone(buzPin,500);<br/>
    delay(100);//buzzer delay<br/>
    digitalWrite(buzPin , LOW);<br/>
    noTone(buzPin);<br/>
    //delay(100);<br/>
  }<br/>
  else {<br/>
    digitalWrite (buzPin , LOW ); //button= LOW<br/>
  }<br/>

  delay(500);<br/>
  
  // alarm<br/>
   s=s+1;<br/>
  if(s==60){<br/>
    m=m+1;<br/>
    s=0;<br/>
  }<br/>
  
  if(m==60){<br/>
    m=0;<br/>
    h=h+1;<br/>
  }<br/>
  
  lcd.print("HOURS=");<br/>
  lcd.print(h);<br/>
  lcd.setCursor(10,0);<br/>
  lcd.print("MIN=");<br/>
  lcd.print(m);<br/>
  lcd.setCursor(0,1);  <br/>
  lcd.print("SEC=");<br/>
  lcd.print(s);<br/>
  delay(1000);<br/>
  lcd.clear();<br/>
  
  //(h == alarmH) &&  for hour<br/>
  if((m == alarmMin) && (alarmSec == s)){<br/>
    tone(buzzer, 500); <br/>
    delay(1000);       <br/>
    noTone(buzzer);     <br/>
     
  }<br/>
}<br/>
