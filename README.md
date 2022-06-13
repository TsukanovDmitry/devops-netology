Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"
1. ![image](https://user-images.githubusercontent.com/75790619/173409178-132f6f6b-1434-4ec4-ba5c-4167c9602de3.png)
2. Done
3. 
Установка
sudo apt install apache2

Генерация ключа
openssl req -x509 -nodes -newkey rsa:2048 -keyout /etc/ssl/private/ssl-key.key -out /etc/ssl/certs/ssl-cert.cer -subj "/C=RU/ST=Moscow/L=NV/O=Netolgy/OU=devops/CN=localhost

Включение ssl.
```bash
a2enmod ssl
systemctl restart apache2
a2ensite default-ssl
sudo systemctl reload apache2
```

Проверка:
![image](https://user-images.githubusercontent.com/75790619/173409667-bf859b79-202c-4849-8c25-92bc552d1fa2.png)

4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
Я так понял, что самый популярный метод это testssl.sh
Проверяем озон
![image](https://user-images.githubusercontent.com/75790619/173409928-a9a9be9f-848f-4e7f-82b5-6ca43c8160d3.png)

5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
devops@devops-netology:/etc/ssh$ sudo apt install openssh-server

Добавить строку в ssh_config
![image](https://user-images.githubusercontent.com/75790619/173410774-3f11fdff-41d3-4c4e-8fda-0b86f4a3e893.png)

Генерируем ключи
PS C:\Users\TsukanovDA-VTB> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\TsukanovDA-VTB/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\TsukanovDA-VTB/.ssh/id_rsa.
Your public key has been saved in C:\Users\TsukanovDA-VTB/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:sC6RyeKjtHCcH/Zv/5A69rd6slO7ZP1YlyD9It4OPzg tsukanovda-vtb@LAPTOP-J145VCT5
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|                 |
|      .          |
|   . o o    .    |
|  . = . S  . o   |
| o o o     .o.o .|
|..* + .   o++o.oo|
|oo.+ +  +.+EO..+.|
|..  . .+o+=X**o .|
+----[SHA256]-----+
PS C:\Users\TsukanovDA-VTB>

Копируем ключ
PS C:\Users\TsukanovDA-VTB> scp.exe -P 22 C:\Users\TsukanovDA-VTB/.ssh/id_rsa.pub devops@192.168.106.150:~/.ssh/authorized_keys
devops@192.168.106.150's password:
id_rsa.pub



