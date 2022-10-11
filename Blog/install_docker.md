# Install docker in Ubuntu 22.04.1
------------------------------------------------


## ขั้นตอนการติดตั้ง Docker ใน Ubuntu 22.04.1
### &nbsp;&nbsp;&nbsp;&nbsp;1.เริ่มจากการ Uninstall Docker ใน Version เก่าออกก่อน
```ShellSession
 $ sudo apt-get remove docker docker-engine docker.io containerd runc
```


### &nbsp;&nbsp;&nbsp;&nbsp;2.ก่อนการติดตั้ง Docker Engine ในครั้งแรก ต้องตั้งค่า repository ของ Docker ก่อน


- อัพเดทแพคเกจ และติดตั้ง repository
```ShellSession
 $ sudo apt-get update
 
 $ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```


- เพิ่ม GPG Key ของ Docker Official
```ShellSession
 $ sudo mkdir -p /etc/apt/keyrings
 $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```


- ตั้งค่า repository ด้วย command ด้านล่าง
```ShellSession
 $ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```


### &nbsp;&nbsp;&nbsp;&nbsp;3.ติดตั้ง Docker Engine 
- อัพเดทแพคเกจ และติดตั้ง Docker Engine Version ล่าสุด
```ShellSession
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```


- แสดงลิสต์ของ Version ใน repository
```
 $ apt-cache madison docker-ce
```

