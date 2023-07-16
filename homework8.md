# Домашнее задание к семинару 8. Запуск веб-приложения из контейнеров

## 1. Переустановить операционную систему (по желанию, для дополнительной практики)
При выполнениии домашнего задания к семинару 6 переустановлена ОС.

## 2. Установить Docker.

Сначала подключим репозиторий docker:
> sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

Установим curl:
> sudo apt install curl

добавляем репозиторий docker:
> sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

Проверяем обновления:
> sudo apt update

Устанавливаем docker:
> sudo apt install docker -y
> sudo apt install docker.io -y

docker - админская утилита, которая всегда запускается от имени root, чтобы избавиться от необходимости ввода sudo перед каждой командой, можно перейти в режим root, для этого создать группу docker (sudo groupadd docker), в группу docker добавить данного пользователя (sudo usermod -aG docker student или командой sudo usermod -aG docker $USER), через команду (tail /etc/group) проверяем добавлен ли пользователь в группу docker. Далее перезапускаем docker (sudo systemctl restart docker) и перечитываем информацию о членстве юзера в группе docker(newgrp docker). Далее в консоли можно писать команды без sudo.

Ищем nginx:
> docker search nginx

Cкачаем образ:
> docker pull nginx

## 3. Запустить контейнер с Ubuntu.

Запускаем с помощью админской команды:
> sudo docker run -d ubuntu

Проверить историю запуска контейнеров:
> sudo docker ps -a

## 4. * Используя Dockerfile, собрать связку nginx + PHP-FPM в одном контейнере.

Через редактор nano создаем файл Dockerfile с содержанием:

> FROM ubuntu:latest
> ARG DEBIAN_FRONTEND=noninteractive 
> MAINTAINER GB_student
> RUN apt-get update
> RUN apt-get install nginx -y
> RUN apt-get install php-fpm -y
> VOLUME "/var/www/html"
> EXPOSE 80
> CMD /usr/sbin/nginx -g "daemon off;" ; php-fpm -D

Посмотреть содержимое:
> cat Dockerfile

Установить 
> sudo docker build -t php_fpm .

Смотрим список запущенных образов:
> sudo docker images

Настраиваем запуск докера (используем ключи -d это detouch -p пробросить порты на 9000 с 80)
> sudo docker run -d -p 9000:80 nginx

порт 9000 занят процессом, перевесила на порт 9001:
> sudo docker run -d -p 9001:80 nginx

Проверим список пакетов:
> sudo docker ps

Останавливаем пакет:
sudo docker stop 1c2aa15b3952
