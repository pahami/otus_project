# Проектная работа
-------------------------------------------------

## Тема: GLPI для ITSM: автоматическое развертывание с прокси, балансировкой и мониторингом.

### Схема проекта

![Схема сети](assets/images/otus_project.png)

Для работы стенда необходимо заранее подготовить серверы или воспользоваться шаблоном Vagrantfile. 

Таблица инфраструктуры:

| Сервер | IP адрес | CPU | RAM | Сервис |
|---|---|---|---|---|
| Otussrv0 | 192.168.103.210/24 | 1 | 1 | NGINX |
| Otussrv1 | 192.168.103.211/24 | 1 | 1 | GLPI, Apache2 |
| Otussrv2 | 192.168.103.212/24 | 1 | 1 | GLPI, Apache2 |
| Otussrv3 | 192.168.103.213/24 | 1 | 2 | Myssql - Master |
| Otussrv4 | 192.168.103.214/24 | 1 | 2 | Myssql - Slave |
| Otussrv5 | 192.168.103.215/24 | 1 | 2 | Zabbix |
| Otussrv6 | 192.168.103.216/24 | 4 | 6 | Elasticsearch |
| Otusstart| 192.168.103.3/24 | 1 | 2 | Ansible |

Доступ по ssh только из Ansible


### Формат сдачи: 

Развернем Vagrant-стенд:
  - Создайте папку с проектом и зайдите в нее (например: /otus_project):
```
mkdir -p otus_project ; cd ./otus_project
```
  - Клонируете проект с Github, набрав команду:
```
apt update -y && apt install git -y ; git clone https://github.com/pahami/otus_project.git
```
  - Запустите проект из папки, в которую склонировали проект (в нашем примере ./otus_project):

```
vagrant up
```

### Описание стенда

Стенд состоит из 2-х частей:
  - Создание и настройка сервиса с нуля
  - Восстановление уже настроенного сервиса

vars зашифровано.

Используемые параметры:

# restore_monitoring
zbx_pass - Zabbix пароль
zabbix_pkgs:
  - zabbix-server-mysql
  - zabbix-frontend-php
  - zabbix-nginx-conf
  - zabbix-sql-scripts
  - zabbix-agent
  - mysql-server
  - mysql-client
  - python3-mysqldb
  - python3-dev
  - libmysqlclient-dev
# restore_apache
glpi_db_host: "ip_mysql_master"
glpi_db_port: "port_mysql_master"
glpi_db_name: "name_db"
glpi_db_user: "user_db"

apache_port:
srv_name: - site.net
srv_root: /var/www/glpi
ondrej_sourcelist_path: "/etc/apt/sources.list.d/ppa_ondrej_php_{{ansible_distribution_release}}.list"
apache_pkgs:
  - php8.3
  - php8.3-cli
  - php8.3-xml
  - php8.3-mysqli
  - php8.3-gd
  - apache2
  - php8.3-intl
  - php8.3-curl
  - php8.3-bz2
  - php8.3-mbstring
glpi_web_owner: "www-data"
glpi_web_group: "www-data"
glpi_install_path: /var/www
# restore_nginx
ansible_ip:
nginx:
  http:
    port: 443
nginx_listen_port: 443
nginx_server_name: 
glpi1: #насройки хоста Apache1
  name:
  ip:
  port:
glpi2: #настройки хоста Apache2
  name:
  ip:
  port:


# restore_logs
elk_srv_ip: 192.168.103.216
elk_pass: esZz9GmFBFhhDjrw6CBc
elk_srv_name: Otussrv6
# restore_db
mysql_replication_user: repl
mysql_replication_password: OtusREPL-2025
glpi_pass: OtusGLPI-2025
mysql_root_password: OtusSQL-2025
mysql_master_host: "192.168.103.213"
mysql_master_port: "3306"
mysql_master_name: Otussrv3
mysql_slave_name: Otussrv4
Для восстановления системы необходимо выполни запуск playbook c тэго ресторе:

Nginx
'''

'''

Apache
'''

'''

Mysql_master
'''

'''

Mysql_slave
'''

'''

Monitoring
'''

'''

Logs
'''

'''



vagrant ssh srv
ll /var/log/rsyslog/client/
```

<details>
<summary> Результат </summary>

```
vagrant@srv:~$ ll /var/log/rsyslog/client/
total 52
drwxr-xr-x 2 syslog syslog  4096 Feb 23 19:32 ./
drwxr-xr-x 4 syslog syslog  4096 Feb 23 19:28 ../
-rw-r----- 1 syslog adm      355 Feb 23 19:32 dbus-daemon.log
-rw-r----- 1 syslog adm     3179 Feb 23 19:28 python3.log
-rw-r----- 1 syslog adm      546 Feb 23 19:28 rsyslogd.log
-rw-r----- 1 syslog adm     1276 Feb 23 19:58 sshd.log
-rw-r----- 1 syslog adm    12232 Feb 23 20:07 sshpass.log
-rw-r----- 1 syslog adm     4776 Feb 23 19:28 sudo.log
-rw-r----- 1 syslog adm      567 Feb 23 19:58 systemd-logind.log
-rw-r----- 1 syslog adm     3730 Feb 23 20:07 systemd.log

```
</details>
