## Задание 1

1. Взять официальный докер образ с **nginx** и выкачать его - команда `docker pull nginx`
* ![simple_docker](./screenshots/1.png)
2. Проверить наличие докер образа `docker images`
* ![simple_docker](./screenshots/2.png)
3. Запустить докер образ `docker run -d 021283c8eb95`
* ![simple_docker](./screenshots/3.png)
4. Проверить, что образ запустился `docker ps`
* ![simple_docker](./screenshots/4.png)
5. Посмотреть информацию о контейнере `docker inspect 021283c8eb95`
* ![simple_docker](./screenshots/5.png)
6. По выводу команды определить и поместить в отчёт:
    * размер контейнера
    
    ![simple_docker](./screenshots/6.png)
    * список замапленных портов
    
    ![simple_docker](./screenshots/7.png)
    * ip контейнера
    
    ![simple_docker](./screenshots/8.png)
7. Остановить докер образ `docker stop [container_id|container_name]`
* ![simple_docker](./screenshots/9.png)
8. Проверить, что образ остановился `docker ps`
* ![simple_docker](./screenshots/10.png)
9. Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run* (флаг -р - добавление порта)
* ![simple_docker](./screenshots/11.png)
10. Проверить, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx**
* ![simple_docker](./screenshots/12.png)
11. Перезапустить докер контейнер через `docker restart [container_id]`
* ![simple_docker](./screenshots/13.png)
12. Проверить любым способом, что контейнер запустился
* ![simple_docker](./screenshots/14.png)

## Задание 2

1. Прочитать конфигурационный файл *nginx.conf* внутри докер контейнера через команду *exec*
* ![simple_docker](./screenshots/15.png)
2. Создать на локальной машине файл *nginx.conf* (src)
3. Настроить в нем по пути */status* отдачу страницы статуса сервера **nginx**
* ![simple_docker](./screenshots/16.png)
4. Скопировать созданный файл *nginx.conf* внутрь докер образа через команду `docker cp`
* ![simple_docker](./screenshots/17.png)
5. Перезапустить **nginx** внутри докер образа через команду *exec*
* ![simple_docker](./screenshots/18.png)
6. Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**
* ![simple_docker](./screenshots/19.png)
7. Экспортировать контейнер в файл *container.tar* через команду *export*
* ![simple_docker](./screenshots/20.png)
8. Остановить контейнер
* ![simple_docker](./screenshots/21.png)
9. Удалить образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры
* ![simple_docker](./screenshots/22.png)
10. Удалить остановленный контейнер
* ![simple_docker](./screenshots/23.png)
11. Импортировать контейнер обратно через команду *import*
* ![simple_docker](./screenshots/24.png)
12. Запустить импортированный контейнер
* ![simple_docker](./screenshots/25.png)
13. Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**
* ![simple_docker](./screenshots/26.png)

## Задание 3

1. Написать мини сервер, который будет возвращать простейшую страничку с надписью `Hello World!`
* файл ./server/hello_server.c
2. Запустить написанный мини сервер через *spawn-fcgi* на порту 8080
* Перед этим серия команд для компиляции hello_server.c в контейнере import_nginx
* ![simple_docker](./screenshots/27.png)
* ![simple_docker](./screenshots/28.png)
* ![simple_docker](./screenshots/29.png)
- apt-get update
- apt-get install gcc
- apt-get install libfcgi-dev
- apt-get install spawn-fcgi
* ![simple_docker](./screenshots/30.png)
3. Написать свой *nginx.conf*, который будет проксировать все запросы с 81 порта на *127.0.0.1:8080*
* файл ./server/nginx.conf
- nginx -s reload
4. Проверить, что в браузере по *localhost:81* отдается написанная вами страничка
* ![simple_docker](./screenshots/31.png)
5. Положить файл *nginx.conf* по пути *./nginx/nginx.conf*

## Задание 4

1. Написать свой докер образ, который:
1) собирает исходники мини сервера на FastCgi из предыдущего задания
2) запускает его на 8080 порту
3) копирует внутрь образа написанный *./nginx/nginx.conf*
4) запускает nginx
- Dockerfile выглядит следующим образом (лежит в src)
* ![simple_docker](./screenshots/32.png)
2. Собрать написанный докер образ через `docker build` при этом указав имя и тег
* ![simple_docker](./screenshots/33.png)
3. Проверить через `docker images`, что все собралось корректно
* ![simple_docker](./screenshots/34.png)
4. Запустить собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки *./nginx* внутрь контейнера по адресу, где лежат конфигурационные файлы nginx
* ![simple_docker](./screenshots/35.png)
5. Проверить, что по localhost:80 доступна страничка написанного мини сервера
* ![simple_docker](./screenshots/36.png)
* ![simple_docker](./screenshots/37.png)
6. Дописать в *./nginx/nginx.conf* проксирование странички */status*, по которой надо отдавать статус сервера nginx
* ![simple_docker](./screenshots/38.png)
7. Перезапустить докер образ
* ![simple_docker](./screenshots/39.png)
8. Проверить, что теперь по *localhost:80/status* отдается страничка со статусом nginx
* ![simple_docker](./screenshots/40.png)
* ![simple_docker](./screenshots/41.png)

## Задание 5
0. Усстановливаем dockle: 'brew install goodwithtech/r/dockle'
1. Просканировать образ из предыдущего задания через `dockle [image_id|repository]`
* ![simple_docker](./screenshots/42.png)
2. Исправить образ так, чтобы при проверке через **dockle** не было ошибок и предупреждений
- иправили dockerfile, собрали образ заново, проверили, что докер запускает контейнер из этого образа и работает в соответствии с предыдущим заданием, затем проверили образ командой dockle -i CIS-DI-0010 mini-server:1
* ![simple_docker](./screenshots/43.png)


## Задание 6

 -  Написать файл *docker-compose.yml*, с помощью которого:
1. Поднять докер контейнер из части 5
2. Поднять докер контейнер с **nginx**, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера
* ![simple_docker](./screenshots/44.png)
- Замапить 8080 порт второго контейнера на 80 порт локальной машины
* ![simple_docker](./screenshots/45.png)
- Остановить все запущенные контейнеры
* ![simple_docker](./screenshots/46.png)
- Собрать и запустить проект с помощью команд `docker-compose build` и `docker-compose up`
* ![simple_docker](./screenshots/47.png)
- Проверить, что в браузере по *localhost:80* отдается написанная вами страничка, как и ранее
* ![simple_docker](./screenshots/48.png)


