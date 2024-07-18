
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»


## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .


## Решение 1

![Repo](https://github.com/user-attachments/assets/3dd07601-9d44-46b2-b73d-0ead9e395da0)
---

![docker build and push](https://github.com/user-attachments/assets/f699826e-b9b4-483c-aa9c-2087091a4227)
---

![Changes](https://github.com/user-attachments/assets/9e2dea0c-d47e-4442-a5fd-a25d07cae985)
---

![Result](https://github.com/user-attachments/assets/a2cef140-9fc5-482d-bfcf-f063c31b0a85)
---

https://hub.docker.com/repository/docker/sash39/custom-nginx/general
---



## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Решение 2

![переименовать и запустить другой контейнер](https://github.com/user-attachments/assets/f5670a7d-7874-483c-9dfe-6ece35adf03d)
---

![Curl after rename](https://github.com/user-attachments/assets/515310d0-abdf-4991-b7b9-60d4f33688d1)
---

![Commands](https://github.com/user-attachments/assets/884e0e69-582d-4276-875a-95f87434f992)
---


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Решение 3

1. docker attach custom-nginx-t2
![docker attach](https://github.com/user-attachments/assets/e160a530-bc62-40d1-9f7f-9db7a03cfc69)

3. Контейнер остановился потому, что при нажатии Ctrl-C основной процесс nginx был завершен, а так как это основной процесс контейнера, контейнер также завершил свою работу.
4. docker start custom-nginx-t2
![image](https://github.com/user-attachments/assets/aae1fc9d-21f1-43c4-8c2c-1d1defff0dab)

5. docker exec -it custom-nginx-t2 /bin/bash
![interactive mode](https://github.com/user-attachments/assets/ac758dd4-6fac-4f48-9956-9b949ba915dd)

7. nano /etc/nginx/conf.d/default.conf
![listen 81](https://github.com/user-attachments/assets/605c3e76-41dc-4f46-8f3b-e82d2651b5c6)

8. 
nginx -s reload
curl http://127.0.0.1:80
curl http://127.0.0.1:81

![nginx reload](https://github.com/user-attachments/assets/37ed4d1a-bb52-480f-aa6c-292bee53a0e9)

10. Команда ss -tlpn не покажет процесс, слушающий порт 127.0.0.1:8080, так как контейнер больше не прослушивает порт 80. Команда docker port custom-nginx-t2 покажет, что контейнер опубликован на порту 8080, но так как внутри контейнера nginx прослушивает порт 81, внешний запрос на порт 8080 не будет работать. Команда curl http://127.0.0.1:8080 вернет ошибку.

![problems](https://github.com/user-attachments/assets/0795b39c-cf3a-49b8-8e08-11827e869589)

11.
![исправление конфигурации контейнера](https://github.com/user-attachments/assets/82bd75a1-b1fd-45aa-8ec6-c69d5a4edcbe)

12. docker rm -f custom-nginx-t2
![delete without stop](https://github.com/user-attachments/assets/921603b4-d5fb-40cd-bc44-4f87f3f3d358)



## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Решение 4

![install CentOS](https://github.com/user-attachments/assets/a701a6ad-676a-4861-9635-45b138d8fa09)
---

![install debian](https://github.com/user-attachments/assets/1f0a63b1-3a2f-4849-afab-e8e0f9fbd033)
---

![content containers](https://github.com/user-attachments/assets/d4189768-9abb-4d22-8d5e-d00608d60cf5)
---

![history centOS container](https://github.com/user-attachments/assets/50237618-71d9-4ef7-9acf-3ac8332c0363)
---

![history debian container](https://github.com/user-attachments/assets/24619aa6-597d-434d-845e-f353783a081d)
---

![docker ps](https://github.com/user-attachments/assets/adcf2576-1ebf-418d-9d4d-35a0a66c232c)
---



## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

## Решение 5

1. Если запущен контейнер portainer, это означает, что был использован файл compose.yaml. Если запущен контейнер registry, это означает, что был использован файл docker-compose.yaml. На практике, поведение Docker Compose может зависеть от версии и конфигурации. 

![docker compose up](https://github.com/user-attachments/assets/132d6c98-d509-4e1a-84c2-9bb044ca3c79)
---

![change compose](https://github.com/user-attachments/assets/bdfd1a2d-0fd4-41d4-8648-9253ef38488a)
---

![result after changed compose and include docker compose](https://github.com/user-attachments/assets/4e75fb55-2380-4ea6-9014-2ac3cc10cb6d)
---


### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

