# Use Raspberry pi with Headless Mode
------------------------------------------
**&nbsp;&nbsp;&nbsp;&nbsp;การใช้งาน Raspberry pi ใน Headless Mode คือการใช้โดยที่ไม่ได้ต่อ Mouse หรือ Keyboard กับตัว Raspberry pi แต่จะใช้ Mouse หรือ Keyboard จากคอมพิวเตอร์เครื่องอื่นแลัวทำการ Remote เข้าไป**

### &nbsp;&nbsp;&nbsp;&nbsp;1. สิ่งที่สำคัญคือการ Configure wifi สำหรับการ Remote ก่อนการเขียนไฟล์ .img ลงใน SD Card

<img src="/Blog/picture/headless/Screenshot 2022-10-20 223557.png" alt="headless1"/>

<img src="/Blog/picture/headless/Screenshot 2022-10-20 223618.png" alt="headless2"/>

### &nbsp;&nbsp;&nbsp;&nbsp;2. Write ไฟล์ลง SD Card

<img src="/Blog/picture/headless/Screenshot 2022-09-30 133838.png" alt="headless3"/>

<img src="/Blog/picture/headless/Screenshot 2022-09-30 134148.png" alt="headless4"/>

### &nbsp;&nbsp;&nbsp;&nbsp;3. Boot Raspberry pi แล้วทำการ Remote ด้วย Secure Shell

```bash
  $ ssh pi@raspberrypi.local
```

<img src="/Blog/picture/headless/Screenshot 2022-09-30 142555.png" alt="headless5"/>

### &nbsp;&nbsp;&nbsp;&nbsp;4. หรือจะ Remote Desktop ด้วย VNC โดยการตั้งค่า Raspberry pi ให้สามารถใช้งาน VNC ได้

```bash
  $ sudo raspi-config
```

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143400.png" alt="headless6"/>

#### จากนั้นใช้ Keyboard ในการเลือกที่ Interface Options -> VNC -> YES

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143630.png" alt="headless7"/>

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143651.png" alt="headless8"/>

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143707.png" alt="headless9"/>

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143802.png" alt="headless10"/>

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143820.png" alt="headless11"/>

### &nbsp;&nbsp;&nbsp;&nbsp;5. ใช้โปรแกรม VNC Client เช่น RealVNC Viewer เพื่อ Remote Desktop ใช้งาน Raspberry pi

<img src="/Blog/picture/headless/Screenshot 2022-09-30 143306.png" alt="headless12"/>

#### [>> Homepage](https://pkrittapon.github.io)
