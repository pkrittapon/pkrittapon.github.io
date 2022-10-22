# Install Node-Red in Ubuntu 22.04.1
--------------------------------------

## ขั้นตอนการติดตั้ง Node-red ใน Ubuntu 22.04.1
### &nbsp;&nbsp;&nbsp;&nbsp;1. ติดตั้ง Curl และ Node
```bash
 $ sudo apt install -y curl
 
 $ sudo -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash
 
 $ sudo apt install -y nodejs
```

<img src="/Blog/picture/node-red/Screenshot 2022-10-19 191926.png" alt="Node-Red1"/>

<img src="/Blog/picture/node-red/Screenshot 2022-10-19 192250.png" alt="Node-Red2"/>

<img src="/Blog/picture/node-red/Screenshot 2022-10-19 192330.png" alt="Node-Red3"/>



### &nbsp;&nbsp;&nbsp;&nbsp;2. ติดตั้ง Node-red
```bash
$ sudo npm install -g --unsafe-perm node-red

$ node-red
```

<img src="/Blog/picture/node-red/Screenshot 2022-10-19 192514.png" alt="Node-Red4"/>

<img src="/Blog/picture/node-red/Screenshot 2022-10-19 192540.png" alt="Node-Red5"/>


### &nbsp;&nbsp;&nbsp;&nbsp;3. เปิด Node-red ใน Browser ด้วย ip [http://127.0.0.1:1880/](http://127.0.0.1:1880/) หรือ [http://localhost:1880/](http://localhost:1880/)

<img src="/Blog/picture/node-red/Screenshot 2022-10-19 192630.png" alt="Node-Red6"/>



#### [>> Homepage](https://pkrittapon.github.io)
