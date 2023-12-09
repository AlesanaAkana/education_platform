# Образовательная платформа Educa

##Технологии:
### Python 3.10, Django 4.1, DRF 3.13,  Redis, Channels, Docker, Nginx, PostgreSQL, uWSGI, Daphne

## Запуск проекта

Запускаем контейнер docker-compose:

```
docker compose up
```

Применяем миграции:

```
docker compose exec web python /code/educa/manage.py migrate
```

Создаём пользователя-администратора:

```
docker compose exec web python /code/educa/manage.py createsuperuser
```

Собираем статические файлы:

```
docker compose exec web python /code/educa/manage.py collectstatic
```

### В целях ознакомления был сгенерирован сертификат SSL для HTTPS соединения. Он находится в директории `educa/ssl` вместе с ключом.

### Вы также можете сгенерировать свой ключ, для этого нужно поставить приложение OpenSSL:

#### Для Linux:

```
sudo apt install openssl
```


#### Установка для Windows:

1. Посетите официальный сайт [OpenSSL](https://www.openssl.org/)) и перейдите на страницу `Win32/Win64 OpenSSL Installation Project`. Оттуда вы можете скачать установщик для Windows.
2. Запустите скачанный установщик и следуйте инструкциям на экране. Обычно у вас будет возможность выбрать директорию установки и другие параметры.
3. Добавьте директорию с исполняемыми файлами OpenSSL в переменную среды `PATH`. Обычно эти файлы находятся в поддиректории `bin`. Вы можете добавить этот путь в переменную среды PATH следующим образом:
- Правой кнопкой мыши на "Этот компьютер" (или "Мой компьютер") -> Свойства.
- Нажмите "Дополнительные параметры системы".
- Перейдите на вкладку "Дополнительно" и нажмите "Переменные среды".
- Выберите переменную среды "Path" в разделе "Системные переменные" и нажмите "Изменить".
- Нажмите "Новый" и добавьте путь к директории bin OpenSSL.

##### Проверка версии:

```
openssl version
```

Обратите внимание, что процедура установки может немного различаться в зависимости от версии OpenSSL и используемого установщика.
### Генерация сертификата:

```
openssl req -x509 -newkey rsa:2048 -sha256 -days 3650 -nodes \
-keyout ssl/educa.key -out ssl/educa.crt \
-subj '/CN=*.educaproject.com' \
-addext 'subjectAltName=DNS:*.educaproject.com'
```


## Запуск без Docker Compose

Запускаем Redis:

```
docker run -it --rm --name redis -p 6379:6379 redis
```

Запуск сервера для локальной разработки:

```
python manage.py runserver --settings=educa.settings.local
```


##### Если возникают ошибки в работе контейнера, проблема может быть в правах. Вот одно из решений:

```
sudo chmod -R 777 ./
```
