# Скриптуем по полной

## 1. Что такое шебанг?
запись, указывающая, какой интерпритатор нужно использовать для выполнения
## 2. Обязательно ли исполняемый файл дожен иметь соотвествующее расширение?
не, линуксу важен лишь шебанг
## 3. Напишите скрипт который выполнит автоматически действия из блока работы с файлами. ( не забудьте включить set -euo pipefail для того что бы ваш скрипт было удобнее отлаживать. Опишите что включают эти флаги)

```
#!/bin/bash
set -euo pipefail

# 1
cd /home/funduck

# 2
ls

# 3
ls -a

# 4
mkdir -p papka/podpapka

# 5
echo "lol ahahaa" > papka/podpapka/lol.txt

# 6
mv papka/podpapka/lol.txt papka

# 7
cp papka/podpapka/lol.txt papka/copy.txt

# 8
mv papka/copy.txt papka/renamed.txt

# 9
diff papka/copy.txt papka/renamed.txt

# 10
sort papka/podpapka/lol.txt
sort -r papka/podpapka/lol.txt

# 11
rm -rf papka
```    
"set -euo pipefail" говорит о том, что скрипт завершит свою работу, если возникнет ошибка, если какая-то из переменных в скрипте не будет про инициализирована или же если любая команда цепочки вернёт ошибку
