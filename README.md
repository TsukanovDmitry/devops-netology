# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Никакого, поскольку интерпретарор выдаст ошибку сложения строкового типа с числовым  |
| Как получить для переменной `c` значение 12?  | ???  |
```python
a = '1'
b = '2'
c = a + b

#######################

a = 1
b = '2'
c = str(a) + b
```
| Как получить для переменной `c` значение 3?  | ???  |

```python
c = a + int(b)
```

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```
Работа старого скрипта:
![image](https://user-images.githubusercontent.com/75790619/174469166-1519e345-00d3-42e8-be29-2f14d043806d.png)


### Ваш скрипт:
```python
#!/usr/bin/env python3
import os

git_dir = '~/netology/sysadm-homeworks'
bash_command = [f"cd {git_dir}", "git status"]
result_git = os.popen(' && '.join(bash_command)).read()

for string in result_git.split('\n'):
  if string.find('modified') != -1:
     result = git_dir + '/' + string.replace('modified:', '').strip()
     print(result)
```

### Вывод скрипта при запуске при тестировании:
```
root@devops-netology:~# root@devops-netology:~# python3 python.py
~/netology/sysadm-homeworks/foo/bar.md
~/netology/sysadm-homeworks/readme.md
~/netology/sysadm-homeworks/some_dir/test_file.md
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import os
import sys


try:
  git_dir = sys.argv[1]
except:
  print("Incorrect path to repo")


bash_command = [f"cd {git_dir}", "git status 2>1"]
result_git = os.popen(' && '.join(bash_command)).read()

if result_git == '':
  print('This is not a git repo!')

for string in result_git.split('\n'):
  if string.find('modified') != -1:
     result = git_dir + '/' + string.replace('modified:', '').strip()
     print(result)
```

### Вывод скрипта при запуске при тестировании:
```
root@devops-netology:~# python3 python.py /root/netology/sysadm-homeworks
/root/netology/sysadm-homeworks/foo/bar.md
/root/netology/sysadm-homeworks/readme.md
/root/netology/sysadm-homeworks/some_dir/test_file.md
root@devops-netology:~# python3 python.py /root
This is not a git repo!
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3
import socket
from string import whitespace

hosts = ["drive.google.com", "mail.google.com", "google.com"]
fileList = []

with open('hosts.log') as file:
    for f in file:
        fileList.append(f)

with open('hosts.log', 'w+') as file:
    for i in hosts:
        result = socket.gethostbyname(i)
        added = 0
        for y in fileList:
            inList = y.find(" {}".format(i))
            if (inList != -1):
                ipstr=y.replace('\n', '').split("  ")[1].translate({None: whitespace})
                if (ipstr == result):
                    print(" {}  {}\n".format(i, result))
                    file.write(" {}  {}\n".format(i, result))
                    added = 1
                    break
                else:
                    print("[ERROR] {} IP mismatch: {}  {}\n".format(i, ipstr, result))
                    file.write("[ERROR] {} IP mismatch: {}  {}\n".format(i, ipstr, result))
                    added = 1
                    break
        if (added == 0):
            print(" {}  {}\n".format(i, result))
```

### Вывод скрипта при запуске при тестировании:
```
root@devops-netology:~# python3 python.py
 drive.google.com  64.233.164.194

 mail.google.com  64.233.161.17

 google.com  173.194.73.138

root@devops-netology:~# python3 python.py
 drive.google.com  64.233.164.194

[ERROR] mail.google.com IP mismatch: 64.233.161.17  64.233.161.83

[ERROR] google.com IP mismatch: 173.194.73.138  173.194.73.113
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так получилось, что мы очень часто вносим правки в конфигурацию своей системы прямо на сервере. Но так как вся наша команда разработки держит файлы конфигурации в github и пользуется gitflow, то нам приходится каждый раз переносить архив с нашими изменениями с сервера на наш локальный компьютер, формировать новую ветку, коммитить в неё изменения, создавать pull request (PR) и только после выполнения Merge мы наконец можем официально подтвердить, что новая конфигурация применена. Мы хотим максимально автоматизировать всю цепочку действий. Для этого нам нужно написать скрипт, который будет в директории с локальным репозиторием обращаться по API к github, создавать PR для вливания текущей выбранной ветки в master с сообщением, которое мы вписываем в первый параметр при обращении к py-файлу (сообщение не может быть пустым). При желании, можно добавить к указанному функционалу создание новой ветки, commit и push в неё изменений конфигурации. С директорией локального репозитория можно делать всё, что угодно. Также, принимаем во внимание, что Merge Conflict у нас отсутствуют и их точно не будет при push, как в свою ветку, так и при слиянии в master. Важно получить конечный результат с созданным PR, в котором применяются наши изменения. 

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```
