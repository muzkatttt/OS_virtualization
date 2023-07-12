###################################################### Nginx
// установка
sudo apt install nginx -y

// проверка конфигов, статуса
sudo nginx -t
systemctl status nginx
sudo ss -ntlp

// конфиги тут
ll /etc/nginx
nano /etc/nginx/nginx.conf

// применение конфигов (если меняем что-то)
sudo systemctl reload nginx


# Конфиги сайта по умолчанию
nano /etc/nginx/sites-enabled/default


###################################################### Apache 
// Установка
sudo apt install apache2 -y

// проверка
sudo apachectl -t
sudo systemctl status apache2
sudo ss -ntlp

// конфиги тут
ll /etc/apache2
nano /etc/apache2/apache2.conf

// применение конфигов (если меняем что-то)
sudo systemctl reload apache2


// запустим apache
sudo systemctl start apache2


// правим сайт на порт 8080
sudo nano /etc/apache2/sites-enabled/000-default.conf
 

###################################################### Reverse proxy

// настройки тут
sudo nano /etc/nginx/sites-enabled/default

// проверим, перезапустим
sudo nginx –t
sudo systemctl reload apache2


###################################################### PHP-FPM
// установка
sudo apt install libapache2-mod-php8.1 php8.1 -y

//путь к конфигам
cd /etc/apache2/mods-enabled/
ll
nano php8.1.conf

// установка
apt install php8.1-fpm
cd /etc/php/8.1/
ll

cd fpm/pool.d
ll
nano www.conf


###################################################### MySQL
// установка
sudo apt install mysql-server-8.0 -y
// проверка
systemctl status mysql

// вход на MySQL сервер
sudo mysql

// список БД
show databases;

// переключиться на выбранную БД
use mysql;

// прочитать все из таблицы user
SELECT * FROM user\G;

// создать БД
CREATE DATABASE gb;

// показать таблицы
SHOW TABLES;

// создать таблицу users
CREATE TABLE users(id INT, email VARCHAR(100));

// ставить новые строки в таблицу
INSERT INTO users VALUES (1, 'ivan@mail.ru'),(2, 'user2@mail.ru'),(3, 'user3@mail.ru');