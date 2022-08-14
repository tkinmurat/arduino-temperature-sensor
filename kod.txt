
#include <Adafruit_GFX.h> // graphics core library
#include <Adafruit_SSD1306.h> //OLED LCD library
#include <Wire.h> //wiring
#include <Adafruit_MLX90614.h> // temperature sensor library

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels
#define OLED_RESET    -1 // Reset pin # (or -1 if sharing Arduino reset pin)

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET); //Declaring the display name (display)
Adafruit_MLX90614 mlx = Adafruit_MLX90614();

const unsigned char PROGMEM wearmask [] = {0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
                                           0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
                                           0x00, 0x00, 0x07, 0xF8, 0x00, 0x00, 0x00, 0x00, 0x3C, 0x0E, 0x00, 0x00, 
                                           0x00, 0x00, 0x60, 0x43, 0x80, 0x00, 0x00, 0x01, 0x8A, 0x10, 0xC0, 0x00, 
                                           0x00, 0x03, 0x20, 0x00, 0x70, 0x00, 0x00, 0x06, 0x00, 0x00, 0x1C, 0x00, 
                                           0x00, 0x0D, 0x00, 0x00, 0x04, 0x00, 0x00, 0x18, 0x00, 0x00, 0x04, 0x00, 
                                           0x00, 0x30, 0x90, 0x00, 0x08, 0x00, 0x00, 0x20, 0xCA, 0x00, 0x0C, 0x00, 
                                           0x00, 0x21, 0x61, 0x70, 0xC4, 0x00, 0x00, 0x61, 0x1C, 0x07, 0xC4, 0x00, 
                                           0x00, 0x43, 0x07, 0xFC, 0x66, 0x00, 0x00, 0x42, 0x00, 0x00, 0x22, 0x00, 
                                           0x00, 0x46, 0x00, 0x00, 0x22, 0x00, 0x00, 0x44, 0x00, 0x00, 0x22, 0x00, 
                                           0x00, 0x44, 0xFC, 0x3F, 0x12, 0x00, 0x00, 0x64, 0x04, 0x01, 0x36, 0x00, 
                                           0x00, 0x2C, 0x00, 0x00, 0x14, 0x00, 0x00, 0x28, 0x30, 0x0C, 0x1C, 0x00, 
                                           0x00, 0x78, 0x30, 0x06, 0x1A, 0x00, 0x00, 0x58, 0x70, 0x0E, 0x1A, 0x00, 
                                           0x00, 0x4E, 0x20, 0x04, 0x76, 0x00, 0x00, 0x6B, 0x80, 0x01, 0xD4, 0x00, 
                                           0x00, 0x28, 0xF0, 0x0F, 0x14, 0x00, 0x00, 0x28, 0x1F, 0xF8, 0x14, 0x00, 
                                           0x00, 0x38, 0x00, 0x00, 0x1C, 0x00, 0x00, 0x19, 0x00, 0x00, 0x18, 0x00, 
                                           0x00, 0x08, 0x80, 0x01, 0x10, 0x00, 0x00, 0x08, 0x28, 0x10, 0x10, 0x00, 
                                           0x00, 0x08, 0x01, 0x40, 0x10, 0x00, 0x00, 0x0D, 0x00, 0x00, 0xB0, 0x00, 
                                           0x00, 0x04, 0x40, 0x02, 0x20, 0x00, 0x00, 0x02, 0x10, 0x14, 0x40, 0x00, 
                                           0x00, 0x03, 0x01, 0x40, 0xC0, 0x00, 0x00, 0x01, 0x80, 0x01, 0x80, 0x00, 
                                           0x00, 0x00, 0xC0, 0x03, 0x00, 0x00, 0x00, 0x00, 0x30, 0x0C, 0x00, 0x00, 
                                           0x00, 0x00, 0x1E, 0x38, 0x00, 0x00, 0x00, 0x00, 0x03, 0xC0, 0x00, 0x00, 
                                           0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
                                           0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00};
                                           //a photo can be converted to bitmap format for lcd

int Button1=8; //defined buttons
int Button2=9;
int Button3=5;

int mainflag = 0; // how are you feeling page
int second_flag = 0; 

int red=2;  // defined leds
int blue=6;
int green=7;



void setup() {  


  pinMode(Button1, INPUT); 
  pinMode(Button2, INPUT);
  pinMode(Button3, INPUT); 
  
  pinMode(green,OUTPUT);  
  pinMode(blue,OUTPUT);  
  pinMode(red,OUTPUT); 
  
 
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C); // address 0x3D for 128x64
  display.clearDisplay();     
  
  display.display();
mlx.begin();      //beginning LCD and Sensor
delay(100);
}

void loop(){
float heatx= mlx.readObjectTempC();
float heat= heatx + 6.3;  //sensor improvement was made according to the distance to be measured
  
mlx.begin();
delay(100);

  if(mainflag == 0) // mainflag=0 is how are you feeling questions' page
  {
      if(digitalRead(Button1)==1){
      mainflag = 1;
     }
     if(digitalRead(Button2)==1){
      mainflag = 2;
      }
     if(digitalRead(Button3)==1){
      mainflag = 3;
     }

      display.clearDisplay();
      display.setTextSize(1);                    
      display.setTextColor(WHITE);             
      display.setCursor(0,0);  
      display.println("How are you \nfeeling today? \n\n 1 - GOOD \n\n 2 - NOT BAD \n\n 3 - BAD ");
      display.display();

    }

    
  if(mainflag == 1){ // if press button1 
    
    
    
         display.clearDisplay();
        display.setTextSize(1);                    
        display.setTextColor(WHITE);             
        display.setCursor(0,4);  
        display.println("Have a healthy day.\nLet's keep social \ndistance pls!");
    
      
      display.setTextSize(1);                    
      display.setTextColor(WHITE);             
      display.setCursor(0,47);                
      display.println("BODY \nTEMPERATURE"); 
      
      display.setTextSize(2);
      display.setCursor(50,37);
      display.println(heat,1);
      
      display.setCursor(110,37);
      display.println("C");
      display.display();

      if(heat>=34 && heat<35.5 ) { //blue led on if body temperatures are between 34 and 35.5 
      
        
      digitalWrite(blue,HIGH);
     digitalWrite(red,LOW);
     digitalWrite(green,LOW);
      
  
  }
  
  else if(heat>=35.5 && heat <=37.5  ){ //green led on if body temperatures are between 35.5 and 37.5 
    
    digitalWrite(green,HIGH);
   digitalWrite(red,LOW);
     digitalWrite(blue,LOW);
     
     
  }
  else if(heat>37.5) { //red led on if the body temperature is higher than 37.5
    
     digitalWrite(red,HIGH);
    digitalWrite(blue,LOW);
     digitalWrite(green,LOW);
     
  }


  
else  {
  
     digitalWrite(red,LOW);
     digitalWrite(green,LOW);
     digitalWrite(blue,LOW);
}


    }

if(mainflag==2|| mainflag==3) //if press button2 or button3 so, NOT BAD or BAD
  
  {
    digitalWrite(blue,HIGH);
      display.clearDisplay();
      display.setTextSize(1);                    
      display.setTextColor(WHITE);             
      display.setCursor(0,4); 
      display.println("Are you in contact\nwith someone who\nhas corona? \n\n 1-YES \n\n 2-NO ");
      display.display();
      
     if(digitalRead(Button1)==1){ //if yes, mainflag=5 and second_flag=1
      second_flag = 1;
      mainflag = 5;
      
     }
    if(digitalRead(Button2)==1){ //if NO, mainflag=5 and second_flag=2
      second_flag = 2;
      mainflag = 5;
     }
   
    }

  if (mainflag == 5) //mainflag=5 here,so the if location to be redirected according to the answer to the question are you in contact
  {
    
    if(second_flag == 2){ // if you are not in contact, redirected mainflag==1 so have a healthy day page
      mainflag = 1;     
      }
      
      else if(second_flag == 1) //if you are in contact
      {

       
        display.clearDisplay(); 
        display.setFont();
        display.setCursor(10,10);  // (x,y)
        display.drawBitmap(0,0,wearmask,48,48, 1);
        
         
        display.setTextSize(1);                    
        display.setTextColor(WHITE);             
        display.setCursor(0,4);  
        display.println("       Wear your mask          and go to          the hospital!");
       
      mlx.begin(); 
      
      display.setTextSize(1);                    
      display.setTextColor(WHITE);             
      display.setCursor(0,47);                
      display.println("BODY \nTEMPERATURE"); 
      
      display.setTextSize(2);
      display.setCursor(50,37);
      display.println(heat,1);
      
      display.setCursor(110,37);
      display.println("C");
      display.display();
      
      if(heat>=34 && heat<35.5 ) {
      digitalWrite(blue,HIGH);
     digitalWrite(red,LOW);
     digitalWrite(green,LOW);
 
  
  }
  else if(heat>=35.5 && heat<=37.5  ){
    digitalWrite(green,HIGH);
   digitalWrite(red,LOW);
     digitalWrite(blue,LOW);
     
  }
  else if(heat>37.5) {
     digitalWrite(red,HIGH);
    digitalWrite(blue,LOW);
     digitalWrite(green,LOW);
  }


  
else  {
     digitalWrite(red,LOW);
     digitalWrite(green,LOW);
     digitalWrite(blue,LOW);
}
delay(500);

       }
    }  
}