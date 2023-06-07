![Deploy badge](https://github.com/GUDvin-lab/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

# API для Yamdb
REST API для плафтормы Yamdb
Проект интерфейса обмена данными для платформой отзывов на произведения искусства Yamdb. С помощью этого интерфейса возможно создание отзывов, их редактирование, добавление комментариев к отзывам, а также оценка произведений и трансляция рейтинга оценок. 

## Используемый стек технологий
- Язык Python
- Фреймворк Django
- Фреймворк Django REST framework
- Язык разметки YAML

## Установка
Клонировать репозиторий и перейти в него в командной строке:

```
git clone git@github.com:GUDvin-lab/infra_sp2.git
```

```
cd infra_sp2
```

Cоздать и активировать виртуальное окружение:

```
py -3.7 -m venv 
```

```
. venv/bin/activate
```

Установить зависимости из файла requirements.txt:

```
python3 -m pip install --upgrade pip
```

```
pip install -r api_yamdb/requirements.txt
```

Переходим в папку infra/ с файлом docker-compose.yaml:
```
cd infra
```

Cоберите контейнеры и запустите их:
```
docker-compose up -d --build
```

Выполнить миграции:

Выполняем миграции:
```
docker-compose exec web python manage.py makemigrations reviews
```
```
docker-compose exec web python manage.py migrate
```

Создаем суперпользователя:
```
docker-compose exec web python manage.py createsuperuser
```

Србираем статику:
```
docker-compose exec web python manage.py collectstatic --no-input
```

Создаем дамп базы данных:
```
docker-compose exec web python manage.py dumpdata > fixtures.json 
```

Останавливаем контейнеры:
```
docker-compose down -v
```

Проект запущен и доступен по адресу: [localhost](http://localhost/admin/)

## Загрузка тестовых значений в БД

Чтобы загрузить тестовые значения в базу данных перейдите в каталог проекта и скопируйте файл базы данных в контейнер приложения:
```
docker cp <DATA BASE> <CONTAINER ID>:/app/<DATA BASE>
```
Перейдите в контейнер приложения и загрузить данные в БД: 
```
docker container exec -it <CONTAINER ID> bash
python manage.py loaddata <DATA BASE>
```



### Шаблон наполнения .env файла, файл расположен по пути infra/.env
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```


## Примеры запросов

Запросы к API начинаются с /api/v1/

### Регистрация нового пользователя

POST /auth/signup/

Request
```
{
  "email": "user@example.com",
  "username": "string"
}
```

Response
```
{
  "email": "string",
  "username": "string"
}
```

### Получение JWT-токена

POST /auth/token/

Request
```
{
  "username": "string",
  "confirmation_code": "string"
}
```

Response
```
{
  "token": "string"
}
```

### Получение списка всех категорий

GET /categories/

Response
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "name": "string",
      "slug": "string"
    }
  ]
}
```

### Получение списка всех жанров

GET /genres/

Response
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "name": "string",
      "slug": "string"
    }
  ]
}
```

### Получение списка всех произведений

GET /titles/

Response
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "name": "string",
      "year": 0,
      "rating": 0,
      "description": "string",
      "genre": [
        {
          "name": "string",
          "slug": "string"
        }
      ],
      "category": {
        "name": "string",
        "slug": "string"
      }
    }
  ]
}
```

### Получение списка всех отзывов

GET /titles/{title_id}/reviews/

Response
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "text": "string",
      "author": "string",
      "score": 1,
      "pub_date": "2019-08-24T14:15:22Z"
    }
  ]
}
```

### Получение списка всех комментариев к отзыву

GET /titles/{title_id}/reviews/{review_id}/comments/

Response
```
{
  "count": 0,
  "next": "string",
  "previous": "string",
  "results": [
    {
      "id": 0,
      "text": "string",
      "author": "string",
      "pub_date": "2019-08-24T14:15:22Z"
    }
  ]
}
```

### Добавление пользователя

POST /users/

Request
```
{
  "username": "string",
  "email": "user@example.com",
  "first_name": "string",
  "last_name": "string",
  "bio": "string",
  "role": "user"
}
```

Response
```
{
  "username": "string",
  "email": "user@example.com",
  "first_name": "string",
  "last_name": "string",
  "bio": "string",
  "role": "user"
}
```