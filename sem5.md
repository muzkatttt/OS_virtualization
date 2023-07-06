#######################################################	apt / apt-get
# получить список обновлений для всех пакетов ОС
sudo apt update

# узнать: кому именно нужны обновления
sudo apt list --upgradable

# запустить обновление
sudo apt upgrade


# Установка программ
sudo apt-get install zip
sudo apt install tree

# Удаление программ
# удалим пакет zip
sudo apt-get remove zip

# можно удалить пакет вместе с конфигами:
sudo apt-get purge zip

# посмотрим все пакеты на машине Linux
dpkg -l

# посмотрим инфо о пакете tree
dpkg –l tree

# посмотрим количество установленных пакетов
dpkg -l | wc -l
 
# Фильтрация списка ПО конвейером
dpkg –l | grep pyhton3


# snap
# если snap не установлен, то это можно сделать
sudo apt install snapd

# поиск пакета в snap
sudo snap find opera

# установим браузер Опера через snap
sudo snap install opera

# посмотрим список установленных snap-пакетов
snap list

# удалим Оперу через snap
snap remove opera

#######################################################	Работа с репозиториями
# посмотрим содержимое файла со списком репозиториев
cat /etc/apt/sources.list

# apt-add-repository - команда для добавления репозиториев
apt-add-repository --help


#################################################################### Cron
# справка
crontab --help

# просмотр списка заплалнированных команд текущего пользователя
crontab -l

# редактирование списка задач текущего пользователя
crontab –e

# удаление списка задач текущего пользователя
crontab -r

# шпаргалка по синтаксису
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * [user-name]  command to be executed

# сайт тренажера
https://crontab.guru/

# примеры cron из методички
# Выполнять задание в 19 часов 5 минут 15 мая, если это пятница
5 19 15 5 5 /var/www_mysite/myssqlss.pl

# Выполнять задание раз в два часа в 10 минут текущего часа 
# (то есть в 00:10, 02:10, 04:10 и т.д.)
10 */2 * * * /var/www_mysite/mysql_script.pl

# Выполнять задание каждые десять часов в 10 минут текущего часа 
# (то есть в 00:10, 10:10 и т.д.)
10 */10 * * /var/www_mysite/exescripts.pl

# Выполнять задание по воскресеньям в 12 часов 26 минут:
26 12 * * 0 /var/www_mysite/myscript.sh

# Выполнять в 10 утра каждый день:
0 10 * * * /var/www_mysite/myscript.sh

# путь к файлу с задачами cron для текущего пользователя
sudo cat /var/spool/cron/crontabs/student

# системные задачи лежат в файле
cat /etc/crontab

# просмотр логов 
grep cron /var/log/syslog
