# Юниты

## 1. Что такое systemd юнит?
инструкция по выполнению для systemd
## 2. Проверье статус любого systemd юнита, какую информацию выводит эта команда?
```
systemctl status smb
```
название, откуда загружен юнит, его состояние (активен или нет), документация, статус, сколько висит задач, потребляемая память, время выполнения, логи по запуску
## 3. ПОпробуйте оставновить сервис.
```
systemctl stop smb
```
если посмотреть статус, там написано, что он помер :/
## 4. Перезапустите его.
```
systemctl restart smb
```
снова ожил!
## 5. УДалите из автозагрузки
```
systemctl disable smb
```
## 6. Верните обратно
```
systemctl enable smb
```
## 7. Что такое таймеры?
юниты, которые позволяют выполнить команду в определённое время

