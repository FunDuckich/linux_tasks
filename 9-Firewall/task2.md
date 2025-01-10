# Открываем firewald

## 1. Удалите iptables и установите firewalld
```
systemctl stop iptables
systemctl disable iptables
apt-get remove iptables
apt-get install firewalld
```
## 2. Попробуйте так-же проверить возможность подключения по ssh
всё работает
## 3. Если её нет то откройте порт
## 4. Выведите список открытых портов с помощью firewall-cmd
```
firewall-cmd --zone=public --list-ports
```
## 5. Можно ли там добавить порты по названию сервиса?
да, с помощью параметра `--add-service`
## 6. На вашей Локальной виртуальной машине попробуйте подключиться к серверу samba из предыдущих заданий
```
smbclient localhost/Obshaya
```
## 7. Если не получилось то откройте нужные порты
## 9. Сделайте так чтобы изменения были постоянными
для этого нужно указать флаг --permanent


