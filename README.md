![image](https://github.com/user-attachments/assets/96dd11b2-7b17-4d02-a943-6f9f3f93c1df)

#include<LiquidCrystal.h><br/>

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;<br/>
LiquidCrystal lcd1(rs, en, d4, d5, d6, d7); <br/>
float celsius;<br/>
int temp = A1;<br/>
int sensorReading = 0;<br/>
// alarm
int h=0,m=0,s=0;<br/>
int alarmH = 0, alarmMin = 1, alarmSec = 1;<br/>
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);<br/>
const int buzzer = 10;<br/>

//code for button and buzzer
int buttonPin = 7; //inform ide button pada pin 2<br/>
int buzPin = 8; //inform ide buzzer pada pin 8<br/>
int buttonState = 0;<br/>


void setup(){<br/>
pinMode(temp,INPUT);<br/>
// for flex sensor and buzzer
pinMode(A0, INPUT);<br/>
Serial.begin(9600);<br/>
pinMode(9, OUTPUT);<br/>
//button and buzzer
pinMode(buzPin , OUTPUT);<br/> // buzzer pin 8 OUTPUT
pinMode (buttonPin , INPUT);<br/> // Button pin 7 INPUT
//alarm
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
lcd.clear();
// read the sensor for flex sensor
sensorReading = analogRead(A0);


// map the sensor reading to a range for the
// flex speaker
  if(sensorReading<139){
  tone(9, 440 * pow(2.0, (constrain(int(map(sensorReading, 0, 1023, 36, 84)), 35, 127) - 57) / 12.0), 1000);
  delay(10); // Delay a little bit to improve simulation performance
  }

// call button buzzer 
    buttonState = digitalRead (buttonPin); 
  if ( buttonState ==HIGH ){ //when button = HIGH
  digitalWrite(buzPin , HIGH); //OUTPUT = HIGH
   tone(buzPin,500);
    delay(100);//buzzer delay
    digitalWrite(buzPin , LOW);
    noTone(buzPin);
    //delay(100);
  }
  else {
    digitalWrite (buzPin , LOW ); //button= LOW
  }

  delay(500);
  
  // alarm
   s=s+1;
  if(s==60){
    m=m+1;
    s=0;
  }
  
  if(m==60){
    m=0;
    h=h+1;
  }
  
  lcd.print("HOURS=");
  lcd.print(h);
  lcd.setCursor(10,0);
  lcd.print("MIN=");
  lcd.print(m);
  lcd.setCursor(0,1);
  lcd.print("SEC=");
  lcd.print(s);
  delay(1000);
  lcd.clear();
  
  //(h == alarmH) &&  for hour
  if((m == alarmMin) && (alarmSec == s)){
    tone(buzzer, 500); 
    delay(1000);       
    noTone(buzzer);     
     
  }
}
