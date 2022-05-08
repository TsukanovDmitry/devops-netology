Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

1. ![image](https://user-images.githubusercontent.com/75790619/167303102-580504b3-3426-4f6d-aceb-81ddadd2a055.png)

Код 301 (Moved Permanently) используется, когда ресурс был перемещен в другое место, и текущие ссылки должны быть обновлены.

2. Первый статус ответа - 307 (Internal Redirect).
![image](https://user-images.githubusercontent.com/75790619/167303254-3bd090d4-d7a9-4a27-9b61-bba4839a4c7f.png)
![image](https://user-images.githubusercontent.com/75790619/167303298-afb8c38f-c3dc-440a-963d-466adf1d9029.png)
Вообще самый долгий запрос был вызван каким-то js скриптом, и он выполнсялся 4.2 сек и упал по таймауту. 
Но думаю тут речь идет про загрузку страницы stackowerflow.com - 373 ms.

3. 
devops@devops-netology:~$ wget -qO- eth0.me
91.236.176.16
4.
devops@devops-netology:~$ whois 91.236.176.16 | grep org-name*
org-name:       LLC "City-Telekom"

Какой автономной системе AS? AS23242

devops@devops-netology:~$ whois 91.236.176.16 | grep AS*
% Abuse contact for '91.236.176.0 - 91.236.179.255' is 'admin@city-t.ru'
org:            ORG-LA323-RIPE
admin-c:        AM45345-RIPE
status:         ASSIGNED PI
organisation:   ORG-LA323-RIPE
abuse-c:        AR51811-RIPE
person:         Aleksey Markin
nic-hdl:        AM45345-RIPE
% Information related to '91.236.176.0/24AS23242'
origin:         AS23242
mnt-by:         COMSTAR-MNT

5. Почему то запрос не идет дальше gateway. Не смог разобраться почему

devops@devops-netology:~$ traceroute -An 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.106.2 [*]  6.191 ms  5.939 ms  5.846 ms
 2  * * *
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *

6. ![image](https://user-images.githubusercontent.com/75790619/167304101-1e5e1117-270c-4484-9df7-71c0b193650d.png)
7. NS запись
![image](https://user-images.githubusercontent.com/75790619/167304147-b28385ef-0419-42b3-8b0d-be153d6d9ad0.png)

A запись
![image](https://user-images.githubusercontent.com/75790619/167304215-92cf30da-44a5-4b88-95f2-ff337bcba617.png)

8. ![image](https://user-images.githubusercontent.com/75790619/167304318-c602e497-1894-4958-a4a6-a18f71c94a7f.png)



