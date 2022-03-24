# Домашняя работа по заданию "Работа в терминале 1"

1. Done
2. PS C:\Users\TsukanovDA-VTB\Documents\Vagrant> vagrant --version
Vagrant 2.2.17.dev
PS C:\Users\TsukanovDA-VTB\Documents\Vagrant>

3. Done
4. 
PS C:\Users\TsukanovDA-VTB\Documents\Vagrant> vagrant init
==> vagrant: A new version of Vagrant is available: 2.2.19 (installed version: 2.2.17.dev)!
==> vagrant: To upgrade visit: https://www.vagrantup.com/downloads.html

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

Предложенный вариант нерабочий, тк отсутствуют образы. Возможно закрыли доступ 
Делал иначе, по документации vagrant.
PS C:\Users\TsukanovDA-VTB\Documents\Vagrant> vagrant box add ubuntu https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/15.04/ubuntu-15.04-amd64.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'ubuntu' (v0) for provider:
    box: Downloading: https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/15.04/ubuntu-15.04-amd64.box
Download redirected to host: objects.githubusercontent.com
    box:
==> box: Successfully added box 'ubuntu' (v0) for 'virtualbox'!

config.vm.box = "bento/ubuntu-20.04" заменил на config.vm.box = "ubuntu"

vagrant up 

5. Дисковое пространство - 40 гб
   Видеопамять - 16 мб
   ОЗУ - 512 мб
6. config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
  end 
  
7. Done
8. HISTFILESIZE, строка 1155
9. Выполнение команд циклом (touch {1..10}.txt создаст 10 файлов)
10. touch {000001..100000}.txt С 300000 не вышло, слишком большое количество аргументов.
11. Проверяет условие у -d /tmp и возвращает ее статус (0 или 1), наличие катаолга /tmp
12. 
ubuntu@ubuntu:~$ mkdir /tmp/new_path_dir/
ubuntu@ubuntu:~$ cp /bin/bash /tmp/new_path_dir/
ubuntu@ubuntu:~$ type -a bash
bash is /usr/bin/bash
bash is /bin/bash
ubuntu@ubuntu:~$ PATH=/tmp/new_path_dir/:$PATH
ubuntu@ubuntu:~$ type -a bash
bash is /tmp/new_path_dir/bash
bash is /usr/bin/bash
bash is /bin/bash
  
13. at - запуск в указанное время
    batch - запуск при условии загрузки ниже 1.5
14. Done    
