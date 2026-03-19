# Zabbix 1

## Установка и настройка Zabbix Server

### Использованные команды

```bash
# Установка репозитория Zabbix
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian12_all.deb
dpkg -i zabbix-release_latest_6.0+debian12_all.deb
apt update

# Установка Zabbix Server и WEB интерфейс
apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts

# Создание базы данных
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

# Импорт начальной схемы базы данных 
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# настройка базы данных для Zabbix сервера (редактируем /etc/zabbix/zabbix_server.conf)
DBPassword=password

# Запускаем процессы Zabbix сервера и добавляем их в автозапуск
systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2

# На хостах host1 и host2 устанавливаем репозиторий Zabbix
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian12_all.deb
dpkg -i zabbix-release_latest_6.0+debian12_all.deb
apt update

# Устанавливаем Zabbix агент
apt install zabbix-agent

# Запускаем процесс Zabbix агента и добавляем его в автозагрузку
systemctl restart zabbix-agent
systemctl enable zabbix-agent

# На хостах host1 и host2 редактируем /etc/zabbix/zabbix_agentd.conf устанавливая настройки сервера и детализацию логов
nano /etc/zabbix/zabbix_agentd.conf
Server=192.168.0.127
ServerActive=192.168.0.127
DebugLevel=5

# Проверка работоспособности сервера в WEB интерфейсе
![Screenshot1](https://github.com/ZoooMBoooM/netology/blob/209bf35fc099cd459d433ac4af887b32a280e7bd/monitoring/zabbix1/screenshots/screenshot1.jpg))

# Проверка подключения хостов host1 и host2 в WEB интерфейсе
![Screenshot2](https://raw.githubusercontent.com/ZoooMBoooM/netology/main/monitoring/zabbix1/screenshots/screenshot2.jpg)

# Проверка работоспособности host1 в WEB интерфейсе
![Screenshot3](https://raw.githubusercontent.com/ZoooMBoooM/netology/main/monitoring/zabbix1/screenshots/screenshot3.jpg)

# Проверка работоспособности host2 в WEB интерфейсе
![Screenshot4](https://raw.githubusercontent.com/ZoooMBoooM/netology/main/monitoring/zabbix1/screenshots/screenshot4.jpg)

# Вывод логов в консоль host1
![Screenshot5](./monitoring/zabbix1/screenshots/screenshot5.jpg)

# Вывод логов в консоль host2
![Screenshot6](monitoring/zabbix1/screenshots/screenshot6.jpg)
