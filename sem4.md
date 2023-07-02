############################################################## Условные связки команд
cat /etc/passwd || echo error

cat /etc/passwd && echo success

cat /etc/Passwd || echo error

cat /etc/Passwd && echo success

// Только после успешного выполнения первой команды будет выполнено обновление (upgrade)
sudo apt update && sudo apt upgrade

############################################################## cat
# Просмотр с помощью cat
cat test.txt 

# Создадим новый файл 
man ls > ls.txt

# Объединяем два файла
cat file1 file2
 
# Объединяем три файла
cat file1 file2 file3
 
# Объединяем и сохраняем в файл 
cat file1 file2 file3 > total

############################################################## Конвейер команд (pipes)
# Использование утилиты для подсчёта строк, слов и символов в файле
man wc

# Подсчитываем строки с использованием конвеера |
cat file1 | wc -l

# Подсчитываем строки в трёх файлах 
cat file1 file2 file3 | wc -l
 
# Подсчитываем байты в трёх файлах 
cat file1 file2 file3 | wc -c

# Получаем значения по количеству строк, слов и символов 
cat file1 file2 file3 | wc

# Подсчитываем слова 
cat file1 file2 file3 | wc -w

# Используем шаблон подстановки * 
cat file* | wc 

# Составляем более сложную команду
ls -l | grep txt | wc -l

# Сколько раз слово 'ты' встречается в поэме Мцыри?
cat mciri_poem.txt | grep -o 'ты' | wc -w


############################################################## Перенаправление потоков ввода-вывода

# отправим результат выполнения команды find в файл python_scripts.txt, а ошибки будем писать в errors.txt 
find / -type f -name *.py 1>python_scripts.txt 2>errors.txt

# пусть ошибки уходят в тот же файл, куда и результаты поиска
find / -type f -name *.py 1>all_results.txt 2>&1


#################################################################### Bash
# пример запуска скрипта
cat > script1.sh
#!/bin/bash
echo It works! # code comment sample
echo "Working dir: " $(pwd)
echo "User name: " $USER
echo "OS: " $(uname)
Ctrl+d


# Добавляем разрешение на выполнение
chmod +x script1.sh

# Проверяем
ls -l

# Запускаем
./script1.sh

#################################################################### Циклы
# Цикл for in
for char in {1..10}; do echo $char; done

for n in {1..4}
  do 
    echo "Linux является одной из самых безопасных операционных систем"
  done

# Если вам необходимо часто использовать такую конструкцию, тогда не помешает 
# создать соответствующую bash-функцию (добавьте в ~/.bashrc)
function run() {
	number=$1
	shift
	for n in $(seq $number); do
  	$@
	done
}

# run 5 <КОМАНДА>

# Пример
run 5 echo "Я умею использовать операционную систему Windows Server Standart 2012 R2"

#################################################################### Условия
# скрипт, проверяющий введенный возраст и выдающий соответствующее приветствие
cat > age_check
#!/bin/bash
if [ $1 == 18 ]; then
	echo "Предъявите документы"
elif [ $1 -lt 18 ]; then
	echo "Вход только для взрослых"
elif [ $1 -gt 18 ]; then
	echo "Добро пожаловать"
fi

# запустим скрипт с разными параметрами
bash age_check 10
bash age_check 18
bash age_check 20

#################################################################### Условия
nano burger_cost

#!/bin/bash
case $1 in
  10)
    echo "Дайте два!"
    ;;
  100)
    echo "Хорошо,  спасибо"
    ;;
  50 | 60)
    echo "Колу со льдом, пожалуйста!"
    ;;
  *)
    echo "Что, простите?"
    ;;
esac

# запуск
bash burger_cost 10
bash burger_cost 50
bash burger_cost 100
bash burger_cost asdfas

#################################################################### Циклы
# Цикл while
# Цикл while будет в работе, пока условие, заданное в списке команды 1, будет верным

y=1
while [ $y -lt 10 ]
do
  echo $y
  y=$(( $y + 1 ))
done


#################################################################### Цикл until
# Пример цикла until

#!/bin/bash
POINTER=20
until [ $POINTER -lt 10 ]
do
  echo POINTER $POINTER
  let POINTER-=1
done


#################################################################### Цикл until
# Пример работы с файлами

#!/bin/bash
directory=$1
if [ -e $directory ] then
 	hidden_count=$(ls -A $directory | grep '^\.' | wc -l)
 	echo “Hidden files in $directory found: $hidden_count”
	else     	echo "Error. No such directory."
fi 
if [ -z $directory ] then
        echo "The string is empty." #если строка пустая, сообщаем об ошибке        
		exit # завершаем скрипт 
else
       echo $directory # в противном случае выводим на экран значение параметра
fi
