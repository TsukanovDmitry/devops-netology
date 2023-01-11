Домашнее задание к занятию "08.01 Введение в Ansible"

Подготовка к выполнению

Установите ansible версии 2.10 или выше.
```
Tsukanovs-MacBook-Air:~ tsukanovdmitry$ ansible --version
ansible [core 2.13.3]
```
Создайте свой собственный публичный репозиторий на github с произвольным именем.
```
https://github.com/TsukanovDmitry/devops-ansible
```
Скачайте playbook из репозитория с домашним заданием и перенесите его в свой репозиторий.
```
done
```
Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.
```
ASK [Gathering Facts] *********************************************************
[WARNING]: Platform darwin on host localhost is using the discovered Python
interpreter at /usr/local/bin/python3.10, but future installation of another
Python interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible-
core/2.13/reference_appendices/interpreter_discovery.html for more information.
ok: [localhost]

TASK [Print OS] ****************************************************************
ok: [localhost] => {
    "msg": "MacOSX"
}

TASK [Print fact] **************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ 
```
Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
```
Tsukanovs-MacBook-Air:all tsukanovdmitry$ cat examp.yml
---
  some_fact: all default fact
Tsukanovs-MacBook-Air:all tsukanovdmitry
```
Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.
```
Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ ansible-playbook -i inventory/prod.yml -v site.yml
No config file found; using defaults

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP *********************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ 
```
Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.
```
done
```
Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.
```
Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ ansible-playbook -i inventory/prod.yml -v site.yml
No config file found; using defaults

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *********************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ 
```

При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
```
Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ ansible-vault encrypt group_vars/deb/examp.yml
New Vault password: 
Confirm New Vault password: 
Encryption successful
Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ ansible-vault encrypt group_vars/el/examp.yml
New Vault password: 
Confirm New Vault password: 
Encryption successful
Tsukanovs-MacBook-Air:playbook tsukanovdmitry$ 
```
Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.

Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.

В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.

Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.

Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.
