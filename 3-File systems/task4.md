# Продолжаем

## 1. Raid массивы, что такое икакие бывают
RAID расшифровывается как "Redundant Array of Independent Disks" - "Массив избыточных независимых дисков"  
как я понял, это объединение жёстких дисков для повышения производительности и безопасности при работе с ними  
Есть разные типы:  
* RAID 0 - данные деляться на блоки и распределяются по дискам, которые, в свою очередь, способны параллельно обрабатывать данные, от чего и возникает рост производительности
* RAID 1 - данные просто копируются на несколько дисков, от чего при отказе работы другого этими данными всё равно можно будет воспользоваться с копии
* RAID 5 - деляться между дисками как в случае RAID 0, но ещё хранится информация о чётности, позволяющая восстановить данные в случае отказа одного из дисков
* RAID 6 - RAID 5, но кол-во информации о чётности удвоено, поэтому можно восстановить данные аж в случае отказа двух дисков
* RAID 10 - пушка-бомба (RAID 0 и RAID 1) в одном флаконе
* RAID 50 - несколько RAID 5, которые объединены как RAID 0
* RAID 60 - несколько RAID 6, которые объединены как RAID 0
Я устал, но вроде основные назвал :(
## 2. Добавьте в виртуальную машину 2 диска отформатируйте их в ext4
![Добавил](https://github.com/user-attachments/assets/b0b12065-fbc1-4816-b18d-a0cc3443502a)  
```
lsblk
parted /dev/sdc
mklabel gpt
mkpart primary ext4 0% 100%
quit
mkfs.ext4 /dev/sdc1
```
сначала посмотрел, как они называются у меня (sdc и sdd)
после этого я просто сделал всё, что делал до этого уже в задании 3.3  
то же самое повторил для sdd  
## 3. Создайте из них raid 0 массив
```
mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdc1 /dev/sdd1
```
громоздко, но тут всё просто, с помощью утилиты mdadm мы создаём RAID массив md0 уровня 0 из двух дисков, указываем пути к разделам  
он предупредил, что будет делать RAID массив с ФС ext2fs, поэтому я принудительно потом поменял ему ФС на ext4:  
```
mkfs.ext4 /dev/md0
```
## 4. Проверье всё ли работает
```
cat /proc/mdstat
```
![воть](https://github.com/user-attachments/assets/5e2b2957-7d93-4f15-ba81-381b9854a036)

## 5. Удалите raid0 и создайте raid1
```
mdadm --stop /dev/md0
mdadm --remove /dev/md0
wipefs -a /dev/sdc1
wipefs -a /dev/sdd1
```  
Если сначала удалить, он не остановится (проверено) xD  
wipefs удалит RAID-метки с дисков  
```
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdc1 /dev/sdd1
mkfs.ext4 /dev/md0
```
всё то же самое, просто поменял уровень RAID массива с 0 на 1
## 6. В чём между ними разница?
данные просто копируются на несколько дисков, от чего при отказе работы другого этими данными всё равно можно будет воспользоваться с копии
## 7. Есть ли файловые системы которые поддерживают raid массивы без стороненго ПО
btrfs вроде умеет без mdadm
## 8. Можно ли создать raid массив во время установки системы?
да, просто при установке на этапе выбора дисков для установки системы нужно настроить разделы вручную
