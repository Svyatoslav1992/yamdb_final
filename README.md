![example workflow](https://github.com/github/docs/actions/workflows/yamdb_workflow.yml/badge.svg)

### Описание проекта: 

Проект YaMDb собирает отзывы пользователей на различные произведения: Книги, Музыка, Фильмы
Администратор может добавить новые категории

Формат выдаваемых данных: JSON

Модели проекта: Категории, Произведения, Отзывы

Поддерживаемые методы: GET, POST, PUT, PATCH, DELETE

### Порядок запуска проекта:

Клонировать репозиторий и перейти в его каталог:

```
git clone https://github.com/DenisovYY/api_yamdb.git

cd api_yamdb.git
```

Настроить виртуальное окружение:

```
python3 -m venv env

source venv/Scripts/activate

python3 -m pip install --upgrade pip
```

Установить зависимости из файла requirements.txt:

```
pip install -r requirements.txt
```

Выполнить миграции и запустить проект:

```
python3 manage.py migrate

python3 manage.py runserver
```

### Примеры запросов:
1. Создание новой категории
POST на http://127.0.0.1:8000/api/v1/categories/
``` 
{
"name": "movies",
"slug": "movies"
}
``` 

2. Создание нового жанра
POST на http://127.0.0.1:8000/api/v1/genres/
``` 
{
"name": "comedy",
"slug": "comedy"
}
``` 

3. Добавление произведения
POST на http://127.0.0.1:8000/api/v1/titles/
``` 
{
"name": "To Kill a Mockingbird",
"year": 1960,
"description": "To Kill a Mockingbird, novel by American author Harper Lee, published in 1960. Enormously popular, it was translated into some 40 languages.",
"genre": [
"novel",
"drama"
],
"category": "books"
}
``` 

4. Частичное изменение данных о произведении (в адресе указывается id произведения)
PATCH на http://127.0.0.1:8000/api/v1/titles/1/
``` 
{
"description": "Just a story about a little girl called Scout.",
}
``` 

5. Публикация отзыва (в адресе указывается id произведения)
POST на http://127.0.0.1:8000/api/v1/titles/1/reviews/
``` 
{
"text": "What a terrific novel!",
"score": 10
}
``` 

6. Публикация комментария к отзыву (в адресе указывается id произведения и id отзыва)
POST на http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/
``` 
{
"text": "Низачот, кг/ам!"
}
``` 