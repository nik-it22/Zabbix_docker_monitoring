# Гайд по настройке Zabbix docker monitoring на Ubuntu
Для мониторинг docker на серверах Linix используеться zabbix agent2
## Скачиваем zabbix-release
+ wget https://repo.zabbix.com/zabbix/*.deb
+ устанавливаем 
+ dpkg -i zabbix-release.*/deb

## Дальше 
+ apt update
+ apt install zabbix-agent2

# Переходим к файлу 
## sudo nano /etc/zabbix/zabbix-agent2.conf
+ Server=***** // ip zabbix-server
+ ServerActive=****** // ip zabbix-server
+ Hostname=******* // присваиваем имя

## Перезапускаем agent и добавляем в автозагрузку
+ systemctl restart zabbix-agent2
+ systemctl enable zabbix-agent2

## Установим ACL для управлением доступа к файлам
+ sudo apt-get install acl
## Нужно добавить пользователя zabbix, от имени которого работает агент, в группу docker, чтобы у него был доступ к docker.sock.
+ usermod -aG docker zabbix
## После этого надо перезагружаем сервер. Можно напрямую выдать права избежав перезагрузки.
+ setfacl --modify user:zabbix:rw /var/run/docker.sock

## Добавление шаблона для мониторинга для zabbix server
+ https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/app/docker
+ Пеереходим по данный ссылке скачиваем yml файл
+ Переходим в web server zabbix -> Сбор данных -> Шаблоны -> Импорт

## При создании нового узла добавляем наш новый шаблон "Docker by Zabbix agent 2"

## Дополнительно
+ Не забывайте сменить настройку port agenta в zabbix-agent2.conf, два агента не могут работают через один порт.
