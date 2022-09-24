


## Arduino Control LED Simulator with Wokwi




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

```

ตัวอย่างการทำงานจากการ Simulator จาก Wokwi 

  
 
<iframe width="560" height="315" src="https://www.youtube.com/embed/3cFXFMgyOkc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



#### -รูปแบบที่ 2

&nbsp;&nbsp;&nbsp;&nbsp;เป็นรูปแบบที่ต้องการให้หลอด LED เริ่มจากดับทั้งหมด และค่อยๆติดที่ละดวง และเพิ่มจำนวนขึ้นเรื่อยๆ และเมื่อ LED ติดทั้งหมดจะค่อยๆดับทีละดวง แบบดวงไหนติดทีหลังจะดับก่อน
  

&nbsp;&nbsp;&nbsp;&nbsp;ข้อนี้จะมีตัวอย่างโปรแกรมใน 2 รูปแบบ การเขียนโปรแกรมแบบปกติ และการเขียนแบบ OOP โดยการเขียนโค้ดแบบแรก มีตัวอย่างตามโค้ดด้านล่าง

```

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




#### [>> Homepage](https://pkrittapon.github.io)


