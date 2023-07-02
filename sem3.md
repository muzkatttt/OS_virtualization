#################################################### Работа с пользователями

# Информация о пользователе
whoami

# В каких группах состоит текущий пользователь
groups

# Создание пользователя
sudo useradd -m -s /bin/bash user1

# Определяем пароль для пользователя
sudo passwd user1

# Параметры будут даны пользователю после создания по умолчанию
useradd -D

# Зайдём в оболочку под другим пользователем
su user1

# Проверим
whoami

# Выйти из сессии этого пользователя
exit

# удаление пользователя
whatis userdel
userdel -- help
userdel -fr user1

#заблокировать пользователя
sudo usermod –L user1
#разблокировать пользователя
sudo usermod –U user1

# дать пользователю права суперпользователя (root), добавив его в группу "sudo"
sudo usermod -G sudo user1



# переключимся в режим суперпользователя
sudo su
	В строке приглашения знак $ изменился на #

# убедимся, что мы работаем под суперпользователем
whoami

# в данном режиме не требуется для привилегированных операций каждый раз набирать префикс sudo
apt update

# выйдем из этого режима
exit

# в обычном режиме (без привилегий) команду обновления репозиториев надо выполнять с префиксом sudo, иначе получим ошибку нехватки прав
apt update
sudo apt update


# создать пользователя отдельно без пароля
sudo useradd -m -d /home/user2 -s /bin/bash user2


# зададим пароль пользователю
sudo passwd user1

# зайдем из-под него
su user1

# убедимся, что мы работаем под новым пользователем
whoami


# заглянем в файлик passwd
more /etc/passwd


# можно отфильтровать по имени пользователя
more /etc/passwd | grep ^user

# список групп
more /etc/group

# пароли (их хэши)
more /etc/shadow


# удалим с ключом  –f (force, принудительно)
sudo userdel –f user9


# создадим группу dev_team
groupadd dev_team
# проверим
more /etc/group | grep dev


# добавим в группу участника команды разработки (user11)
sudo usermod -G dev_team user11


#################################################### Права доступа
# Команда chmod
# Предоставить другим пользователям права на запись в файл header.txt
chmod o+w header.txt

# Можно менять несколько прав для ряда категорий
chmod go-rw header.txt

# Другие варианты работы с правами
chmod u+w,g+r header.txt
chmod -rw header.txt
chmod u=rwx,g=wr,o=r header.txt

# задание прав триадами цифр
•	0: (000) No permission.
•	1: (001) Execute permission.
•	2: (010) Write permission.
•	3: (011) Write and execute permissions.
•	4: (100) Read permission.
•	5: (101) Read and execute permissions.
•	6: (110) Read and write permissions.
•	7: (111) Read, write, and execute permissions.
chmod 770 header.txt
# ключ -R для задания рекурсивного изменения прав
chmod 777 -R ~/lesson3

# Изменяем владельца файла
sudo chown user1 header.txt
 

# Sticky bit нужен для запрета удаления файлов всем, кроме владельца
chmod +t filename


# установить флаг SUID
chmod u+s filename

# снять флаг SUID
chmod u-s filename


# установить флаг SGID 
chmod g+s filename

# снять флаг SGID
chmod g-s filename


#################################################### Смена владельца
# Команда chown
# Изменяет владельцев файла или папки, а также группу владельцев

# сменим владельца файла f1
sudo chown user2 f1

# сменим группу владельцев 
sudo chown :dev_team f1

# сменим одновременно владельца и группу владельцев
sudo chown student:student –R .

