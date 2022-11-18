![example workflow](https://github.com/Svyatoslav1992/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Проверить доступность сервиса: http://${{ secrets.HOST }}/admin/

### Описание проекта: 

Проект YaMDb собирает отзывы пользователей на различные произведения: Книги, Музыка, Фильмы
Администратор может добавить новые категории

Формат выдаваемых данных: JSON

Модели проекта: Категории, Произведения, Отзывы

Поддерживаемые методы: GET, POST, PUT, PATCH, DELETE

## Описание

Проект **YaMDb** собирает отзывы (Review) пользователей на произведения (Titles).
Произведения делятся на категории:

- "Книги"
- "Фильмы"
- "Музыка"
  Список категорий (Category) может быть расширен администратором (например, можно добавить категорию "Ювелирка").
  Сами произведения в **YaMDb** не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.
Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число).
На одно произведение пользователь может оставить только один отзыв.

Процедура запуска проекта представлена ниже.

## Расширение функциональности

Функционал проекта адаптирован для использования PostgreSQL и развертывания в контейнерах Docker. Используются инструменты CI и CD.

---

## Стек

 - Python 3.7
 - Django 2.2.16
 - REST Framework 3.12.4
 - PyJWT 2.1.0
 - Django filter 21.1
 - Gunicorn 20.0.4
 - PostgreSQL 12.2
 - Docker 20.10.2

## Установка
### Инструкции для развертывания и запуска приложения
для Linux-систем все команды необходимо выполнять от имени администратора
- Склонировать репозиторий
```bash
git clone https://github.com/EvgeniyPrivalov1986/yamdb_final.git
```
- Выполнить вход на удаленный сервер
- Установить docker на сервер:
```bash
apt install docker.io 
```
- Установить docker-compose на сервер:
```bash
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
- Локально отредактировать файл infra/nginx/default.conf, обязательно в строке server_name вписать IP-адрес сервера
- Скопировать файлы docker-compose.yml и default.conf из директории infra на сервер:
```bash
scp docker-compose.yml <username>@<host>:/home/<username>/docker-compose.yml
scp nginx.conf <username>@<host>:/home/<username>/nginx/default.conf
```
- Создать .env файл по предлагаемому выше шаблону. Обязательно изменить значения POSTGRES_USER и POSTGRES_PASSWORD
- Для работы с Workflow добавить в Secrets GitHub переменные окружения для работы:
    ```
    DB_ENGINE=<django.db.backends.postgresql>
    DB_NAME=<имя базы данных postgres>
    DB_USER=<пользователь бд>
    DB_PASSWORD=<пароль>
    DB_HOST=<db>
    DB_PORT=<5432>
    
    DOCKER_PASSWORD=<пароль от DockerHub>
    DOCKER_USERNAME=<имя пользователя>

    USER=<username для подключения к серверу>
    HOST=<IP сервера>
    PASSPHRASE=<пароль для сервера, если он установлен>
    SSH_KEY=<ваш SSH ключ (для получения команда: cat ~/.ssh/id_rsa)>

    TELEGRAM_TO=<ID чата, в который придет сообщение>
    TELEGRAM_TOKEN=<токен вашего бота>
    ```
    Workflow состоит из четырёх шагов:
     - Проверка кода на соответствие PEP8
     - Сборка и публикация образа бекенда на DockerHub.
     - Автоматический деплой на удаленный сервер.
     - Отправка уведомления в телеграм-чат.
- собрать и запустить контейнеры на сервере:
```bash
docker-compose up -d --build
```
- После успешной сборки выполнить следующие действия (только при первом деплое):
    * провести миграции внутри контейнеров:
    ```bash
    docker-compose exec web python manage.py migrate
    ```
    * собрать статику проекта:
    ```bash
    docker-compose exec web python manage.py collectstatic --no-input
    ```  
    * Создать суперпользователя Django, после запроса от терминала ввести логин и пароль для суперпользователя:
    ```bash
    docker-compose exec web python manage.py createsuperuser
    ```
### Шаблон описания файла .env
 - DB_ENGINE=django.db.backends.postgresql
 - DB_NAME=postgres
 - POSTGRES_USER=postgres
 - POSTGRES_PASSWORD=postgres
 - DB_HOST=db
 - DB_PORT=5432
 - SECRET_KEY=<секретный ключ проекта django>