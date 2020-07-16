# Требования для установки системы CallForce
## Общие требования ко всем серверам клиента
1. CallForce желательно устанавливать на отдельный выделенный сервер (или виртуалку)
2. В случае доступа через VPN все сервера должны иметь свои VPN-адреса
3. Сервер для CallForce должен “видеть” все сервера Астериска (через локальную сеть или интернет)
4. Сервера должны быть доступны по ssh через ssh-ключ
5. Если пользователь для ssh-доступа отличается от root, то у пользователя должны быть права для установки ПО (через sudo, например)
6. На серверах должны быть прописаны ssh-ключи **CallForce-разработчиков** (описаны ниже) в файле ~/.ssh/authorized_keys (или ~/.ssh/authorized_keys2). На серверах Астериска ssh-ключи **CallForce-разработчиков** можно удалить после установки и запуска CallForce

## На машине на которой должна быть установлена система CallForce

1. ОС: CentOS 7+
2. Свободное место: минимум 60GiB
3. RAM: минимум 4GiB
4. CPU: минимум 2 ядра, 2 GHz
5. С сервера **обязателен доступ** к серверу **main.callforce.pro** (публичный/интернет ip  **87.245.157.133** или VPN Voxlink ip **10.19.10.1**) по TCP порту **7788** — для механизма обновления и мониторинга системы. Никакие файрволы не должны блокировать соединение на этот ip и этот порт.
6. Для автодеплоя, сервер должен быть доступен по ssh с сервера **main.callforce.pro** (публичный/интернет ip - **87.245.157.133** или по VPN Voxlink ip **10.19.10.1** - при подключении через VPN-сеть Voxlink).
7. На сервере должен быть открыт доступ на сайты по портам 80 и 443:
	* Репозитории yum: mirrorlist.centos.org, fedoraproject.org, dl.fedoraproject.org и famillecollet.com - для установки [Redis DB](https://redis.io/)
	* nodesource.com - для установки [nodejs](https://nodejs.org/en/)
	* mongodb.org - для установки [MongoDB](https://www.mongodb.com/)
	* github.com и api.github.com - для установки [Blank](https://github.com/getblank)
	* npmjs.org - для установки основных компонентов и обновления системы
8. ==Для доступа к записям разговоров==, на сервере должна быть **подмонтирована** папка /var/spool/asterisk/monitor/ с Астериск-сервера ==по тому же пути== на сервере CallForce. В случае, ==если монтирование папки не возможно== (например, при кластере, когда одновременно работают несколько Астерисков и распределяют нагрузку между собой), **Указать об этом** - в этом случае, на сервера Астериска нужна будет установка "**сервиса по передаче записей разговоров**", который будет передавать их на сервер CallForce
9. Следующие порты должны быть открыты порты для доступа из вне (или только из локальной сети предприятия), для того, чтобы можно быть войти в **Web**-интерфейс:
	* 3000 для http и 1443 для https - Для доступа к Мониторингу и для Агентов
	* 80 для http и 443 для https (не обязательно, резервные, в случае, если они не заняты другими сервисами)
10. Следующие порты Должны быть доступны из интернета (не обязательно)
	* 9230-9240 для отладчика nodejs
## На каждой машине, на которой установлен Астериск
1. В файле /etc/asterisk/manager.conf в секцию [general] обязательно нужно добавить следующую инструкцию:
	* ```channelvars=CDR(linkedid)```
2. В файле /etc/asterisk/manager.conf нужно добавить AMI-пользователя **callforce** (пароль любой)
3. Следующие порты должны быть доступны на машине, на которой установлена наша система (если наша система стоит не на машине с Астериском)
	* 5038 — стандартный порт Астериск
	* 8889 — сервиса доступа к записям разговоров
	* 44444 — для передачи AMI-событий в защищённом протоколе по **https**, с кешированием и буферизацией
4. Если на сервере необходима установка "**сервиса по передаче записей разговоров**”, то ssh-пользователь должен иметь права на установку ПО и прописывание авто-запуска сервисов

## ssh-ключи CallForce-разработчиков

	* ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDdRuTPeCQm6nNKT+qGWIdB199pGAaZjDN2z3wlLDoN2yk/fWLQv6hr6LAhPF+ZM0I0BQ7CX2grt0TKlWm6g72s7T7vCb1c4Y2azW/SokxCAd4drzTPTWR1Ded8Ivv2FrfHi+DgFIvPTLbjzLgtJVK8PkhQjaDXwUPXctzc52wTnMuWxUroX3QB6bYlHytsBbCjkWpZ/C56wJ/etfLoLVOsLWEfKw9BMludiCK3xyi91Uz3hrsJA+W+52DC0LGv771HwPgiRbH18PsAuJBcZXZZ0exfIjgUEB8/y3OM1Jo63Qn0wuNPVGSPNdQuLfm2K9LIn3qSkuKldkQGcOS9jfSh termi@termi-ws
	* ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDQyOFZaKbgMyW5N07lbhsguEZSZH7kIQJDcdAmMQCF1pyudhzenQFeoFRHmvzpgRIfKm/fCOVpItyj7Wd/wT138VwngQn+dnQj9GXCvwwS3B6atD3YkmKjeX740WyvmKghKooVW2SnDJyXHVH1NqURWtzXvbmaG8fkQ/SDpjoPlqr6KwDHB3setfFI/JWJh9hcdN9jEo1YvSLDx/vEs5TyYuNbX+PRLBlXEF+R4V0vESVDnwf4MdJrEhDnNz9ERDUr1cAw9HohTRQpvJcpF+axn54wyUgBfRBul4Y/fR4sLYhWpY+2MpFyORUf0xdxHOwyXTsvNBw5XrOQDkaXfIYP termi@termi-laptop
	* ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAzX21Mwj6KHdnSRT1TaK+ENEQ5V+zO8k+aupaU+mEyih7fEEICmLqOzHnNYqLxaJ5QREtltqFJQjXePmT9vldMn2OFVIQNpDkc6lVUNY0H8O0akXczLHW7d98a7EZ9OQukCd/qZ6CXLX5CzhxIgW8CAXQqknKWjPe3iuPg3cqpET1dA+HCML/Y5WJ3/uTMqZkb3TCW5+4xGMFCtLwizJCbvm6sbKsdKs7gQaG6jH37GcXIv9UgjaENHBeytEkicuOAGRRVARFgJQIKxshflF9WF18BYlRTYni6hIDLejY3XyDER+ZnnTTNoe6S5cMy0vvmBbmoCoqWSSP6j2APG7zPQ== root@CallForceServer

## В каком виде предоставлять информацию для установки системы:

1. ip-адрес сервера для установки CallForce (внешний или VPN) для подключения по ssh. Указать ssh-порт, в случае если он отличается от 22
2. локальный (в локальной сети клиента) ip-адрес сервера для установки CallForce
3. ip-адреса Астерисков (внешние или VPN) для подключения по ssh. Указать ssh-порт, в случае если он отличается от 22
4. локальные (в локальной сети клиента) ip-адреса Астерисков клиента, которые “видны” с сервера для установки CallForce, для AMI-соединения. Указать AMI-порт, если он отличается от 5038
5. Asterisk AMI-пользователь и его пароль
6. Если у клиента кластер - Указать какой тип кластера
7. Указать необходима ли установка “сервиса по передаче записей разговоров” с серверов Астериска на сервер CallForce
8. Данные ответственного сотрудника со стороны заказчика: ФИО, должность, email и телефон

##Решение проблемы при установки CallForce

1. Прокси для curl/wget (для установки nodejs через прокси):
	* ```export http_proxy=http://proxy.company.com:8080/```
	* ```export https_proxy=https://proxy.company.com:8080/```
2. Прокси для npm:
	* ```npm config set proxy http://proxy.company.com:8080```
	* ```npm config set https-proxy http://proxy.company.com:8080```
3. Для перезапуска asterisk/manager после добавления нового пользователя AMI используйте команду:
	* ```asterisk -x 'manager reload'```
4. Если есть проблемы с подключением по AMI (вплоть до ошибки “Incorrect login or password”), возможно ip-адрес, с которого подключаемся не разрешен для доступа. Нужно в файле /etc/asterisk/manager.conf в секции пользователя прописать:
	* ```permit=<inet>/<netmask>```
	* Значения брать из ifconfig на сервере, с которого коннектимся по AMI. Значения ip-адресов должны быть локальными (не VPN)
5. Для проверки, что с сервера на котором установлен CallForce проверить, что соединение с AMI есть:
	* в консоле:
	* telnet <внутренний ip Астериск сервера> 5038
	* в установленном telnet-соединении:
	* Action: Login
	* ActionId: 1
	* Username: callforce
	* Secret: /<secret/>
	* /<два раза Enter/>
6. В случае, если интернет доступен через прокси, бинарные пакеты Blank’а могут не скачаться в /opt/nodeside/cffrontend/node_modules/blank-cli/bin/ с адреса https://api.github.com/repos/getblank/<package-name>/releases/latest - их придется закинуть вручную - package-name=[blank-fs, blank-queue, blank-cron, blank-router, blank-sr]
7. Если файлы с записью разговоров не скачиваются с ошибкой “bad file extension”, нужно проверить файл /etc/mime.types - и если он пустой или отсутствует, заполнить его стандартным содержимым (найти в интернете)
8. **79.137.209.166** - старый адрес центрального сервера. Должен быть заменен на новый **87.245.157.133**, или на Voxlink VPN-адрес **10.19.10.1**