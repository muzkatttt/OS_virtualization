# Задание 1
# Установить Nginx
// установка
sudo apt install nginx -y

// проверка конфигов
sudo nginx -t


# Задание 2
# * настроить Nginx на работу с PHP-FPM.

	Есть в скрипте/видео записи с прошлого урока.


# Задание 3
# Установить Apache.
//
sudo apt install apache2 -y

//
sudo apachectl -t


# Задание 4
# * Настроить обработку PHP. Добиться одновременной работы с Nginx.

	Есть в скрипте/видео записи с прошлого урока.


# Задание 5
# Настроить схему обратного прокси для Nginx (динамика - на Apache).

	Есть в скрипте/видео записи с прошлого урока.


# Задание 6
# Установить MySQL. Создать новую базу данных и таблицу в ней.
sudo apt install mysql-server-8.0 -y
systemctl status mysql


# Задание 7
# * Установить пакет phpmyadmin и запустить его веб-интерфейс для управления MySQL.
// установка модуля расширения php-mbstring. 
sudo apt install php-mbstring -y

// установка, собственно, phpmyadmin
sudo apt install phpmyadmin -y



