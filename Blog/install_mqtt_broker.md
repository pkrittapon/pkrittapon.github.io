# Install MQTT Broker in Ubuntu 22.04.1 for VirtualBox
-------------------------------------------------
> **สำหรับขั้นตอนที่สำคัญการติดตั้ง MQTT Broker ใน VirtualBox คือการตั้งค่า Network ให้เป็นโหมด Bridged Adapter**
>
>
> <img src="/Blog/picture/broker/Screenshot 2022-10-20 194405.png" alt="ฺBroker1"/>

## ขั้นตอนการติดตั้ง Mosquitto MQTT Broker

### &nbsp;&nbsp;&nbsp;&nbsp;1. พิมพ์ Command ต่อไปนี้เพื่อติดตั้ง mosquitto

```ShellSession
  $ sudo apt update 
  $ sudo apt install -y mosquitto
```

### &nbsp;&nbsp;&nbsp;&nbsp;2. สร้างไฟล์ mosquitto.conf เพื่อกำหนดรูปแบบการใช้งาน

```
pid_file /run/mosquitto/mosquitto.pid

persistence true
persistence_location /var/lib/mosquitto/

log_dest file /var/log/mosquitto/mosquitto.log

include_dir /etc/mosquitto/conf.d

# mqtt
listener 1883
protocol mqtt
```


- เปิด port 1883 สำหรับ protocal MQTT



### &nbsp;&nbsp;&nbsp;&nbsp;3. เปิดใช้งาน Mosquitto

```ShellSession
  $ sudo systemctl enable mosquitto.service
  $ sudo systemctl restart mosquitto
  $ sudo systemctl status mosquitto
```

## การใช้งาน MQTT Broker เบื้องต้น


### &nbsp;&nbsp;&nbsp;&nbsp;1. ติดตั้ง Mosquitto MQTT Client

```ShellSession
  $ sudo apt install mosquitto-clients -y
```

### &nbsp;&nbsp;&nbsp;&nbsp;2. หลังจากติดตั้ง Mosquitto Client เรียบร้อยแล้ว เริ่มทำการรอรับข้อความจาก MQTT Broker ด้วยคำสั่ง Subscribe


```ShellSession
  $ mosquitto_sub -h localhost -p 1883 -t 'test/1234/#'
```

<img src="/Blog/picture/broker/sub.png" alt="ฺBroker2"/>


- การเชื่อมต่อด้วย localhost จะใช้งานได้สำหรับภายในอุปกรณ์เดียวกัน ถ้าต้องการเชื่อมต่อกับอุปกรณ์ภายนอก จำเป็นต้องรู้ IP Address ของเครื่องที่เป็น Broker เช่น IP Address ```192.168.0.40``` เป็น IP Address ของเครื่องที่เป็น Broker จะใช้คำสั่งเป็น

```ShellSession
  $ mosquitto_sub -h 192.168.0.40 -p 1883 -t 'test/1234/#'
```


<img src="/Blog/picture/broker/sub2.png" alt="ฺBroker3"/>


### &nbsp;&nbsp;&nbsp;&nbsp;3. จากนั้นเปิด Terminal ขึ้นมาอีก 1 Tab เพื่อเป็น Client อีกหนึ่งตัว สำหรับ Publish ข้อความไปยัง MQTT Broker


```ShellSession
  $ mosquitto_pub -h localhost -p 1883 -t 'test/1234/msg' -m 'Hello'
```


<img src="/Blog/picture/broker/pub.png" alt="ฺBroker4"/>

## บทสรุป
&nbsp;&nbsp;&nbsp;&nbsp;บทความนี้พูดถึงการ Configure MQTT Broker เบื้องต้นสำหรับงานที่ไม่ได้ต้องการยืนยันตัวตน หรือ ไม่ต้องการเข้ารหัสข้อความที่ต้องการส่ง เท่านั้นหากต้องการคุณสมบัติเหล่านี้เพิ่มเติมจำเป็นต้อง Configure เพิ่มเติม 

#### [>> Homepage](https://pkrittapon.github.io)
