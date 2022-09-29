


## Arduino Control LED Simulator with Wokwi

--------------------------------------------------------------------------------------------------


### ตัวอย่างการเขียนโค้ด Arduino บนบอร์ด Arduino nano และ ESP32 เพื่อควบคุมหลอด LED ให้แสดงผลในรูปแบบต่างๆ

&nbsp;&nbsp;&nbsp;&nbsp;รูปแบบการแสดงผลของ LED ในบทความนี้จะยกตัวอย่าง 4 รูปแบบ โดยจะมีการเขียนโปรแกรมใน 2 รูปแบบคือ การเขียนโปรแกรมแบบปกติ และการเขียนแบบ OOP โดยใช้ Class ของภาษา C++ จะมีการยกตัวอย่างให้เห็นทั้งสองรูปแบบในบางข้อ


#### -รูปแบบที่ 1 

&nbsp;&nbsp;&nbsp;&nbsp;เป็นรูปแบบที่ต้องการให้หลอด LED เริ่มจากดับทั้งหมด และค่อยๆติดที่ละดวง และค่อยๆเลื่อนไปทีละ 1
  

&nbsp;&nbsp;&nbsp;&nbsp;ข้อนี้จะมีตัวอย่างโปรแกรมใน 2 รูปแบบ การเขียนโปรแกรมแบบปกติ และการเขียนแบบ OOP โดยการเขียนโค้ดแบบแรก มีตัวอย่างตามโค้ดด้านล่าง


```C++

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


และการเขียนโค้ดด้วย Class ตัวอย่างตามโค้ดด้านล่าง


```C++
#include "Pin.h"

#define DEFAULT_PIN   (22) // (onboard LED)
#define OFF           (0)
#define ON            (1)
#define REPEAT_TIMES  (20)
#define DELAY_MS      (500)

// define a function type
typedef void (*test_func_ptr)(int);

// function prototypes
void test1(int);
void test2(int);

// an array of function pointers which point to test functions
test_func_ptr test_func_list[] = { &test1};

int LED_PINS[] = {23,22,32,33,25,26,27,14,12,13};

void setup() {
   Serial.begin( 115200 );
   Serial.flush();
}

void loop() {
   int num_funcs = sizeof( test_func_list ) / sizeof(test_func_ptr);
   int num_pins = sizeof( LED_PINS ) / sizeof(int);
   for ( int i=0; i < num_pins; i++ ) {
      test_func_list[i % num_funcs]( LED_PINS[ i ] );
   }
}

void test1( int led_pin=DEFAULT_PIN ) {
   Pin pin( led_pin );
   pin.setDirection( Pin::Direction::OUT );
   pin = OFF;
   Serial.printf( "use pin: %d\n", led_pin );
   pin = ON;
   delay(  DELAY_MS );
   pin = OFF;
}

```


ตัวอย่างการทำงานจากการ Simulator จาก Wokwi 

 
<iframe width="560" height="315" src="https://www.youtube.com/embed/4cIonyunK7k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>




#### -รูปแบบที่ 2


&nbsp;&nbsp;&nbsp;&nbsp;เป็นรูปแบบที่ต้องการให้หลอด LED เริ่มจากดับทั้งหมด และค่อยๆติดที่ละดวง และเพิ่มจำนวนขึ้นเรื่อยๆ และเมื่อ LED ติดทั้งหมดจะค่อยๆดับทีละดวง แบบดวงไหนติดทีหลังจะดับก่อน
  

&nbsp;&nbsp;&nbsp;&nbsp;ข้อนี้จะมีตัวอย่างโปรแกรมใน 2 รูปแบบ การเขียนโปรแกรมแบบปกติ และการเขียนแบบ OOP โดยการเขียนโค้ดแบบแรก มีตัวอย่างตามโค้ดด้านล่าง


```C++

#if defined(ESP32)
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
    digitalWrite( LED_PINS[i], LOW );
  }
}
bool state = 1;
void loop() {
  //-------------------------------1-----------------------------
  //ใช้หลักการของ Bit Shift ในการควบคุมรูปแบบของ LED
  static uint16_t value=1;
  if (value == 0){//ใช้เพื่อเลือกให้ LED วิ่งขึ้นหรือวิ่งลง
    Serial.println("Change state!");
    state = !state;
  }
  if (state){
    for ( int i=0; i < NUM_LEDS; i++ ) {
      digitalWrite( LED_PINS[i], (value >> i) & 1 );//สั่งให้ LED ติดปกติ    
    }
    str = "LEDs: value=0b";
    str += String(value, BIN);
    Serial.println( str.c_str() );
  }
  else{
    for ( int i=0; i < NUM_LEDS; i++ ) {
      digitalWrite( LED_PINS[NUM_LEDS-i-1], !(value >> i) & 1 ); //สั่งให้ LED ค่อยๆดับ   
    }
    str = "LEDs: value=0b";
    str += String(value, BIN);
    Serial.println( str.c_str() );
 }
 value = (((value << 1) | (value >> (NUM_LEDS-2)))+1U) & MASK;//ใช้กำหนดว่า Bit ที่ใช้ควบคุมรูปแบบของ LED จะมีค่าสูงสุดเมื่อไหร่ แล้วจึงจะ Reset ค่าให้เป็น 0
 delay(200);
  //----------------------------------------------------------

  //--------------------------2-------------------------------
  //โค้ดนี้เป็นการใช้ Array ในการควบคุมรูปแบบของ LED 
  // for ( int i=0; i < NUM_LEDS; i++ ) {
  //   digitalWrite( LED_PINS[i], HIGH);  
  //   delay(500);   
  // }
  // for ( int i=0; i < NUM_LEDS; i++ ) {
  //   digitalWrite( LED_PINS[NUM_LEDS-1-i], LOW);  
  //   delay(500);   
  // } 
  //---------------------------------------------------------
}

```


และการเขียนโค้ดด้วย Class ตัวอย่างตามโค้ดด้านล่าง


```C++

```


ตัวอย่างการทำงานจากการ Simulator จาก Wokwi 


<iframe width="560" height="315" src="https://www.youtube.com/embed/3cFXFMgyOkc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>




โดยข้อนี้ได้ลองนำโค้ดไป Upload ให้กับ Board Arduino Nano ที่ต่อเข้ากับ LED Array ได้ผลตามวิดีโอด้านล่าง


<iframe width="560" height="315" src="https://www.youtube.com/embed/A-HI3R4zhRU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>





#### -รูปแบบที่ 3


&nbsp;&nbsp;&nbsp;&nbsp;รูปแบบนี้เป็นรูปแบบที่ปรับความสว่างของ LED โดยแต่ละหลอดจะค่อยๆติด จนติดทั้งหมด จากนั้น LED ก็จะค่อยๆดับจนดับทั้งหมด
  

&nbsp;&nbsp;&nbsp;&nbsp;ข้อนี้จะมีตัวอย่างการเขียนโปรแกรมแบบปกติ มีตัวอย่างตามโค้ดด้านล่าง


```C++

const int LED_PINS[] = {5,18,19,21};
const int NUM_LEDS   = sizeof(LED_PINS)/sizeof(int);

#define OFF (LOW) // active-low LED

void setup() {
  Serial.begin(115200);
  for ( int i=0; i < NUM_LEDS; i++ ) {
    // set the direction of the i-th LED pin
    pinMode( LED_PINS[i], OUTPUT ); 
    digitalWrite( LED_PINS[i], OFF );
  }
}

const int PWM_RESOLUTION = 8;
const int PWM_FREQ = 1000;
const int DUTY_MAX = (1 << PWM_RESOLUTION);

void loop() {
  for ( int i=0; i < NUM_LEDS; i++ ) {//loop นี้ใช้เพื่อ
      ledcSetup( i /*channel*/, PWM_FREQ, PWM_RESOLUTION );
      ledcAttachPin( LED_PINS[i] /*pin*/, i /*channel*/ );
      for ( int x=0; x < DUTY_MAX; x++ ) {
        Serial.println(x);
         ledcWrite( i, x);
         delay(5);
      }
  }
  for ( int i=0; i < NUM_LEDS; i++ ) {
      ledcSetup( i /*channel*/, PWM_FREQ, PWM_RESOLUTION );
      ledcAttachPin( LED_PINS[i] /*pin*/, i /*channel*/ );
      for ( int x=DUTY_MAX; x >= 0; x-- ) {
        Serial.println(x);
         ledcWrite( i, x);
         delay(5);
      }
  }
  for ( int i=0; i < NUM_LEDS; i++ ) {
    ledcDetachPin( LED_PINS[i] ); 
    digitalWrite( LED_PINS[i], OFF );
  }
  delay(200);
}

```


ตัวอย่างการทำงานจากการ Simulator จาก Wokwi 


<iframe width="560" height="315" src="https://www.youtube.com/embed/Z8XAAt6aU2A" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>




#### -รูปแบบที่ 3


&nbsp;&nbsp;&nbsp;&nbsp;รูปแบบนี้เป็นรูปแบบที่ให้ LED ติดพร้อมกันที่ละหลายหลอด โดยแต่ละหลอดมีความสว่างไม่เท่ากัน แล้วค่อยๆเลื่อน LED ที่ติดพร้อมกันไปเรื่อยๆ ทำให้มีลักษณะคล้ายกับไฟวิ่ง
  

&nbsp;&nbsp;&nbsp;&nbsp;ข้อนี้จะมีตัวอย่างการเขียนโปรแกรมแบบปกติ มีตัวอย่างตามโค้ดด้านล่าง


```C++

const int LED_PINS[] = {5,18,19,21,12,14,27,26};
const float diff_duty[] = {1,0.4,0.1,0.01};
const int NUM_LEDS   = sizeof(LED_PINS)/sizeof(int);
const int NUM_DUTY = sizeof(diff_duty)/sizeof(int);


#define OFF (LOW) // active-low LED

void setup() {
  Serial.begin(115200);
  for ( int i=0; i < NUM_LEDS; i++ ) {
    // set the direction of the i-th LED pin
    pinMode( LED_PINS[i], OUTPUT ); 
    digitalWrite( LED_PINS[i], OFF );
  }
}

const int PWM_RESOLUTION = 8;
const int PWM_FREQ = 1000;
const int DUTY_MAX = (1 << PWM_RESOLUTION);

void loop() {
  for ( int i=0; i < NUM_LEDS+NUM_DUTY; i++ ) {
    int max_led_on;
    if (i<NUM_DUTY){//ใช้เพื่อเลือกว่า LED จะดิดสูงสุดกี่ดวง
      max_led_on = i+1;
    }
    else if (i>=NUM_LEDS){
      max_led_on = NUM_LEDS+NUM_DUTY-i-1;
    }
    else{
      max_led_on = NUM_DUTY;
    }
    Serial.println("i "+String(i)+"| number of led on "+String(max_led_on));
    if (i < NUM_LEDS){//กรณีที่หัวแถวยังอยู่ใน LED
      for (int x = 0 ; x < max_led_on; x++){
        ledcSetup( i-x /*channel*/, PWM_FREQ, PWM_RESOLUTION );
        ledcAttachPin( LED_PINS[i-x] /*pin*/, i-x /*channel*/ );
        Serial.println("<pin"+String(i-x)+" | analog "+String(diff_duty[x]*DUTY_MAX));
        ledcWrite( i-x, diff_duty[x]*DUTY_MAX );
      }
    }
    else{
      for (int x = 0 ; x < max_led_on; x++){//กรณีที่หัวแถวไม่อยู่ใน LED
        ledcSetup( NUM_LEDS-1-x /*channel*/, PWM_FREQ, PWM_RESOLUTION );
        ledcAttachPin( LED_PINS[NUM_LEDS-1-x] /*pin*/, NUM_LEDS-1-x /*channel*/ );
        Serial.println(">pin"+String(NUM_LEDS-1-x)+" | analog "+String(diff_duty[x+(NUM_DUTY-max_led_on)]*DUTY_MAX));
        ledcWrite( NUM_LEDS-1-x, diff_duty[x+(NUM_DUTY-max_led_on)]*DUTY_MAX );
      }
    }
    delay(100);
    for ( int i=0; i < NUM_LEDS; i++ ) {
      ledcDetachPin( LED_PINS[i] );
      digitalWrite( LED_PINS[i], OFF );
    }
  }
}


```


ตัวอย่างการทำงานจากการ Simulator จาก Wokwi 


<iframe width="560" height="315" src="https://www.youtube.com/embed/Y7x_nj6HJhg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



&nbsp;&nbsp;&nbsp;&nbsp;บทความนี้เป็นเพียงแค่ตัวอย่างในการเขียนโปรแกรม Arduino เพื่อควบคุมการติด-ดับของ LED ให้เป็นในรูปแบบต่างๆ อาจจะไม่ได้มีหลักการทำงานในแต่ละวงจรบอกอย่างชัดเจนและเข้าในได้ง่าย เนื่องจากการเขียนโปรแกรมให้ควบคุมรูปแบบของ LED 1 รูปแบบ สามารถเขียนโปรแกรมได้หลายวิธีมาก ดังนั้นโค้ดที่เห็นจึงเป็นเพียงแค่ตัวอย่างเท่านั้น ไม่ใช่วิธีที่ดีที่สุดเสมอไป



#### [>> Homepage](https://pkrittapon.github.io)
