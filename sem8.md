// ставим докер
sudo apt install docker.io -y
// или (разница не существенна)
sudo apt install docker -y


# посмотрим в справке основные команды
docker --help
 

# Списки контейнеров и образов
sudo docker ps
sudo docker images


# запустим hello world
sudo docker run hello-world


# история всех контейнеров
sudo docker ps -a


#	оф. доки рекомендуют знакомиться с докером с запуска этого контейнера
# запустим контейнер с названием getting-started
docker run -dp 80:80 docker/getting-started
	если его локально нет, то его образ скачается и тогда уже контейнер запустится
	заходим в браузер по пути http://localhost:80

# удалим контейнер, указав его ID
docker stop 4fc0289cdd93


# пустим веб сервер [1:05]
// поищем
docker search nginx
// скачаем образ
docker pull nginx

# запустим веб сервер nginx на докере с пробросом портов
docker run -d -p 8123:80 nginx # 80 - порт на локальной машине, 8123 - на внешней машине
	Теперь в браузере заходим на веб сервер по адресу: http://localhost:8123

командой можно запустить контейнер с документацией:
> docker run -d -p 80:80 docker/getting-started


# войдем в докер, найдем HTML-файлы
docker exec -it jolly_meitner /bin/bash

// найдем файл-заглушку на вебсервере
find / -name "index.html"

// выход из контейнера
exit


# Dockerfile
# напишем инструкции для сборки нового образа:
nano Dockerfile

FROM ubuntu:latest
MAINTAINER GB_student
RUN apt-get update
RUN apt-get install nginx -y
VOLUME "/var/www/html"
EXPOSE 80
CMD /usr/sbin/nginx -g "daemon off;"


# соберем образ из инструкций в Dockerfile
docker build -t my_nginx_image .

# увидеть наш новый образ можно так:
docker images

# запускаем наш образ на порту 8080
docker run -d -p 9000:80 my_nginx_image

 

# Docker-compose
// устанавливаем
sudo apt install docker-compose -y

// создадим веб-сайт на Wordpress
mkdir wordpress_site
cd wordpress_site

nano docker-compose.yml

wordpress:
    image: wordpress
    links:
     - mariadb:mysql
    environment:
     - WORDPRESS_DB_PASSWORD=password
     - WORDPRESS_DB_USER=root
    ports:
     - "public_ip:80:80"
    volumes:
     - ./html:/var/www/html
mariadb:
    image: mariadb
    environment:
     - MYSQL_ROOT_PASSWORD=password
     - MYSQL_DATABASE=wordpress
    volumes:
     - ./database:/var/lib/mysql
	 
sudo docker-compose up -d
	http://localhost:80

docker ps

docker images


# Удаляем все
docker rm 860aba9898c5

// или так
docker rm $(docker ps -aq)

// удаление образа
docker rmi –f e1f77663ea39

// удалить все образы
docker rmi $(docker images -q)

