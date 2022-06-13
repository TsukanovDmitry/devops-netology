Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"
1. ![image](https://user-images.githubusercontent.com/75790619/173409178-132f6f6b-1434-4ec4-ba5c-4167c9602de3.png)
2. Done
3. 
Установка
sudo apt install apache2

Генерация ключа
openssl req -x509 -nodes -newkey rsa:2048 -keyout /etc/ssl/private/ssl-key.key -out /etc/ssl/certs/ssl-cert.cer -subj "/C=RU/ST=Moscow/L=NV/O=Netolgy/OU=devops/CN=localhost

Включение ssl.
a2enmod ssl
systemctl restart apache2
a2ensite default-ssl
sudo systemctl reload apache2

Проверка:
![image](https://user-images.githubusercontent.com/75790619/173409667-bf859b79-202c-4849-8c25-92bc552d1fa2.png)

4. Я так понял, что самый популярный метод это testssl.sh
Проверяем озон
![image](https://user-images.githubusercontent.com/75790619/173409928-a9a9be9f-848f-4e7f-82b5-6ca43c8160d3.png)

