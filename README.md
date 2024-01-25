# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Казанцева Елена`

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Требования к результаты 
1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

Текст моих команд:

```bash
# apt update
# apt install postgresql
# wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
# dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
# apt update
# apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts
# systemctl status zabbix-server.service
# -u postgres createuser --pwprompt zabbix
# -u postgres createdb -O zabbix zabbix
# zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
# sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf
# systemctl restart zabbix-server apache2
# systemctl enable zabbix-server apache2
# systemctl status zabbix-server.sevice
# ip a
```

1. `Zabbix на хосте работает`

![Zabbix на хосте работает](https://github.com/ElenaKazantseva/homeworks/blob/hw-zabbix-1/img/Screenshot_12.jpg)
   
2. `Авторизация в веб-интерфейсе`
   
![Скриншот авторизации в веб-интерфейсе 1](https://github.com/ElenaKazantseva/homeworks/blob/hw-zabbix-1/img/Screenshot_14.jpg)
![Скриншот авторизации в веб-интерфейсе 2](https://github.com/ElenaKazantseva/homeworks/blob/hw-zabbix-1/img/Screenshot_16.jpg)

---

### Задание 2 

Установите Zabbix Agent на два хоста.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

#### Требования к результаты 
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

Текст моих команд:

```bash
# ssh cubic@158.160.34.218
# apt update
# wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
# dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
# apt update
# apt install zabbix-agent
# systemctl status zabbix-agent.service
# tail -f /var/log/zabbix/zabbix_agentd.log
# sed -i 's/Server=127.0.0.1/Server=95.215.86.93/g' /etc/zabbix/zabbix_agentd.conf
# cat /etc/zabbix/zabbix_agentd.conf | grep Server=
# systemctl restart zabbix-agent.service
# systemctl status zabbix-agent.service
```

1. `скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу`

![Zabbix на хосте работает]()
   
2. `скриншот лога zabbix agent, где видно, что он работает с сервером:
   на ВМ в Яндексе (только агент)
   на ВМ на компьютере (заббикс-сервер)`
   
![ВМ в Яндексе (только агент)]()

![ВМ на компьютере (заббикс-сервер)]()

3. `скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные`

![ВМ в Яндексе (только агент)]()

![ВМ на компьютере (заббикс-сервер)]()

---

### Задание 3

**Что нужно сделать:**

1. Создайте новую ветку dev и переключитесь на неё.
2. Создайте в ветке dev файл test.sh с произвольным содержимым.
3. Сделайте несколько коммитов и пушей  в ветку dev, имитируя активную работу над  файлом в процессе разработки.
4. Переключитесь на основную ветку.
5. Добавьте файл main.sh в основной ветке с произвольным содержимым, сделайте комит и пуш . Так имитируется продолжение общекомандной разработки в основной ветке во время разработки отдельного функционала в dev  ветке.
6. Сделайте мердж dev  ветки в основную с помощью git merge dev. Напишите осмысленное сообщение в появившееся окно комита.
7. Сделайте пуш в основной ветке.
8. Не удаляйте ветку dev.

В качестве ответа прикрепите ссылку на граф коммитов https://github.com/ваш-логин/ваш-репозиторий/network в ваш md-файл с решением.

Ваш граф комитов должен выглядеть аналогично скриншоту:   

![скрин для Git](https://github.com/netology-code/sdvps-homeworks/assets/77622076/e73589cf-7e97-40e5-ac01-d1d55376f1b9)

[ссылка на мое решение, graph](https://github.com/ElenaKazantseva/hw-git-1/network)
