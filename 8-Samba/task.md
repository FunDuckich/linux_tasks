
# Шарим


## 1. Установите пакет samba
```
apt-get install samba
```
## 2. ЧТо такое побщая папка, зачем оно может быть нужно?
папка, которая с помощью samba предоставляется для всех пользователей в лоакльной сети  
хах, ну можно обмениваться файлами)  
на что воображения хватит
## 3. Создайте общую папку без пароля с правами только на чтение файлов
```
mkdir obshaya
vim /etc/samba/smb.conf
```
тут я добавил такую инфу:  
\[Obshaya]  
    path = /root/obshaya  
    browseable = yes  
    guest ok = yes  
    read only = yes  
```
systemctl restart smb
smbclient -L localhost
```
перезапустил юнит самбы, второй командой посмотрел все общедоступные папки
## 4. Создайте общую папку с паролем с правами на чтение и запись
\[Drugaya]
	path = /root/drugaya
	browseable = yes
	writable = yes
	guest ok = no
## 5. Создайте общую папку с доступом для какой-то группы с полными правами
```
groupadd all_shared
vim /etc/samba/smb.conf
```
тут я сделал вот что:  
\[Mmm]
    path = /root/mmm
    browseable = yes
    valid users = @all_shared
    guest ok = no
    writable = yes
## 6. Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа
```
groupadd full
groupadd readonly
groupadd noaccess
vim /etc/samba/smb.conf
```
ну и снова модифицируем конфиг:  
\[SambaItsCool]
	path = /root/sambaitscool
    browseable = yes
    guest ok = no
	read only = yes
 	write list = @full
  	invalid users = @noaccess

