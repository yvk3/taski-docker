# taski-docker
# Яндекс-практикум.
Размещение проекта на удалённом сервере с использованием технологии Docker.
Проект Taski позволяет составлять список задач и фиксировать их выполнение.

# Технологии.

![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white) ![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray) ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)

- [Python 3.9](https://www.python.org/downloads/)
- [Django 3.2.3](https://www.djangoproject.com/download/)
- [Django REST framework 3.12.4](https://pypi.org/project/djangorestframework/#files)
- [Docker](https://docs.docker.com/)
- [Nginx](https://nginx.org/en/docs/)
- [Gunicorn](https://pypi.org/project/gunicorn/20.1.0/)
- [PostgreSQL](https://www.postgresql.org/docs/)

## Установка проекта на локальный компьютер из репозитория

- Клонировать репозиторий git clone <адрес вашего репозитория>
перейти в директорию с клонированным репозиторием.
- установить виртуальное окружение ```python3 -m venv venv``` 
- активировать виртуальное окружение:
  * если у вас Linux/macOS 
       ``` 
      source env/bin/activate 
      ``` 
  * если у вас windows 
     ``` 
    source env/scripts/activate 
    ``` 
- установить зависимости ```pip install -r requirements.txt```
- выполнить миграции ```python3 manage.py migrate```
- запустить проект ```python3 manage.py runserver```
- в директории /taski-docker/ создать файл .env . Примерное содержание:
```
POSTGRES_USER=django_user
POSTGRES_PASSWORD=mysecretpassword
POSTGRES_DB=django
DB_HOST=db
DB_PORT=5432
```   

## Запуск контейнеров локально.

В терминале, в папке с файлом ```docker-compose.yml``` нужно выполнить
команду:
```python
docker compose up
```
- Выполняем миграцию: ```docker compose exec backend python manage.py migrate```
- Остановить работу Docker Compose можно командой ```Ctrl+C```

## Деплой на удалённый сервер.

В репозитории проекта в разделе GitHub Actions Settings/Secrets/Actions прописать Secrets - переменные 
окружения для доступа к сервисам:
```
DOCKER_USERNAME                # имя пользователя в DockerHub
DOCKER_PASSWORD                # пароль пользователя в DockerHub
HOST                           # ip_address сервера
USER                           # имя пользователя
SSH_KEY                        # приватный ssh-ключ
PASSPHRASE                     # кодовая фраза (пароль) для ssh-ключа

TELEGRAM_TO                    # id телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN                 # токен бота (получить токен можно у @BotFather, /token, имя бота)
```
Для деплоя необходимо внести изменения в некоторые файлы:

- taski-docker/.github/workflows/main.yml - в tags необходимо заменить 
имя пользователя на ваше имя в Docker Hub tags:``` <ваш логин Docker Hub>/kittygram_backend:latest``` 
для всех tags
- settings.py внести данные вашего сервера в ALLOWED_HOSTS
- в корне проекта внести изменения в файл ```docker-compose.production.yml```: 
заменить имя пользователя на ваше имя в Docker Hub image: ``` tags: ваш-логин-на-docker-hub/taski_backend:latest ```
для всех image

При push в ветку main:
```
- git add . - добавляем изменения из рабочего каталога.
- git commit -m 'текст коментария' - фиксирует изменения перед отправкой.
- git push - отправляем локальную ветку на удаленный сервер.
```
Будет автоматически отработаны сценарии по тестированию кода, 
сборке Docker образов и загрузке их на Docker Hub, деплой проекта из Docker Hub на сервер со сборкой образов, 
выполнением миграций и сборкой статики. В случае успешного деплоя будет отправлено сообщение в Telegram.

## Автор
Кузнецов Юрий [GitHub](https://github.com/yvk3)