


## Arduino Control LED Simulator with Wokwi




### ตัวอย่างการเขียนโค้ด Arduino บนบอร์ด Arduino nano และ ESP32 เพื่อควบคุมหลอด LED ให้แสดงผลในรูปแบบต่างๆ

&nbsp;&nbsp;&nbsp;&nbsp;รูปแบบการแสดงผลของ LED ในบทความนี้จะยกตัวอย่าง 4 รูปแบบ โดยจะมีการเขียนโปรแกรมใน 2 รูปแบบคือ การเขียนโปรแกรมแบบปกติ และการเขียนแบบ OOP โดยใช้ Class ของภาษา C++ จะมีการยกตัวอย่างให้เห็นทั้งสองรูปแบบในบางข้อ


#### -รูปแบบที่ 1 

&nbsp;&nbsp;&nbsp;&nbsp;เป็นรูปแบบที่ต้องการให้หลอด LED เริ่มจากดับทั้งหมด และค่อยๆติดที่ละดวง และค่อยๆเลื่อนไปทีละ 1 ตำแหน่งตามในวีดิโอด้านล่าง
  
  
 
<iframe width="560" height="315" src="https://www.youtube.com/embed/3cFXFMgyOkc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


&nbsp;&nbsp;&nbsp;&nbsp;ข้อนี้จะมีตัวอย่างโปรแกรมใน 2 รูปแบบ การเขียนโปรแกรมแบบปกติ และการเขียนแบบ OOP โดยการเขียนโค้ดแบบแรก มีตัวอย่างตามโค้ดด้านล่าง

```
// Author: RSP @KMUTNB
// Date: 2022-09-08

#if defined(ESP32)//ตั้งค่า Pin ให้สามารถใช้กับบอร์ด Arduino Nano และ ESP32 ได้โดยไม่ต้องเปลี่ยน Setting Pin
const int LED_PINS[] = {23,22,32,33,25,26,27,14,12,13};
#else
const int LED_PINS[] = {2,3,4,5,6,7,8,9,10,11};
#endif

const int NUM_LEDS = sizeof(LED_PINS)/sizeof(int);
uint16_t MASK = (1U << NUM_LEDS)-1;

String str;

void setup() {
  Serial.begin(115200);
  for ( int i=0; i < NUM_LEDS; i++ ) {
    // set the direction of the i-th LED pin
    pinMode( LED_PINS[i], OUTPUT ); 
    // turn on the first LED (i=0) and the rest off. 
    digitalWrite( LED_PINS[i], (i==0) ? HIGH : LOW );
  }
}

void loop() {
  int value[NUM_LEDS]={0};
  for ( int i=0; i < NUM_LEDS; i++ ) {//โค้ดใช้การไล่ลำดับใน Array ในการเลื่อนการติดของ LED
      value[i]=1;
      digitalWrite( LED_PINS[i], value[i] );  
      delay(500);
      value[i]=0;
      digitalWrite( LED_PINS[i], value[i] );  
  }
  /*โค้ดที่ใช้หลักการ Bit Shift 
  // str = "LEDs: value=0b";
  // str += String(value, BIN);
  // Serial.println( str.c_str() );
  // value = ((value << 1) | (value >> (NUM_LEDS-1))) & MASK;
  */
}

```







#### [>> Homepage](https://pkrittapon.github.io)


