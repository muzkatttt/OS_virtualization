# Домашнее задание к семинару 7. Запуск стека для веб-приложения
1. Установить Nginx 

Для того, чтобы установить Nginx, достаточно выполнить команду:

> sudo apt -y install nginx

После установки пакета, проверяем что Nginx успешно запустился при помощи команды:

> sudo systemctl status nginx

nginx расположен /etc/nginx


2. *Настроить Nginx на работу с PHP-FPM.

Сначала устанавливаем  php:

> sudo apt install php

установим библиотеку для связи apache2 с PHP

> sudo apt install libapache2-mod-php

Далее переходим в директорию /etc/apache2/mods-enabled/:

> cd /etc/apache2/mods-enabled/

через команду ls -all проверяем наличие файлов "php8.1.conf..." в директории
переходим в директорию /var/www/html/

Создадим php-файл, для этого переключимся в режим root:

> sudo su

И создадим файл:
> nano info.php

>> <?php
>> 	    phpinfo();
>> ?>
Если в браузере ввести команду localhost/info.php, то появится страница PHP (на 80 порту отвечает Apache 2.0 Handler, то есть реверс-proxy заработал).

В режиме root установим php-fpm:

> apt install php-fpm

выходим из режима root-пользователя командой exit

переходим в директорию /etc/php/8.1/
> cd /etc/php/8.1/

В ней находятся файлы с инфраструктурой php (проверить через ввод команды ls -all).
Далее перейдем в папку fpm/pool.d/, далее командой ls -all найдем файл www.conf, его откроем через редактор nano:
> cd fpm/pool.d/
> pwd
> ls -all
> nano www.conf (интересует настройка 
listen = /run/php/php8.1-fpm.sock, 
копируем ее и далее переходим в директорию /etc/nginx/sites-enabled/):
> cd /etc/nginx/sites-enabled/

Далее через команду ls -all находим default и через редактор nano его открываем:
>  sudo nano default

находим в нем location и раскомментируем строку location ~ \.php$ { (начало)
и строку } (конец)
и после строки 
>> fastcgi_pass unix:/run/php/php7.4-fpm.sock; 
сразу вводим путь 
>> /run/php/php8.1-fpm.sock;
bash ~~~

    # pass PHP scripts to FastCGI server
        #
        location ~ \.php$ { # раскоментируем эту строку 
                include snippets/fastcgi-php.conf; # раскоментируем эту строку тоже
        #
        #       # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/run/php/php8.1-fpm.sock; # раскоментируем эту строку и добавим путь из файла www.conf
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        } # раскоментируем эту строку 
 
Далее сохраняем файл и выходим из него. Проверим настройки через команду:
> sudo nginx -t

Если выходим сообщение "nginx: the configuration file /etc/nginx/nginx.conf syntax is ok", значит, все сделано правильно.

Далее выполняем команду reload
> sudo systemctl reload nginx

Проверяем статус php:
> systemctl status php*

Служба php запущена успешно, если выводится сообщения "active (running)"...

3. Установить Apache. 

Установить Apache:
> sudo apt install apache2 -y

после установки apache2 проверяем статус через команду:

> systemctl status apache2

Порт 80 занят nginx, поэтому apache2 не смог запуститься.

4. *Настроить обработку PHP. Добиться одновременной работы с Nginx.

Для того, чтобы настроить работу Apache2 одновременно с Nginx, необходимо распределить их на разные порты. сначала остановим nginx:
> sudo systemctl stop nginx

проверим статус выполнения команды:
> systemctl status nginx

После того, как убедимся, что команда выполнена - проверим, каким процессом заняты порты:
> sudo ss -ntlp

Если порт 80 свободен, то проверяем статус apache2:
> sudo systemctl status apache2

запускаем apache2:

> sudo systemctl start apache2

Проверяем статус, далее проверяем, каким процессом занят 80 порт (apache2 должен занять порт 80).

перевесим apache2 на порт 9000:
> sudo nano ports.conf

В конфигурационном файле ports.conf меняем порт 80 на порт 9000

В конфигурационном файле sites-available/000-default.conf
меняем настройку порта с 80 на 9000(<VirtualHost *:9000>)

проверяем статус:
> sudo apachectl -t

перезапускаем apache2 (перезагружаем):
> sudo systemctl reload apache2

после перезагрузки можно проверить какие процессы знимают какие порты:

> sudo ss -ntlp

Как результат, apache2 висит на порту 9000, запустим nginx на 80 порт:
> sudo systemctl start nginx

Проверяем статус nginx:
> sudo systemctl status nginx

Смотрим порты:
> sudo ss -ntpl

Как результат, nginx занял порт 80, apache2 занял порт 9000 (Можно в браузере проверить заголовки ответа через F12 - там будет указан nginx). 
На консольной машине MacOS проверить данное решение через браузер трудновыполнимо, поэтому верю на слово (когда-нибудь проверю в профессиональной деятельности).

5. Настроить схему обратного прокси для Nginx (динамика - на Apache).

Сначала настроим nginx, чтобы он пробрасывал запросы дальше. Для этого в конфигурационном файле sites-enabled/default.conf подправим: внизу файла location /
закомментируем строку try_files $uri $uri/ =404; вместо нее напишем 
>   proxy_pass http%//localhost:9000;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;


> sudo nano /etc/nginx/sites-available/default

проверяем работоспособность nginx:
> sudo nginx -t

reload nginx:
> sudo systemctl reload nginx

6. Установить MySQL. Создать новую базу данных и таблицу в ней.

Установить mysql:
> sudo apt install mysql-server 

Проверяем статус:
> systemctl status mysql 

или через команду:
>sudo ss -ntlp

Порт 3306 занят mysql.
Переходим на сервер mysql:

> sudo mysql -u root -p

(-u это user, -p это пароль password). У mysql тоже есть пользователь root.
Если появилась строка mysql>, значит, мы подключились на сервер mysql и можно вводить команды sql. Для выхода из mysql> достаточно ввести команду exit. Вводить в конце команды точку с запятой --> ;

Снова заходим на сервер mysql и вводим команды (для ознакомления):

> show databases; # показывает список баз данных, размещенных на сервере 

> select version(); # покажет версию mysql

> use sys; # переключиться на конкретную базу данных sys

> show tables; # показать таблицы 

> use mysql; # переключиться на БД mysql

> select * from user; # прочитать содержимое таблицы

Если требуется просмотреть содержимое таблицы в удобном формате по ширине терминала, то вводим команду:

> select * from user\G;

Создать свою БД:
> create database geekbrains; # где geekbrains - это название БД

Посмотреть содержимое БД geekbrains:
> show databases;

В списке БД проверяем созданную БД:
> use geekbrains;
> show tables;

Добавим таблицу в БД:
> create table students (id INT, email VARCHAR(100));

> show tables;

> SELECT * FROM students;

> INSERT students (id) VALUES (1, 'muz@mail.ru'),(2, 'loc@mail.ru'),(3, 'tree@mail.ru); # вставить новые строки в таблицу

> SELECT * FROM students;

7. *Установить пакет phpmyadmin и запустить его веб-интерфейс для управления MySQL.
