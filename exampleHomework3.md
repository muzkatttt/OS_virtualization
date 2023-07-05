####################################################### Разбор ДЗ
# 1. Управление пользователями:
# a)	создать нового пользователя;
# b)	убедиться, что информация о нем появилась в соответствующих файлах в системе;
# c)	удалить созданного пользователя;

# создадим пользователя с домашней папкой и оболочкой Bash
sudo useradd -s /bin/bash -d /home/user11 -m user11
sudo adduser user22

# проверим успешное создание пользователя
tail /etc/passwd
tail /etc/group
sudo tail /etc/shadow

# зададим ему пароль отдельной командой
sudo passwd user11

# проверим появление хэша пароля
sudo tail /etc/shadow

# удалим юзера вместе с его домашней папкой (ключ -r)
sudo userdel user11 -r

# проверим успешное удаление
tail /etc/passwd
tail /etc/group
tail /etc/shadow


# 2. Управление группами:
# a) создать группу;
# b) попрактиковаться в смене групп у пользователей;
# c) добавить пользователя в группу, не меняя основной;
# d) удалить пользователя из группы.

# создадим новых менеджеров
sudo useradd -s /bin/bash -d /home/manager1 -m manager1
sudo useradd -s /bin/bash -d /home/manager2 -m manager2	

# создадим новых тестировщиков
sudo useradd -s /bin/bash -d /home/tester1 -m tester1
sudo useradd -s /bin/bash -d /home/tester2 -m tester2	

# создадим группы для каждого отдела
sudo groupadd managers
sudo groupadd testers	
# проверим группы
tail /etc/group

# добавим сотрудников в группы своих отделов
sudo usermod -a -G testers tester1
sudo usermod -a -G testers tester2
sudo usermod -a -G managers manager1
sudo usermod -a -G managers manager2

# проверим группы
tail /etc/group
groups tester1

# удалить пользователя из всех групп, кроме основной
sudo usermod -G "" tester1
# добавим его в целевую группу
sudo usermod -a -G managers tester1
# проверим
groups tester1

# добавим пользователя manager1 в группу testers
sudo usermod -a -G testers manager1
# проверим
groups manager1

# удалим пользователя manager1 из группы testers
sudo deluser manager1 testers
# проверим
tail /etc/group


# 3. Работа с группами.
# a) Создать пользователя с правами суперпользователя. Проверить результат.

# создадим нового админа
sudo useradd -s /bin/bash -d /home/ladmin -m ladmin

# проверим группы админа
groups ladmin

# добавим его в целевую группу
sudo usermod -a -G sudo ladmin

# проверим группы админа
groups ladmin


# b) * Создать группу developer и нескольких пользователей, входящих в неё. Создать директорию для совместной работы. Сделать так, чтобы созданные одними пользователями файлы могли изменять другие пользователи этой группы.

groupadd developers
adduser user1
adduser user2
adduser user3
usermod –aG developers user1
usermod –aG developers user2
usermod –aG developers user3

mkdir dev_dir
chmod g=rw dev_dir/
chmod g+s dev_dir/



# c) * Создать в директории для совместной работы поддиректорию для обмена файлами, но чтобы удалять файлы могли только их создатели.

mkdir shared_dir
chmod +t shared_dir



