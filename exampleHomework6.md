# 4.1	Задание 1
# Настроить iptables: разрешить подключения только на 22-й и 80-й порты.
sudo iptables -L

# для MacOS с чипом M1 НЕЛЬЗЯ ВВОДИТЬ ЭТУ КОМАНДУ!
# После ввода данной команды отрубается доступ к multipass
// используем политики по умолчанию на запрещение общего входящего трафика
sudo iptables -P INPUT DROP

// разрешаем подключения по порту 80
sudo iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT

// разрешаем подключения по порту 22
sudo iptables -A INPUT -i eth0 -p tcp --dport 22 -j ACCEPT


# 4.2	Задание 2
# Настроить проброс портов локально с порта 80 на порт 8080.

sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo iptables –L

sudo iptables –L –t nat


# 4.3	Задание 3
# Запретить любой входящий трафик с IP-адреса 3.4.5.6.
sudo iptables -A INPUT -p tcp -s 3.4.5.6. -j DROP


# 4.4	Задание 4
# Запустите mc. Используя ps, найдите PID процесса, завершите процесс, передав ему сигнал 9.

# откроем новый терминал и там запустим Midnight Commander
mc

# посмотрим первые несколько процессов
ps aux | head
	Главный интерес – первая строка, чтобы знать: в какой колонке PID

# найдем процесс mc
ps aux | grep mc

# прибьем процесс с сигналом 9 (строго, без возможности сохранить данные)
kill -s 9 30779

# убедимся, что больше нет этого процесса
ps aux | grep mc
	в соседнем терминале от процесса mc остался только статический экран

# 4.1	Задание 1
# Настроить iptables: разрешить подключения только на 22-й и 80-й порты.
sudo iptables -L

// используем политики по умолчанию на запрещение общего входящего трафика
sudo iptables -P INPUT DROP

// разрешаем подключения по порту 80
sudo iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT

// разрешаем подключения по порту 22
sudo iptables -A INPUT -i eth0 -p tcp --dport 22 -j ACCEPT


# 4.2	Задание 2
# Настроить проброс портов локально с порта 80 на порт 8080.

sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo iptables –L

sudo iptables –L –t nat


# 4.3	Задание 3
# Запретить любой входящий трафик с IP-адреса 3.4.5.6.
sudo iptables -A INPUT -p tcp -s 3.4.5.6. -j DROP


# 4.4	Задание 4
# Запустите mc. Используя ps, найдите PID процесса, завершите процесс, передав ему сигнал 9.

# откроем новый терминал и там запустим Midnight Commander
mc

# посмотрим первые несколько процессов
ps aux | head
	Главный интерес – первая строка, чтобы знать: в какой колонке PID

# найдем процесс mc
ps aux | grep mc

# прибьем процесс с сигналом 9 (строго, без возможности сохранить данные)
kill -s 9 30779

# убедимся, что больше нет этого процесса
ps aux | grep mc
	в соседнем терминале от процесса mc остался только статический экран

Пример идеального решения:
Пункт 1.
Конфиг netplan:
network: version: 2 renderer: networkd ethernets:
enpOs3: dhop4: no
addresses: [192.168.0.8/24]
routes:
- to: default
via: 192.168.0.254
nameservers:
addresses:
- 8.8.8.8
- 1.1.1.1
Команда ip:
sudo ip addr add 192.168.0.9/255.255.255.0 broadcast 192.168.0.255 dev enp0s3
ping ya.ru
Пункт 2.
iptables -A INPUT -i lo -j ACCEPT iptables -A INPUT -p TCP --dport 22 -j ACCEPT iptables -A INPUT -p TCP --dport 80 -j ACCEPT iptables -A INPUT -p TCP --dport 443 -j ACCEPT iptables -A INPUT -m state --state ESTABLISHED, RELATED -j ACCEPT
iptables -P INPUT DROP
Пункт 3.
iptables -I INPUT -s 3.4.5.6 -j DROP
Пункт 4.
iptables -t nat -I PREROUTING -p tcp --dport 8090 -j REDIRECT
--to-port 80
Пункт 5.
iptables -I INPUT -p TCP --dport 22 - j DROP
iptables -I INPUT -p TCP --dport 22 -s 192.168.0.0/24 -j ACCEPT