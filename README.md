# arduino
The aim of the project is to develop an Alcohol detection system with vehicle controlling using arduino uno and sensors.
For this we will be using arduino uno,mq3 gas sensor,motor driver,lcd to display.

#include <SoftwareSerial.h>
#include <LiquidCrystal.h>

SoftwareSerial gsm(9, 10);
const int rs = 7, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

int AOUTpin=A0;
int value;
int flag=0;
int flag2=0;
int buzzer=8;
int led=6;
const int inputPin1  = 12;
const int inputPin2  = 13;


void setup() {
    gsm.begin(9600);
    Serial.begin(9600);
    pinMode(AOUTpin, INPUT);
    lcd.begin(16, 2);
    lcd.cursor();
    lcd.print("ACCI- PREVENTION");
    pinMode(buzzer,OUTPUT);
    pinMode(led,OUTPUT);
    pinMode(inputPin1, OUTPUT);
    pinMode(inputPin2, OUTPUT);
    digitalWrite(inputPin1, HIGH);
    digitalWrite(inputPin2, LOW);
}

void loop() {
    value= analogRead(AOUTpin);
    Serial.print(" Alcohol value: ");
    Serial.println(value);
    delay(1000);
    digitalWrite(led,HIGH);
    //lcd.display();
    if (value>=300) {
        flag+=1;flag2+=1;
        digitalWrite(led,LOW);
        lcd.begin(16, 2);
        lcd.display();
        digitalWrite(buzzer,HIGH);
        lcd.print("Stopping Engine");
        delay(10000);
        if(flag==1){
            gsm.println("AT+CMGF=1");
            delay(1000);
            gsm.println("AT+CMGS=\"+91xxxxxxxxxx\"\r"); //mobile number where sms is to be sent.
            delay(1000);
            gsm.println("Your ward has been caught drunk driving");
            delay(100);
            gsm.println((char)26);
            delay(1000);
        } 
        if(flag2==1){
            digitalWrite(inputPin1, LOW);
            digitalWrite(inputPin2, LOW);
        }
    }
    digitalWrite(buzzer,LOW);
}
