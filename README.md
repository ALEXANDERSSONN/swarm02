# REFERENCE

[github.com/docker/awesome-compose](https://github.com/docker/awesome-compose)


# SWARM-DEPLOY

[spcn04fastapi.xops.ipv9.me](https://spcn04fastapi.xops.ipv9.me/)

# WAKATIME
[WAKATIME](https://wakatime.com/@spcn04/projects/sfryxbanlt)
# Setup-Linux
- Set เวลา โดยใช้คำสั่ง
```
sudo time datectl set-timezone Asia/Bangkok
```
 - คำสั่งที่ใช้ในการ Set ชื่อ Hostname
```
sudo hostnamectl set-hostname **ชื่อที่ต้องการจะตั้ง**
```
- คำสั่งที่ใช้ในการเปลี่ยน Machine-ID
```
rm /var/lib/dbus/machine-id
echo -n > /etc/machine-id
cat /etc/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
```

# INSTALL-DOCKER
- คำสั่งที่ใช้ลง Docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- คำสั่งตรวจสอบการใช้งาน Docker
```
sudo docker run hello-world
```
- หากใช้งานได้ตามปกติ ผลลัพธ์จะขึ้นดังรูปนี้

![image](https://user-images.githubusercontent.com/115150753/224604678-c6126e4c-a2e4-4eef-9229-f46b1c733d47.png)
# BUILD-IMAGE & TAG
- คำสั่งการ Build image
```
sudo docker compose "fastapi/compose.yaml" up -d --build
```
- คำสั่งการ Tag
```
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

- ผลลัพธ์การ TAG สำเร็จ

![image](https://user-images.githubusercontent.com/115150753/224604932-e4b3384d-12c7-414f-ab81-194216e3dbf5.png)

# PUSH IMAGE TO DOCKER HUB 
- คำสั่งเข้าสู่ระบบ Docker ใน VSCODE
```
docker login
```
- คำสั่ง Push Image To Docker Hub
```
docker push TARGET_IMAGE[:TAG]
```

![image](https://user-images.githubusercontent.com/115150753/224605223-dcc10cef-b94c-4d01-a6c3-d669921e8dd4.png)


# CREATE STACK DEPLOY
- สร้างไฟล์ compose.yaml
```
version: '3.7'
services:
  api:
    image: alexanderssonn/swarm02-api:0304
    networks:
     - webproxy
    ports:
     - "8808:8000"
    environment:
     PORT: 8000
    logging:
      driver: json-file
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - app:/app
    deploy:
      replicas: 1

volumes:
  app:          
networks:
  webproxy:
    external: true
```
- นำ compose.yaml ไป Stack Deploy on local

# SWARM CLUSTER
- Revert Proxy compose.yaml
```
version: '3.7'
services:
  api:
    image: TARGET_IMAGE[:TAG]
    networks:
     - webproxy
    environment:
     PORT: 8000
    logging:
      driver: json-file
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - app:/app
    deploy:
      replicas: 1
      labels:
        - traefik.docker.network=webproxy
        - traefik.enable=true
        - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
        - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME} URL ")
        - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
        - traefik.http.services.${APPNAME}.loadbalancer.server.port=8000

volumes:
  app:          
networks:
  webproxy:
    external: true
```

- เข้าไปที่ Portainer กดที่ Stack
- ADD Stack

![image](https://user-images.githubusercontent.com/115150753/224602833-fa1c022d-9c46-441f-b47b-de656e29446e.png)

- COPY ข้อมูลใน Revert Proxy compose.yaml มาทั้งหมดแล้วนำไปใส่ที่ Web Editor

![image](https://user-images.githubusercontent.com/115150753/224603071-8f8f2159-ea41-43e8-9acd-67967724d543.png)

- เพิ่ม ENVIRONMENT VARIABLE ของ APPNAME ตามชื่อต้องการ

![image](https://user-images.githubusercontent.com/115150753/224603357-2cf8f55c-37c1-4650-84cc-20724bc8398b.png)


![chrome_pBhYL4EpB5](https://user-images.githubusercontent.com/115150753/223736430-23798aae-7ec1-4be0-a68b-4c865c4f763a.png)
