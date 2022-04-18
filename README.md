# Домашнее задание по лекции "Операционные системы (лекция 1)"

1. Скачал tar.gz архив, распаковал. 
Создаем юнит файл 
![image](https://user-images.githubusercontent.com/75790619/163824230-63f247d6-fdde-42e6-8512-a8104ba66828.png)

Добавляем в автозагрузку:
sudo systemctl enable node-exporter

Рестартуем, проверяем что служба запустилась

![image](https://user-images.githubusercontent.com/75790619/163824412-2f1e913b-3317-4911-a2cb-4d396ec65aee.png)

Создаем файл с дополнительной переменной окружения, в каталоге, который прописан в unit

Проверяем что переменная применяется
![image](https://user-images.githubusercontent.com/75790619/163824860-98d64285-52bc-4624-99e8-bbd21679760d.png)


2. node_cpu_seconds_total
   node_filesystem_avail_bytes
   node_network_receive_bytes_total
   node_memory_Active_bytes
   
3. Done
     ![image](https://user-images.githubusercontent.com/75790619/163826910-8a96d2ff-59a1-46cd-95a7-24530505703a.png)

4. Да, даже понимает какая система виртуализации
devops@devops-netology:~$ dmesg | grep virt
[    0.016116] Booting paravirtualized kernel on VMware hypervisor
[    4.618119] systemd[1]: Detected virtualization vmware.

5. 
devops@devops-netology:~$ /sbin/sysctl -n fs.nr_open
1048576

Это максимальное число открытых дескрипторов для системы.

Я так понимаю речь о мягких и жестких лимитах

devops@devops-netology:~$ ulimit -Sn
1024
devops@devops-netology:~$ ulimit -Hn
1048576

6.
root@devops-netology:/# unshare -f --pid --mount-proc sleep 1h
^Z
[1]+  Stopped                 unshare -f --pid --mount-proc sleep 1h
root@devops-netology:/# ps aux | grep sleep
root        2144  0.0  0.0   5480   580 pts/0    T    15:13   0:00 unshare -f --pid --mount-proc sleep 1h
root        2145  0.0  0.0   5476   580 pts/0    S    15:13   0:00 sleep 1h
root        2153  0.0  0.0   6432   724 pts/0    S+   15:14   0:00 grep --color=auto sleep
root@devops-netology:/# nsenter -t 2145 -p -m
root@devops-netology:/# ps
    PID TTY          TIME CMD
      1 pts/0    00:00:00 sleep
      2 pts/0    00:00:00 bash
     13 pts/0    00:00:00 ps
root@devops-netology:/#

7. Это функция, которая рекурсивно вызывает сама себя, пока не забьются ресурсы системы. 

Сработал механизм сgroups. Дефолтные параметры можно глянуть командой unlimit -a.

Переназначение параметров происходит в этом файле -  /etc/security/limits.conf
