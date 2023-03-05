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
