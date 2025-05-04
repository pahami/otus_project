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

Используются репозитории mirror.yandex
vars зашифровано.

<details>
<summary> Используемые параметры: </summary>
```
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
elk_srv_ip: #ip сервера elasticsearch
elk_pass: #пароль от пользователя elastic
elk_srv_name: # имя сервера
# restore_db
mysql_replication_user: #имя пользователя репликации
mysql_replication_password: #пароль репликации
glpi_pass: #пароль от польхователя glpi
mysql_root_password: #пароль от sql root
mysql_master_host: "ip host"
mysql_master_port: "port host"
mysql_master_name: # имя сервера mysql_master
mysql_slave_name: # имя mysql_slave
```
</details>

Для восстановления системы необходимо выполни запуск playbook c тэго ресторе:

Nginx
```
ansible-playbook site.yml --vault-password-file=ans_vault -K -l nginx --tags restore
```

Apache
```
ansible-playbook site.yml --vault-password-file=ans_vault -K -l apache --tags restore
```

Mysql_master
```
ansible-playbook site.yml --vault-password-file=ans_vault -K -l db --tags restore
```

Mysql_slave
```
ansible-playbook site.yml --vault-password-file=ans_vault -K -l db --tags restore_slave
```

Monitoring
```
ansible-playbook site.yml --vault-password-file=ans_vault -K -l monitoring --tags restore_zabbix
```

Logs
```
ansible-playbook site.yml --vault-password-file=ans_vault -K -l logs --tags start,restore_logs
```
