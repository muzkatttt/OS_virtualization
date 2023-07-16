Разбор ДЗ-8

# 2. Подключить репозиторий Docker.
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

sudo apt install curl
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update

sudo apt install docker -y

sudo apt install docker.io -y


# 3. Запустить контейнер с Ubuntu.
sudo docker run -d ubuntu


# 4. * Используя Dockerfile, собрать связку nginx + PHP-FPM в одном контейнере.

cat > Dockerfile
FROM ubuntu:20.04
MAINTAINER GB_student
RUN apt-get update
RUN apt-get install nginx -y
RUN apt-get install php-fpm -y
RUN mkdir /run/php-fpm
VOLUME "/var/www/html"
EXPOSE 80
CMD /usr/sbin/nginx -g "daemon off;" ; php-fpm -D

sudo docker build -t php_fpm .

sudo docker images

