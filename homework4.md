# Домашнее задание к уроку 4. Скрипты Bash
### 1. Вывести на экран 3 раза имя пользователя, от которого запускается команда.
_Для этого достаточно в командной строке написать команду с циклом for:_ 
> for n in {1..3}; do echo $USER; done

Или можно создать файл с Bash-скриптом через редактор nano
> #! /bin/bash

> for n in {1..3}

> do

>   echo $USER

> done

далее файлу необходимо дать права исполнения через команду:

> chmod u+x for_hw4.sh

и отправляем на исполнение командой:
> /.for_hw4.sh


### 2. Вывести с помощью цикла while все четные числа от 0 до 100 включительно.
Для этого создадим файл с Bash-скриптом через редактор nano:
> nano while_hw4.sh

> #! /bin/bash

> x=0

> while [ $x -lt 101 ]

> do

> if !((x % 2)); then

> printf "${x} "

#echo ${x}

> fi

> let x+=1

> done

Даем файлу права на исполнение
> chmod u+x while_hw4.sh

И отправляем на исполнение

> ./while_hw4.sh

### 3.* Командой 'cut' вывести для текущей папки права доступа файлов (первая колонка вывода команды ‘ll’). Отсортировать этот вывод (команда ‘sort’). Удалить дубликаты (команда ‘uniq’). Использовать для решения конвейер обработки задач (pipeline - вертикальный слэш).
> $ ls -all -ld /etc/* | cut -f1 -d " " | sort -u

-r--r-----
-r--r--r--
-rw-r-----
-rw-r--r--
-rwxr-xr-x
drwx------
drwxr-x---
drwxr-xr-x
drwxrwxr-x
lrwxrwxrwx

### 4.* Написать скрипт очистки директорий. На вход принимает путь к директории. Если директория существует, то удаляет в ней все файлы с расширениями .bak, .tmp, .backup. Если директории нет, то выводит ошибку.
Для этого создадим файл с Bash-скриптом через редактор nano: 
> nano delfiletodir.sh

> #! /bin/bash

> read -p "Введите путь к директории: " DELFILEFROMDIR

> if [ -e $DELFILEFROMDIR ]
>    then
>        echo 'Директория найдена'
>        cd $DELFILEFROMDIR
>        echo 'Удаление файлов...'
>        rm -v *.bak *.tmp *.backup # удаление файлов с заданными расширениями
>        echo 'Удаление завершено'
>    else
>        echo 'Для выполнения команды очистки недостаточно данных или директория не найдена'
>        exit 2
> fi 

Даем файлу права на исполнение
> chmod u+x delfiletodir.sh

И отправляем на исполнение

> ./delfiletodir.sh

