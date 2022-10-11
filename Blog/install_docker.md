# Install docker in Ubuntu 22.04.1
------------------------------------------------


## ขั้นตอนการติดตั้ง Docker ใน Ubuntu 22.04.1
### &nbsp;&nbsp;&nbsp;&nbsp;1. เริ่มจากการ Uninstall Docker ใน Version เก่าออกก่อน
```ShellSession
 $ sudo apt-get remove docker docker-engine docker.io containerd runc
```


### &nbsp;&nbsp;&nbsp;&nbsp;2. ก่อนการติดตั้ง Docker Engine ในครั้งแรก ต้องตั้งค่า repository ของ Docker ก่อน


- อัพเดทแพคเกจ และติดตั้ง repository

```ShellSession
 $ sudo apt-get update
 
 $ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

<img src="/Blog/picture/docker/Screenshot 2022-10-11 160409.png" alt="Docker1" width="588" height="281"/>


- เพิ่ม GPG Key ของ Docker Official

```ShellSession
 $ sudo mkdir -p /etc/apt/keyrings
 $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

<img src="/Blog/picture/docker/Screenshot 2022-10-11 160502.png" alt="Docker2" width="600" height="39"/>


- ตั้งค่า repository ด้วย command ด้านล่าง

```ShellSession
 $ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

<img src="/Blog/picture/docker/Screenshot 2022-10-11 160539.png" alt="Docker3" width="600" height="51"/>


### &nbsp;&nbsp;&nbsp;&nbsp;3. ติดตั้ง Docker Engine 


- อัพเดทแพคเกจ และติดตั้ง Docker Engine Version ล่าสุด

```ShellSession
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```


- แสดงลิสต์ของ Version ใน repository

```ShellSession
 $ apt-cache madison docker-ce
```

<img src="/Blog/picture/docker/Screenshot 2022-10-11 172427.png" alt="Docker4" width="600" height="119"/>


จากนั้นนำ String Version ของ Column บนสุด เช่น <code>5:20.10.18~3-0ubuntu-jammy</code> แทนใน "<VERSION_STRING>"

```ShellSession
 $  sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin
```

<img src="/Blog/picture/docker/Screenshot 2022-10-11 161852.png" alt="Docker5" width="600" height="10"/>


### &nbsp;&nbsp;&nbsp;&nbsp;4. ติดตั้ง Docker Engine เสร็จเรียบร้อย

- หลังจากติดตั้ง Docker Engine เสร็จ ทดลองความถูกต้องด้วย Command ต่อไปนี้

```ShellSession
 $ sudo service docker start
 $ sudo docker run hello-world
```

<img src="/Blog/picture/docker/Screenshot 2022-10-11 162029.png" alt="Docker5" width="600" height="352"/>


#### [>> Homepage](https://pkrittapon.github.io)
