# Major-Project
Smart air bag system for fault protection

#include <math.h>

const int x_out = 39; 
const int y_out = 34; 
const int z_out = 35; 
int buzzer=33;
int relay=32;

#include <WiFi.h>
#include <EMailSender.h>
#include "Arduino.h"

uint8_t connection_state = 0;
uint16_t reconnect_interval = 10000;

EMailSender emailSend("madhugd6361@gmail.com", "grcmdnbewrksloam");

const char* ssid = "Redmi";  // Enter SSID here
const char* password = "12345678";  //Enter Password here


void sendmail()
{
  EMailSender::EMailMessage message;
  message.subject = "Alert!";
  message.message = "Fall Detected";

  EMailSender::Response resp = emailSend.send("impanab22@gmail.com", message);

  Serial.println("Sending status: ");

  Serial.println(resp.status);//1
  Serial.println(resp.code);//0
//  int c = resp.status.toInt();
  if(resp.status)
  {

     Serial.print("Mail Sent");
     delay(5000);    
  }
  else
  {

     Serial.println("Error in sending mail, respnse code: ");
     Serial.print(resp.code);
     delay(5000);
  }
  Serial.println(resp.desc);
  
}







void setup(void){
  Serial.begin(9600);   
   WiFi.begin(ssid, password); 
  while (WiFi.status() != WL_CONNECTED) {
  delay(1000);
  Serial.print(".");
  
  }
  
  Serial.println("WiFi connected..!");
  pinMode(relay,OUTPUT);
  pinMode(buzzer,OUTPUT);

  delay(2000);             
}

void loop() {
  int x_adc_value, y_adc_value, z_adc_value; 
  
  x_adc_value = analogRead(x_out); 
  y_adc_value = analogRead(y_out); 
  z_adc_value = analogRead(z_out); 
  Serial.print("x = ");
  Serial.print(x_adc_value);
  Serial.print("\t\t");
  Serial.print("y = ");
  Serial.print(y_adc_value);
  Serial.print("\t\t");
  Serial.print("z = ");
  Serial.print(z_adc_value);
  Serial.println("");
  delay(1000);




if((x_adc_value>1800) && (x_adc_value<2000) &&(y_adc_value>1800)&&(y_adc_value<2000)){
  Serial.println("NO");
  digitalWrite(relay,0);
}


else{
  Serial.println("Fall");
  digitalWrite(relay,1);
  tone(buzzer,2000);
  
  
  sendmail();
  noTone(buzzer);
}
}




youtube video link:-
https://youtu.be/wtDJ367H9SI
