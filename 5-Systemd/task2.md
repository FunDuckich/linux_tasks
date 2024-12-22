# Пишем юниты

## 1. Создайте скрипт который создаёт папку заполняет её файлами ( имена 1-4 ) и записывает в них информацию
о текущей дате, версии ядра, имени компьютера и списе всех файлов в домашнем каталоге пользователя от которого выполняется скрипт( не забудьте сдлеать проверку на существование файлов и папок)
``` bash
#!/bin/bash

folder_path="/home/funduck/papochka"

if [ ! -d "$folder_path" ]; then
    mkdir "$folder_path"
else
    existing_path=$(realpath "$folder_path")
    echo "$papochka уже существует по пути $existing_path"
fi


current_date=$(date)
kernel_version=$(uname -r)
hostname=$(hostname)
file_list=$(ls -a "/home/funduck")

files=("1" "2" "3" "4")

for file in "${files[@]}"; do
    file_path="$folder_path/$file"

    if [ ! -f "$file_path" ]; then
        echo "$current_date" > "$file_path"
        echo "$kernel_version" >> "$file_path"
        echo "$hostname" >> "$file_path"
        echo -e "\n\n\n\n" >> "$file_path"
        echo "$file_list" >> "$file_path"
    else
        echo "$file_path уже существует"
    fi
done
```
## 2. Создайте юнит который будет вызывать этот скрипт при запуске. Проверьте
```
nano /etc/systemd/system/for_task_5_2.service
```
так я создал юнит
```
[Unit]
Description=just executing script on start xd

[Service]
ExecStart=/bin/bash /home/funduck/scripts/for_task_5_2.sh

[Install]
WantedBy=multi-user.target
```
а так он выглядит
> [Install]
> WantedBy=multi-user.target  
это как раз указывет на то, что юнит будет запускаться, когда система будет готова к мультизадачности (по сути после запуска)
```
systemctl daemon-reload && systemctl enable for_task_5_2.service
```
это я перезагрузил все юниты, которые есть (демоны гы), и включил автозапуск моего
  
после ребута, посмотрев статус моего юнита, можно увидеть, что он выполнился успешно)  
ну и файлики можно проверить  
при повторном ребуте естественно ничего не создастся, это можно посмотреть в статусе моего юнита  
## 3. Создайте таймер который будет вызывать выполнение одноимённого systemd юнита каждые 5 минут.
```
nano /etc/systemd/system/for_task_5_2.timer
```
а в нём
```                                  
[Unit]
Description=zachem blin                                    

[Timer]
OnUnitActiveSec=5min

[Install]
WantedBy=timers.target
```
## 4. От какого пользователя вызыаются юниты поумолчанию?
от рута, но это можно изменить)  
учитывая, что я захардкодил пути в своём скрипте (домашний каталог пользователя funduck), это будет полезно, ведь тогда юнит тупо не выполнится, если нет такого пользователя:
```                                   
[Unit]
Description=just executing script on start xd

[Service]
ExecStart=/bin/bash /home/funduck/scripts/for_task_5_2.sh
User=funduck

[Install]
WantedBy=multi-user.target
```
## 5. Создайте пользователя от имени которого будет выполняться ваш скрипт.
ну он у меня уже есть!
## 6. Дополните юнит информацией о пользователе от которого должен выплняться скрипт.
ой, это я сделал в 4-ом пункте
## 7. Дополните ваш скрипт так, что бы он независимо от местоположения всега выполнялся в домашней папке того кто его вызывает.
ну блин, тогда надо убрать в юните инфу о том, от имени какого пользователя выполняется он :(
```
folder_path="$HOME/papochka"
# и ниже там
file_list=$(ls -a "$HOME")
```
а вот, что надо у меня изменить в скрипте
