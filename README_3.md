Задача 1. Выбор инструментов.

Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?
Неизменяемый. При частых релизах лучше использовать связку Packer + Terraform. К тому же сказано, что команда привыкла использовать Docker

Будет ли центральный сервер для управления инфраструктурой?

Нет. Инструменты, которые планируется использовать, не требуют центрального сервера.

Будут ли агенты на серверах?

Нет. Для обновления конфигураций будет использоваться ssh соединение

Будут ли использованы средства для управления конфигурацией или инициализации ресурсов?

Да, Terraform и Ansible

Какие инструменты из уже используемых вы хотели бы использовать для нового проекта?

Packer, Terraform, Docker, Kubernetes, Ansible.

Хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта?

Я бы заменил TeamCity в пользу Jenkins, поскольку он обладает более гибкой настройкой и простым интерфейсом. Хотя это может дело вкуса)

Задача 2. Установка терраформ.

Уже где то было в задачах....
```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ terraform --version
Terraform v1.2.8
on darwin_amd64

Your version of Terraform is out of date! The latest version
is 1.3.1. You can update by downloading from https://www.terraform.io/downloads.html
Tsukanovs-Air:elasticsearch tsukanovdmitry$ 
```

Задача 3. Поддержка легаси кода.

В какой-то момент вы обновили терраформ до новой версии, например с 0.12 до 0.13. А код одного из проектов настолько устарел, что не может работать с версией 0.13. В связи с этим необходимо сделать так, чтобы вы могли одновременно использовать последнюю версию терраформа установленную при помощи штатного менеджера пакетов и устаревшую версию 0.12.

В виде результата этой задачи приложите вывод --version двух версий терраформа доступных на вашем компьютере или виртуальной машине.

```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ terraform --version
Terraform v1.2.8
on darwin_amd64

Your version of Terraform is out of date! The latest version
is 1.3.1. You can update by downloading from https://www.terraform.io/downloads.html
Tsukanovs-Air:elasticsearch tsukanovdmitry$ 
```

```bash
Tsukanovs-Air:elasticsearch tsukanovdmitry$ docker run -i -t hashicorp/terraform:0.12.0 version
Terraform v0.12.0

Your version of Terraform is out of date! The latest version
is 1.3.1. You can update by downloading from www.terraform.io/downloads.html
Tsukanovs-Air:elasticsearch tsukanovdmitry$ 
```
