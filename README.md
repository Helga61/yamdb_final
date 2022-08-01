![example workflow](https://github.com/helga61/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Ссылка на проект: http://51.250.23.31/admin
## Описание
Бэкенд проекта YaMDb.

Проект собирает отзывы пользователей на произведения. API позволяет создавать, редактировать, удалять отзывы, а также ставить произведению оценку.

## Использованные технологии
- django2
- djangorestframework
- python3
- Docker
- Postgres

## Запуск проекта
Склонировать репозиторий на компьютер

```
git clone git@github.com:Helga61/infra_sp2.git
```

В директории infra/ создать файл .env, наполнить по шаблону:

```
SECRET_KEY='testkey'
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

Собрать образ, выполнив команду docker-compose up --build -d

Выполнить миграции, собрать статику и создать суперпользователя

```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```

Заполнить базу данными:

```
docker-compose exec web python manage.py loaddata fixtures.json
```

Сервис доступен по адресу http://51.250.23.31/admin

## документация API

Документация API доступна по адресу http://51.250.23.31/redoc/

### Примеры работы с API

#### Получение списка всех произведений
Доступно незарегистрированному пользователю

Запрос
```
GET http://localhost/api/v1/titles/
```
Ответ
```
[
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
]
```

#### Добавление комментария к отзыву:
Доступно после аутентификации

Запрос
```
POST http://localhost/api/v1/titles/{title_id}/reviews/{review_id}/comments/

{
  "text": "string"
}
```
Ответ
```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "pub_date": "2019-08-24T14:15:22Z"
}
```

#### Регистрация пользователей

1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами `email` и `username` на эндпоинт `/api/v1/auth/signup/`.
2. **YaMDB** отправляет письмо с кодом подтверждения (`confirmation_code`) на адрес `email`.
3. Пользователь отправляет POST-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (JWT-токен).
4. При желании пользователь отправляет PATCH-запрос на эндпоинт `/api/v1/users/me/` и заполняет поля в своём профайле.

## Авторы проекта

- Жуков Артем - [Art-py](https://github.com/Art-py)
- Пылаев Данил - [Danstiv](https://github.com/danstiv)
- Воронюк Ольга - [Helga61](https://github.com/Helga61)
- Тростянский Дмитрий - [trdeman](https://github.com/trdeman)