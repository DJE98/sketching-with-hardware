---
title: "Microcontroller"
date: 2022-12-13T15:45:38+01:00
---
# Theory
We learned about microcontrollers in our last two sessions. In the first theory of the first session, we got a basic introduction to microcontrollers in general and the usable sensors and actuators. In the theory part of the second session, we talked about communication like serial, I2C, and SPI.
# Control a RGB led with a joystick
![RGB with joystick](/posts/images/rgb_with_joystick.jpg)
After both theory parts, we got time to try the sensors for ourselves. My personal goal was to use the first session to try new sensors I had never used before and connect them with other parts to build mini projects. In my first project, I used a joystick to control the color of an RGB led. 
```c
int vx = 0;
int vy = 0;

void setup() {
}

void loop() {
  vx = analogRead(5);
  vy = analogRead(4);
  analogWrite(9, 0);
  analogWrite(10, vx);
  analogWrite(11, vy);
}
```
I’ve used a potentiometer and a led before, so I could see how I could do new things with my familiar knowledge.
# Seven segment display
![seven segment display](/posts/images/seven_segment_display.jpg)
In my second project, I manually controlled a seven-segment display without the use of a chip. I used an existing coding example to avoid the mapping between areas and signs. I like the idea of expressing complex signs with a combination of seven simple LEDs.
# Water sensor and other analog sensors
![water sensor](/posts/images/water_sensor.jpg)
In my last project for the first lab, I tried to print out analog values on a LCD display to test different sensors. I liked the water sensor because of its easy and effective design. There are multiple parallel contacts on the sensor. If water comes into contact with these contacts, you will know that there is water. The contacts will be connected earlier depending on the emergence of the water, and the height can thus be determined through the current resistance. 
```c
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);
//int value = 0;
void setup() {
  lcd.init(); //initialize the lcd
  lcd.backlight(); //open the backlight 
}

void loop() {
  // put your main code here, to run repeatedly:
  value = analogRead(5);
  lcd.setCursor(0,0);
  lcd.print(value);
}
```
# User interface with hardware
![user interface](/posts/images/user_interface.jpg)
In our second session, I tried to build an interface to control different parameters of my cat feeder project. Therefore, I used an OLED display for the presentation of the parameters. Also, I used a potentiometer and a button for movement action in the interface.
```c
#include "U8glib.h"
#define POTENTIOMETER_PIN 0
#define BUTTON_PIN 13
#define STATUS_MENU 0
#define QUANTITY_MENU 1
#define INTERVALL_MENU 2
#define BUTTON_TRIGGABLE 2000

U8GLIB_SH1106_128X64 u8g(U8G_I2C_OPT_NONE);
int intervall = 1;
int quantity = 25;
int fillStatus = 100;
char* intervall_description [] = {"half ", "", "second ", "third ", "forth "};
int status_menu = 0;
char starQuantity []= "[x]";
char starIntervall[] = "[ ]";
bool changed_status = false;
int last_triggered = -BUTTON_TRIGGABLE;

// Sensor functions

int get_potentiometer_degree(int pin){
  int potentiometer_range = analogRead(pin);
  int degree = (potentiometer_range / 664.0) * 330.0; // For 3.3 Voltage 
  return degree;
}

bool is_triggered(int pin){
  bool triggered = digitalRead(BUTTON_PIN) != HIGH && millis() -last_triggered >= BUTTON_TRIGGABLE;
  if (triggered){
    last_triggered = millis();
  }
  return triggered;
}

// Menu functions

void menu_header(void){
  u8g.setFont(u8g_font_profont10);
  u8g.setPrintPos(0, 10);
  u8g.print("Cat Feeder");
}

void print_day(int intervall){
  u8g.print("Every ");
  u8g.print(intervall_description[intervall]);
  u8g.print("day");
}
void menu(int status){
  switch(status){
    case STATUS_MENU:
      state_menu();
      break;
    case QUANTITY_MENU:
      quantity_menu();
      break;
     case INTERVALL_MENU:
      intervall_menu();
      break;
    } 
    if (changed_status){
      //delay(2000); 
    }
}


void state_menu(void) {
    u8g.firstPage();
  do {
  menu_header();
  u8g.setFont(u8g_font_profont10);
  u8g.setPrintPos(0, 25);
    u8g.print("Fill status: ");
  u8g.print(fillStatus);
  u8g.print(" %");
  u8g.setPrintPos(0, 40);
  u8g.print(starIntervall);
  u8g.print("Intervall: ");
  print_day(intervall);
  u8g.setPrintPos(0, 55);
  u8g.print(starQuantity);
  u8g.print("Quantity: ");
  u8g.print(quantity);
  u8g.print(" %");

  int input = get_potentiometer_degree(POTENTIOMETER_PIN) / 20 ;
  if (input % 2 == 0){
      strcpy(starQuantity, "[X]");
      strcpy(starIntervall, "[ ]");  
    }else{
       strcpy(starQuantity, "[ ]");
      strcpy(starIntervall, "[X]");  
      }
  if (is_triggered(BUTTON_PIN)){
    if (input % 2 == 0){
      status_menu = QUANTITY_MENU;    
    }else{
      status_menu = INTERVALL_MENU;  
      }
       changed_status = true;
  }
  } while (u8g.nextPage() );
}

void intervall_menu(void){
    u8g.firstPage();
  do {
  menu_header();
  u8g.setFont(u8g_font_profont10);
  u8g.setPrintPos(0, 25);
  u8g.print("Turn to set the intervall");
  u8g.setPrintPos(0, 40);
  u8g.print("for feeding");
  u8g.setPrintPos(0, 60);
  int input = get_potentiometer_degree(POTENTIOMETER_PIN) / 330.0 * 4.0;
  print_day(input);
  if (is_triggered(BUTTON_PIN)){
    intervall = input;  
    status_menu = STATUS_MENU;
    changed_status = true;
  }
  } while (u8g.nextPage() );
}

void quantity_menu(void){
    u8g.firstPage();
  do {
  menu_header();
  u8g.setFont(u8g_font_profont10);
  u8g.setPrintPos(0, 25);
  u8g.print("Turn to set the quantity");
  u8g.setPrintPos(0, 40);
  u8g.print("for feeding");
  u8g.setPrintPos(0, 60);
  int input = get_potentiometer_degree(POTENTIOMETER_PIN) / 330.0 * 100.0;
  u8g.print(input);
  u8g.print("%");
  if (is_triggered(BUTTON_PIN)){
    quantity = input;  
    status_menu = STATUS_MENU; 
    changed_status = true;
  }
  } while (u8g.nextPage() );
}

void setup(void) {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop(void) {
  menu(status_menu);
}
```

# Serial communication
![Serial communication](/posts/images/serial_communication.jpg)
After I finished that goal of programming an interface, I tested an example of the usage of serial communication. Therefore, I programmed one Arduino as the receiver and the second as the sender. The idea was to control an LED remotely.
```c
// Receiver
int toggle = false;
void setup() {
  Serial.begin(9600);
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  if (Serial.available() > 0){
    toggle = (bool) Serial.read();
    digitalWrite(LED_BUILTIN, toggle);
  }
} 
```
```c
// Sender
bool toggle = false;

void setup() {
  Serial.begin(9600);
}

void loop() {
    Serial.write(toggle);
    delay(1000);
    toggle = !toggle;
}
```
A lot of Arduino boards have only one serial communication interface. So it can cause some trouble to watch the communication over USB and communicate with different devices at the same time.
## But how does a serial communication work? 
Serial communication is a way of sending data bit by bit. This is useful for sending data between different parts of a computer or between different devices. To do this, the sending device converts the data into a series of bits, which are then sent one at a time over a communication channel. The receiving device then converts those bits back into the original data, allowing the two devices to talk to each other and exchange information.
# Interrupts
Finally, I want to make a test case for interrupts. So I used the LED again to visualize the trigger of an interrupt function.
```c
int pin = 2; 
volatile int state = LOW; 

void setup() {
  Serial.begin(9600);
   pinMode(LED_BUILTIN, OUTPUT); 
   attachInterrupt(digitalPinToInterrupt(pin), blink, CHANGE);

} 
void loop() { 
   Serial.print("Current value: ");
   Serial.println(state);
} 

void blink() { 
   state = !state; 
   digitalWrite(LED_BUILTIN, state);
   Serial.println("State changed");
}
```
The volatile key is necessary to get a proper update between the interrupt function and the main loop.
## why are interrupts useful?
They let the microcontroller pause its current tasks and immediately handle important stuff, like user input or sensor input. This helps microcontrollers do multiple things at once. Interrupts also save power by only waking up when needed.
# Further things to test
In my free time, I really want to test RFID sensors and E-Ink displays. I didn't have enough time to test them in the lab, so I will do that if I have more free time. We will investigate when the time comes ;-) 
