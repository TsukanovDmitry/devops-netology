Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"

Задача 1

Опишите своими словами основные преимущества применения на практике IaaC паттернов.
Непрерывная интеграция позволяет находить ошибки в коде из за постоянного тестирования вносимых изменений. Непрерывная доставка позволяет исправить ошибки или откатиться на предыдущее состояние системы.

Какой из принципов IaaC является основополагающим?
Получение стабильного и предсказуемого результата при запуске кода.

Задача 2

Чем Ansible выгодно отличается от других систем управление конфигурациями?
Использование ssh инфраструктуры без установки дополнительного окружения и большое количество модулей.

Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?
Я думаю что лушче pull, поскольку мы можем получить большое количество одинаковых конфигураций в определенный момент времени.

Задача 3

Tsukanovs-MacBook-Air:~ tsukanovdmitry$ ansible --version
ansible [core 2.13.2]
  config file = None
  configured module search path = ['/Users/tsukanovdmitry/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/Cellar/ansible/6.2.0/libexec/lib/python3.10/site-packages/ansible
  ansible collection location = /Users/tsukanovdmitry/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/local/bin/ansible
  python version = 3.10.5 (main, Jun 23 2022, 17:15:32) [Clang 13.0.0 (clang-1300.0.29.30)]
  jinja version = 3.1.2
  
Tsukanovs-MacBook-Air:~ tsukanovdmitry$ 

Virtual box
![image](https://user-images.githubusercontent.com/75790619/183254979-d4251bfa-df28-400b-8a42-804e3c809185.png)

Vagrant 

Tsukanovs-MacBook-Air:~ tsukanovdmitry$ vagrant --version
Vagrant 2.2.19
