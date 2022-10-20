# Basic Example Node-red
------------------------------------------------


## ตัวอย่างการ Config Node-red เพื่อใช้งานกับ Protocal MQTT
- รูปตัวอย่าง flow การทำงาน Node-red


<img src="/Blog/picture/basic_node-red/305754181_1167008047558232_5808860046977088597_n.png" alt="ExpNodered1"/>


### &nbsp;&nbsp;&nbsp;&nbsp;1. Setting Block แรกให้เป็น Timer ในการสั้งให้ Block ต่อไปทำงานเป็นรอบการทำงาน ให้ทำงานทุกๆ 5 วินาที


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 214721.png" alt="ExpNodered2"/>


### &nbsp;&nbsp;&nbsp;&nbsp;2. Setting Block ที่สองสำหรับ Function ที่แปลงข้อมูลเวลาให้เป็น String ด้วย code ด้านล่างในภาษา javascript

```javascript
 // Convert the payload to a String object
 msg.payload = new Date(msg.payload).toString();
 return msg;
```

<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 214804.png" alt="ExpNodered2"/>


### &nbsp;&nbsp;&nbsp;&nbsp; 3. ตั้งค่า Mosquitto Client ให้ Publish ข้อมูลไปยัง Client ที่ Subscribe ไว้ใน Topic เดียวกัน


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 215033.png" alt="ExpNodered3"/>


- จากนั้นตั้งค่า Broker ที่ต้องการให้เป็นตัวกลางโดยเลือกใช้ Broker ที่ตั้งขึ้นส่วนตัว


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 215102.png" alt="ExpNodered4"/>


### &nbsp;&nbsp;&nbsp;&nbsp; 4. ตั้งค่า Mosquitto Client ให้รอรับข้อมูลจาก ฺBroker จาก Topic ที่ Subscribe ไว้


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 215149.png" alt="ExpNodered5"/>


### &nbsp;&nbsp;&nbsp;&nbsp; 5. ตั้งค่า Debug Page ให้แสดงผลข้อความที่่รับมาจาก Client


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 215207.png" alt="ExpNodered6"/>


### &nbsp;&nbsp;&nbsp;&nbsp; 6. เมื่อตั้งค่าทั้งหมดแล้ว ค่าที่ได้จาก Debug Page มีค่าตามรูปด้านล่าง 


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 215312.png" alt="ExpNodered7"/>


- และเมื่อดูข้อความที่รอรับจาก Client อีกตัวที่ Subscribe Topic เดียวกันไว้ ก็จะแสดงข้อความเดียวกัน


<img src="/Blog/picture/basic_node-red/Screenshot 2022-10-19 215341.png" alt="ExpNodered8"/>





### &nbsp;&nbsp;&nbsp;&nbsp; 7. สำหรับไฟล์ตัวอย่างสามารถ Export และ Import ได้ด้วยไฟล์ .json 

- ไฟล์ json สำหรับตัวอย่างของการทำงาน
```json
[
    {
        "id": "fc0e24ce8c0e468f",
        "type": "tab",
        "label": "Flow 1",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "cde591e8dd803624",
        "type": "inject",
        "z": "fc0e24ce8c0e468f",
        "name": "Timer",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "5",
        "crontab": "",
        "once": true,
        "onceDelay": "0",
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "x": 120,
        "y": 180,
        "wires": [
            [
                "de9e552a57dcb5e4"
            ]
        ]
    },
    {
        "id": "2dfe0bb901316a30",
        "type": "debug",
        "z": "fc0e24ce8c0e468f",
        "name": "debug 1",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 460,
        "y": 340,
        "wires": []
    },
    {
        "id": "a461a0248c86f96d",
        "type": "mqtt in",
        "z": "fc0e24ce8c0e468f",
        "name": "Subscriber",
        "topic": "test/1234/#",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "373c28f3ff7a7b84",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 240,
        "y": 340,
        "wires": [
            [
                "2dfe0bb901316a30"
            ]
        ]
    },
    {
        "id": "6d5e8f76655688e4",
        "type": "mqtt out",
        "z": "fc0e24ce8c0e468f",
        "name": "Publisher",
        "topic": "test/1234/msg",
        "qos": "0",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "373c28f3ff7a7b84",
        "x": 660,
        "y": 180,
        "wires": []
    },
    {
        "id": "de9e552a57dcb5e4",
        "type": "function",
        "z": "fc0e24ce8c0e468f",
        "name": "function ConvDateString",
        "func": "// Convert the payload to a String object\nmsg.payload = new Date(msg.payload).toString();\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 390,
        "y": 180,
        "wires": [
            [
                "6d5e8f76655688e4"
            ]
        ]
    },
    {
        "id": "373c28f3ff7a7b84",
        "type": "mqtt-broker",
        "name": "Mosquitto",
        "broker": "localhost",
        "port": "1883",
        "clientid": "nodered_demo",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "4",
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    }
]
```


#### [>> Homepage](https://pkrittapon.github.io)
