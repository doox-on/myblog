---
title: Self-Sustaining Solar-Powered Greenhouse
categories:
- General
excerpt: |
  A pot still is a type of still used in distilling spirits such as whisky or brandy. Heat is applied directly to the pot containing the wash (for whisky) or wine (for brandy).
feature_text: |
  ## Solar-Powered Greenhouse

feature_image: "https://picsum.photos/2560/600?image=872"

---

An automated, self-sufficient greenhouse designed for urban agriculture, featuring a sun-tracking solar panel and integrated environmental controls.


{% include figure.html image="/assets/greenhouse1.jpeg" %}

{% include video.html id="1XSyMbiWBRU" %}

---

### Project Overview

The SMART Greenhouse is a compact, automated system designed to make gardening accessible to urban dwellers and plant enthusiasts with limited space. It addresses the challenges of manual greenhouse management by automating key processes like temperature and humidity regulation, all while operating entirely on self-generated solar power. The core novelty lies in its self-sufficiency, combining automated controls with an energy-efficient power system featuring a solar panel mounted on a rotating gimbal to maximize sun exposure throughout the day.

{% include figure.html image="/assets/greenhouse2.jpeg" %}

---

### Key Features

I was responsible for the design and implementation of the greenhouse’s control systems. This involved developing the closed-loop climate regulation mechanisms and integrating real-time environmental monitoring.

{% include video.html id="Jp6AsWEoLeo" %}

* **Automated Climate Control**
    * The internal environment is regulated using a closed-loop control system.
    * If the internal temperature exceeds the external temperature, a servo-controlled venting window opens to allow for passive air exchange.
    * If the temperature continues to rise, an exhaust fan activates to actively push hot air out.

* **Real-Time Environmental Monitoring**
    * Three sets of temperature and humidity sensors monitor the conditions outside the greenhouse, inside the greenhouse, and in the soil.
    * All sensor data is clearly displayed on a 2-inch LCD screen, providing the user with an immediate overview of the plant's environment.


* MCU 1 : Handling LCD monitor and Data Calculation


```cpp
#include <Adafruit_GFX.h>    // Core graphics library
#include <Adafruit_ST7789.h> // Hardware-specific library for ST7789
#include <SPI.h>
#include <Wire.h>
#include <AHT20.h>
#include <SoftwareSerial.h>
#include <ClosedCube_SHT31D.h>

SoftwareSerial mbedSerial(0, 1); // RX, TX

AHT20 aht20;


#define TFT_CS        10
#define TFT_RST        9 // Or set to -1 and connect to Arduino RESET pin
#define TFT_DC         6
#define TFT_MOSI 11  // Data out
#define TFT_SCLK 13  // Clock out

Adafruit_ST7789 tft = Adafruit_ST7789(TFT_CS, TFT_DC, TFT_MOSI, TFT_SCLK, TFT_RST);
ClosedCube_SHT31D sht3xd;

char buffer_12[26];
int buffer_index = 0;

float p = 3.1415926;

void setup(void) {
  Serial.begin(9600);
  mbedSerial.begin(9600);
  
  Serial.println(F("Hello! ST77xx TFT Test"));

  delay(50);

  tft.init(240, 320);           // Init ST7789 320x240

  // SPI speed defaults to SPI_DEFAULT_FREQ defined in the library, you can override it here
  // Note that speed allowable depends on chip and quality of wiring, if you go too fast, you
  // may end up with a black screen some times, or all the time.
  //tft.setSPISpeed(40000000);
  // Serial.println(F("Initialized"));
  tft.fillScreen(ST77XX_BLACK);
  delay(50);

  Wire.setSDA(12);
  Wire.setSCL(20);
  Wire.begin();
  sht3xd.begin(0x44); // I2C address: 0x44 or 0x45
  if (sht3xd.periodicStart(SHT3XD_REPEATABILITY_HIGH, SHT3XD_FREQUENCY_10HZ) != SHT3XD_NO_ERROR)
  Serial.println("[ERROR] Cannot start periodic mode");

  delay(50);

}

void loop() {
  // float temperature = aht20.getTemperature();
  // float humidity = aht20.getHumidity();

  Serial.print("Im in the loop\n");
  Serial.println(mbedSerial.available());

  tft.setTextWrap(true);
  tft.fillScreen(ST77XX_BLACK);
  tft.setCursor(0, 30);
  tft.setTextColor(ST77XX_WHITE);
  tft.setTextSize(4);

  int linecount = 0;
  while (linecount < 4) {
  if(mbedSerial.available()) {
    char c = mbedSerial.read();     
    // mbedSerial.readBytes((char *)&c, sizeof(int)); 
    // tft.print("Received: ");
    // Serial.println(c);
    // tft.print(c);
    // tft.println(c);

    // delay(2000);
    if (c == '\n') {            
    tft.setTextColor(ST77XX_WHITE);

      buffer_12[buffer_index] = '\0';     
      // int value = atoi(buffer_12);
      int value = atoi(buffer_12 + 3);
      if (strncmp(buffer_12, "T1:", 3) == 0) {

        if (value >= 30) {
          tft.setTextColor(ST77XX_RED);
        } else if (value <= 17) {
          tft.setTextColor(ST77XX_BLUE);
        }

        tft.print("IN T:");
        tft.print(value);
          tft.println("C");

      } else if(strncmp(buffer_12, "T2:", 3) == 0){
        if (value >= 30) {
          tft.setTextColor(ST77XX_RED);
        } else if (value <= 17) {
          tft.setTextColor(ST77XX_BLUE);
        }
        tft.print("OUT T:");
        tft.print(value);
          tft.println("C");

      } else if(strncmp(buffer_12, "H1:", 3) == 0){
        if (value >= 70) {
          tft.setTextColor(ST77XX_RED);
        } else if (value <= 50) {
          tft.setTextColor(ST77XX_BLUE);
        }
        tft.print("IN H:");
        tft.print(value);
          tft.println("%");

      } else if(strncmp(buffer_12, "H2:", 3) == 0){
        if (value >= 70) {
          tft.setTextColor(ST77XX_RED);
        } else if (value <= 50) {
          tft.setTextColor(ST77XX_BLUE);
        }
        tft.print("OUT H:");
        tft.print(value);
          tft.println("%");

      }

      buffer_index = 0;                 
      linecount++;

    } else {
      if (buffer_index < sizeof(buffer_12) - 1) {
        buffer_12[buffer_index++] = c;   
      }
    }
  }
  }
    while (mbedSerial.available()) {
    mbedSerial.read();
  }
  tft.setTextColor(ST77XX_WHITE);

  SHT31D result = sht3xd.periodicFetchData();
  int tem = (int)result.t;
  int hum = (int)result.rh;

if (tem >= 30) {
  tft.setTextColor(ST77XX_RED);
} else if (tem <= 17) {
  tft.setTextColor(ST77XX_BLUE);
}
  tft.print("Soil T:");
  tft.print(int(result.t));
  tft.println("C");
  tft.setTextColor(ST77XX_WHITE);

if (hum >= 70) {
  tft.setTextColor(ST77XX_RED);
} else if (hum <= 50) {
  tft.setTextColor(ST77XX_BLUE);
}
  tft.print("Soil H:");
  tft.print(int(result.rh));
  tft.println("%");


  delay(3000);

}

```


* MCU 2: Handling sensors and motors. Send data to the MCU 1.



```cpp
/* mbed Microcontroller Library
 * Copyright (c) 2019 ARM Limited
 * SPDX-License-Identifier: Apache-2.0
 */

#include "mbed.h"
#include "Motor.h"
#include "Servo.h"
#include "AHT20.h"
#include <cstdio>


DigitalOut mosfetGate(PA_4);
// AnalogIn motorPot(p18);

DigitalOut led1(LED1);
BufferedSerial uart(PA_9,PA_10,9600);
PwmOut myservo1(PA_8);
PwmOut myservo2(PA_11);
// float servoVal = 0.001;
// PwmOut motorPWM(p23);
// DigitalOut AIn2(p20);
// DigitalOut AIn1(p19);
DigitalIn pin(PA_0);

Thread thread1;
Thread thread2;
Thread thread3;
Thread thread4;
Thread thread5;
bool isWindowOpen = false;

char buffer[26];
float s;
bool value = true;
int what = 100;
int temp1 = 0;
int humid1 = 0;
int temp2 = 0;
int humid2 = 0;
/// Sensor /// 
AHT20 myAHT201   ( PB_7, PB_6, AHT20::AHT20_ADDRESS, 100000 ); //(SDA,SCL)
AHT20 myAHT202   ( PB_4, PA_7, AHT20::AHT20_ADDRESS, 100000 );
Ticker      newReading;

//@brief Variables.
uint32_t    myState = 0UL;

//@brief   FUNCTION PROTOTYPES
void    changeDATA     ( void );



void out_temp_sensor1() {

    AHT20::AHT20_user_data_t    myAHT20_Data;
    AHT20::AHT20_status_t       aux;


    // Reset the device
    aux  =   myAHT201.AHT20_SoftReset ();
    wait_us(80000);

    // Get the state byte
    aux  =   myAHT201.AHT20_TriggerStatus ();
    wait_us(80000);

    aux  =   myAHT201.AHT20_GetStatus ( (uint8_t*)&myAHT20_Data.state );

    // Check if the device is calibrated
    if ( ( myAHT20_Data.state & AHT20::STATUS_CAL_MASK ) == AHT20::STATUS_CAL_UNCALIBRATED ) {
        // Calibrate the sensor
        aux  =   myAHT201.AHT20_Calibrate ();
    }            

    while(1) {

        // Trigger a new measurement data
        aux  =  myAHT201.AHT20_TriggerMeasurement ();

        // Get the temperature value
        aux  =  myAHT201.AHT20_GetAllData ( (AHT20::AHT20_user_data_t*)&myAHT20_Data );

        // Process the temperature data
        myAHT20_Data.temperature.temperature = myAHT201.AHT20_ProcessTemperature ( myAHT20_Data.temperature.raw_temperature );

        // Process the humidity data
        myAHT20_Data.humidity.humidity = myAHT201.AHT20_ProcessHumidity ( myAHT20_Data.humidity.raw_humidity );

        temp1 = (int)(myAHT20_Data.temperature.temperature);
        humid1 = (int)(myAHT20_Data.humidity.humidity);
        sprintf(buffer,"T1:%dC\nH1:%d\nT2:%dC\nH2:%d\n", temp1,humid1,temp2,humid2);
        
        // printf("What: %d\n", uart.writable());

        if (uart.writable()) {
            uart.write(buffer, 26);
        }
        // Transmit result over the UART
        // printf( "Sensor 1 - Temp: %d C | RH: %d %% --", temp1, humid1 );
        wait_us(1000);
        myState  =   0UL;  
        ThisThread::sleep_for(500ms);

        }

}


void out_temp_sensor2() {

    AHT20::AHT20_user_data_t    myAHT20_Data;
    AHT20::AHT20_status_t       aux;


    // Reset the device
    aux  =   myAHT202.AHT20_SoftReset ();
    wait_us(80000);

    // Get the state byte
    aux  =   myAHT202.AHT20_TriggerStatus ();
    wait_us(80000);

    aux  =   myAHT202.AHT20_GetStatus ( (uint8_t*)&myAHT20_Data.state );

    // Check if the device is calibrated
    if ( ( myAHT20_Data.state & AHT20::STATUS_CAL_MASK ) == AHT20::STATUS_CAL_UNCALIBRATED ) {
        // Calibrate the sensor
        aux  =   myAHT202.AHT20_Calibrate ();
    }            

    while(1) {

        // Trigger a new measurement data
        aux  =  myAHT202.AHT20_TriggerMeasurement ();

        // Get the temperature value
        aux  =  myAHT202.AHT20_GetAllData ( (AHT20::AHT20_user_data_t*)&myAHT20_Data );

        // Process the temperature data
        myAHT20_Data.temperature.temperature = myAHT202.AHT20_ProcessTemperature ( myAHT20_Data.temperature.raw_temperature );

        // Process the humidity data
        myAHT20_Data.humidity.humidity = myAHT202.AHT20_ProcessHumidity ( myAHT20_Data.humidity.raw_humidity );

        temp2 = (int)(myAHT20_Data.temperature.temperature);
        humid2 = (int)(myAHT20_Data.humidity.humidity);


        // Transmit result over the UART
        // printf( "Sensor 2 - Temp: %d C | RH: %d %%\r\n", temp2, humid2 );
        wait_us(1000);
        myState  =   0UL;  

        ThisThread::sleep_for(500ms);
    }

}


void servo1() {

    while(1)
    {   

        if (value) {
            myservo1.period_ms(20);
            myservo1.pulsewidth_us(1280);
            wait_us(10000);

        } else {
            
                myservo1.period_ms(20);
                myservo1.pulsewidth_us(1720);
            wait_us(10000);
        }

        value = !value;

        // for (int i= 0; i <100000; i++) {
        //     myservo.period_ms(20);
        //     myservo.pulsewidth_us(1500);
        // }        
 
        // if (value) {
        //     myservo.position(90);
       
        // else {
        //     myservo.position(-90);
        // }
        
        
        // value = !value;
        ThisThread::sleep_for(50ms);
    }

}

void servo2() {
    bool shouldOpen = false;
    bool shouldClose = false;
    while(1)
    {   

    while(pin.read()) {
        myservo2.period_ms(20);
        myservo2.pulsewidth_us(1780);
    } 

            myservo2.period_ms(20);
    myservo2.pulsewidth_us(1100);
    
        
        ThisThread::sleep_for(50ms);
    }

}

void fan() {
    while(true) {
        printf("%d\n",temp1);

        if(temp1 >= 30) {
            mosfetGate = 1;
        } else {
            mosfetGate = 0;
        }
        ThisThread::sleep_for(50ms);
    }
}

void openWindow() {
    myservo2.pulsewidth_us(2500);
}
void closeWindow() {
    myservo2.pulsewidth_us(1000);
}


void controlThread() {
    myservo2.period_ms(20);

    while (true) {
        bool doOpen = false;
        bool doFan  = false;

        if (pin.read()) {
            openWindow();
        } else {

            if (temp1 > 30) {
                doOpen = true;
                doFan  = true;
            } else if (temp1 > 27) {
                doOpen = true;
            }
            else if (temp1 < 18) {
            }
            else {
            }


            if (doOpen && !doFan) {
                openWindow();
                mosfetGate = 0; //fan off
            }
            else if (doOpen && doFan) {
                openWindow();
                mosfetGate = 1; //fan on
            }
            else {
                closeWindow();
                mosfetGate = 0;  //fan off
            }
        }

        ThisThread::sleep_for(500ms);
    }
}

int main() {

    thread1.start(out_temp_sensor1);
    thread2.start(out_temp_sensor2);
    // thread3.start(servo1);
    // thread4.start(servo2);
    thread5.start(controlThread);
    // thread5.start(fan);

    while (true){

        ThisThread::sleep_for(10ms);

        
    }
 
}

```
---

### Challenges & Solutions

* **Challenge: I/O Port Scarcity and Hardware Integration**
    * A key technical challenge arose from I/O port limitations on our microcontroller. The design required multiple temperature/humidity sensors, but due to the address-based nature of the I2C protocol, only one could be used per bus. This, combined with other peripherals, created a critical shortage of available I/O ports.
    * **Solution:** We implemented a dual-microcontroller architecture to overcome this hurdle. A second MCU was added to manage a subset of the sensors, and the two controllers communicated seamlessly via UART. This allowed the primary MCU to aggregate all environmental data for display on the LCD. For future iterations, using an I2C multiplexer would be an alternative solution to explore.

* **Challenge: Aligning Team-Wide Design Vision**
    * Initial implementation was significantly hampered because team members had different undeclared concepts for the final design. This discrepancy between visions caused rework and implementation difficulties, ultimately contributing to the decision to de-scope the water irrigation system.
    * **Solution:** Once we identified this conceptual gap, we immediately held detailed discussions to align on every aspect of the project. We made the pragmatic decision to cut features that could not be completed in time and focused entirely on the core functionalities. This experience was a critical lesson in the importance of ensuring all team members are on the same page through detailed, shared documentation from the project's outset.

