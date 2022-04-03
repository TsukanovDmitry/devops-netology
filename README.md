# Домашнее задание по лекции "Операционные системы (лекция 1)"

1. Я так понимаю что - chdir("/tmp")
2. openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
3. Получилось только с редактором vim 

vi log

string 
string
string


(В другой консоли)

devops@devops-netology:~$ rm .log.swp
devops@devops-netology:~$ lsof | grep deleted
vi        35754                          devops    3u      REG              253,0    12288     655929 /home/devops/.log.swp (deleted)

3u - дескриптор. Его надо удалить как я понял
(gdb) call close(999)
$1 = -1

Далее 

devops@devops-netology:~$ echo '' >/proc/35754/fd/3


4. Нет, они только место в таблице процессов занимают))
5. ![image](https://user-images.githubusercontent.com/75790619/161430941-330037b1-09ee-47eb-93d3-303e3dbca18b.png)
6. uname()
Цитата :
     Part of the utsname information is also accessible  via  /proc/sys/ker‐
       nel/{ostype, hostname, osrelease, version, domainname}.
7. && - оператор условия. Команда выполнится даже если первая завершилась неуспешно
   ; - разделитель команд, выполняющихся последовательно. Команда 2 выполнится если успешно выполнилась Команда 1
   set -e - прерывает сессию при любом ненулевом значении исполняемых команд в конвеере кроме последней.
   в случае &&  вместе с set -e- вероятно не имеет смысла, так как при ошибке , выполнение команд прекратиться. 
8. 
  -e прерывает выполнение исполнения при ошибке любой команды кроме последней в последовательности 
  -x вывод трейса простых команд 
  -u неустановленные/не заданные параметры и переменные считаются как ошибки, с выводом в stderr текста ошибки и выполнит завершение неинтерактивного вызова
  -o pipefail возвращает код возврата набора/последовательности команд, ненулевой при последней команды или 0 для успешного выполнения команд.

9. devops@devops-netology:~$ ps -o stat
  STAT
  S* - interruptible sleep (waiting for an event to complete)
  R* - running or runnable (on run queue)
