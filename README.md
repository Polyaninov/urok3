Часть 1: Работа с процессами

    1. Вывести список процессов
          ps aux  top 
      
    2. Отфильтровать вывод так, чтобы осталься только процесс dockerd
            pmn@pmn-ubuntu:~$ ps aux | grep dockerd 
            ps aux | grep dockerd | grep -v grep

    3. Вывести информацию о потребляемой памяти и cpu для процесса nginx
         pmn@pmn-ubuntu:~$ ps aux | grep 'nginx' | awk '{print $3, $4}'

    
    4. Отобразить процессы только своего пользователя
                     top -u pmn
             
    5. Вывести в консоль запущенные процессы, отображая только их id, и того, от кого они запущены

                    pmn@pmn-ubuntu:~$ top  -eo pid,user


        
    6. Запустить процесс sleep 10000 в фоновом режиме
              pmn@pmn-ubuntu:~$ sleep 10000 &  
              pmn@pmn-ubuntu:~$ jobs  dlya proverki 

              

         
    7. Вывести процесс sleep 10000 из фонового режима

        pmn@pmn-ubuntu:~$ fg %1 sleep 10000
        fg  bnopmer 

    8. Остановить процесс sleep 10000 (остановить, а не убить)
                pmn@pmn-ubuntu:~$ killall sleep 10000

           
    9. Вывести процесс sleep 10000 из сна
                        pmn@pmn-ubuntu:~$ killall sleep 10000

    
    10 Убить процесс sleep 10000

        pmn@pmn-ubuntu:~$ kill 174172

    11. Вывести процессы, которые потребляют больше всех памяти
            pmn@pmn-ubuntu:~$ top -b -o +%MEM | head -n 22
    
    ?????В директории proc найти папку, соответсвующую процессу nginx. Найти

    лимиты процесса
    какая команда запущенна в процессе
    содержимое стэка

 12. Запустить top и отсортировать процессы по

    объему используемой памяти;
       pmn@pmn-ubuntu:~$ top -b -o +%MEM
    
    времени работы

    pmn@pmn-ubuntu:~$ top -b -o +TIME+

    
    проценту использования cpu
    pmn@pmn-ubuntu:~$ top -b -o +%CPU


    

Часть 2: Скрипты

   1.  Написать скрипт, который установит на ВМ стек

    nginx

#!/usr/bin/ bash
sudo apt -y update
sudo apt -y install nginx
sudo nginx -vl
sudo systemctl start nginx
sudo systemctl status nginx
    
    php-fpm


#!/usr/bin/ bash
sudo apt -y update
sudo apt -y install php
sudo php -v
sudo apt install php-fpm -y
sudo systemctl start php8.1-fpm
sudo systemctl status php8.1-fpm
    
    mariadb-server
    
#!/bin/bash
sudo apt install update
sudo apt install mariadb-server -y
#sudo systemctl enable mariadb-server
sudo systemctl start mariadb
sudo systemctl status mariadb
root_password=mypass
    

    2. Написать скрипт, который будет проверять работоспособность сайта (сайт нужно указать как аргумент при запуске скрипта)

    проверку можно реализовать с помощью curl и проверки ответа, отдает он код 200 или нет

    Написать скрипт для создания структуры нового проекта

    создать директорию самого проекта (должна передаваться как аргумент скрипту)
    в директории проекта создать директории
        src/: исходный код
        docs/: документация
        tests/: тесты
        resources/: ресурсы проекта
    создать файл
        readme.md : файл с описанием проекта
    вывести созданную структуру на экран
         pmn@pmn-ubuntu:~$ tree proj/

    

    3. Модифицировать скрипт проверки веб-сервера, так чтобы он мог проверять несколько адресов
    Написать скрипт для создания проекта на python:

    передать название проекта аргументом скрипту
    создать директорию проекта
    активировать venv (python3 -m venv .venv и source .venv/bin/activate)
    установить зависимости (python3 -m pip install pylint black pytest)
    создать файл .gitignore в котоом будет запись о .venv
    создать файл с зависимостями (requirements.txt)
    вывести структуру проекта

   4.  Написать скрипт, который будет проверять состояние указанных служб и перезапускать их, если они остановлены (например nginx)

    #!/bin/bash
service nginx status > output.txt
if grep -q "running" output.txt; then
    echo "The Nginx service is running"
else
    systemctl restart nginx
    echo "The Nginx service was stoped. The service has been started."
fi

    5. Написать скрипт для сбора статистики о процессах:

    об использовании cpu
    об использовании памяти
    выводить парамтеры pid, user, command и cpu/mem
    результат должен записываться в файл /var/log/process_stat.log в формате


#!/bin/bash
echo "This script monitors CPU and memory usage"

  echo "Get using CPU and Memory"
cpuUsage=$(top -bn1 | awk '/Cpu/ { print $2}')

memUsage=$(free -m | awk '/Mem/{print $3}')

Parametri=$(ps aux | awk '{print $1, $2, $3, $4, $11}')



  echo "CPU Usage: $cpuUsage%">> /var/log/process_stat.log
  echo "Memory Usage: $memUsage MB">> /var/log/process_stat.log
  echo "Memory Usage: $Parametri">> /var/log/process_stat.log








    
   5.  Написать скрипт для поиска свободных портов из диапазона, который укажет пользователь при запуске скрипта
    Написать скрипт для сбора статистики о процессах:
