# Проект YaMDb
## Описание
###### Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка».

### Используется:

[![Python](https://img.shields.io/badge/-Python_3.7.9-464646??style=flat-square&logo=Python)](https://www.python.org/downloads/)
[![Django](https://img.shields.io/badge/-Django-464646??style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![Django](https://img.shields.io/badge/-Django_rest_framework_3.12.4-464646??style=flat-square&logo=Django)](https://www.django-rest-framework.org)

## Установка
_на Mac или Linux используем Bash_
_Для Windows PowerShell_

#### Клонируем репозиторий на локальную машину:
https://github.com/PythonGun/api_yamdb
git clone git@github.com:PythonGun/api_yamdb.git

#### Создаем и активируем виртуальное окружение:
_на Mac или Linux_
python3 -m venv venv
source venv/bin/activate 

_для Windows_
python -m venv venv
source venv/Scripts/activate

#### Устанавливаем зависимости:
pip install -r requirements.txt

#### Запускаем миграции:
python manage.py migrate

#### Запускаем проект:
python manage.py runserver

#### Пользовательские роли
- Аноним — может просматривать описания произведений, читать отзывы и комментарии.
- Аутентифицированный пользователь (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
- Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
- Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- Суперюзер Django — обладет правами администратора (admin)

#### Регистрация нового пользователя
Получить код подтверждения на переданный email.
Права доступа: Доступно без токена.
Использовать имя 'me' в качестве username запрещено.
Поля email и username должны быть уникальными.


# Примеры
## Права доступа: Доступно без токена.
GET /api/v1/categories/ - Получение списка всех категорий
GET /api/v1/genres/ - Получение списка всех жанров
GET /api/v1/titles/ - Получение списка всех произведений
GET /api/v1/titles/{title_id}/reviews/ - Получение списка всех отзывов
GET /api/v1/titles/{title_id}/reviews/{review_id}/comments/ - Получение списка всех комментариев к отзыву

## Права доступа: Администратор
__GET /api/v1/users/__ - Получение списка всех пользователей

## Получение JWT-токена:
__POST /api/v1/auth/token/__
```
{
  "username": "string",
  "confirmation_code": "string"
}
```

## Примеры работы с API для авторизованных пользователей
Добавление категории:

Права доступа: Администратор.
__POST /api/v1/categories/__
```
{
  "name": "string",
  "slug": "string"
}
```
Удаление категории:
Права доступа: Администратор.
__DELETE /api/v1/categories/{slug}/__

Добавление жанра:
Права доступа: Администратор.
__POST /api/v1/genres/__
```
{
  "name": "string",
  "slug": "string"
}
```

Обновление публикации:
__PUT /api/v1/posts/{id}/__
```
{
"text": "string",
"image": "string",
"group": 0
}
```

Добавление произведения:
Права доступа: Администратор. 
__POST /api/v1/titles/__
```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```

Добавление произведения:
Права доступа: Доступно без токена
__GET /api/v1/titles/{titles_id}/__
```
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
```

## Работа с пользователями:
Получение списка всех пользователей.
Права доступа: Администратор
__GET /api/v1/users/__ - Получение списка всех пользователей

Добавление пользователя:
Права доступа: Администратор
Поля email и username должны быть уникальными.
__POST /api/v1/users/__ - Добавление пользователя
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

Получение пользователя по username:
Права доступа: Администратор
__GET /api/v1/users/{username}/__ - Получение пользователя по username

Изменение данных пользователя по username:
Права доступа: Администратор
__PATCH /api/v1/users/{username}/__ - Изменение данных пользователя по username
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

Удаление пользователя по username:
Права доступа: Администратор
__DELETE /api/v1/users/{username}/__ - Удаление пользователя по username

Получение данных своей учетной записи:
Права доступа: Любой авторизованный пользователь
__GET /api/v1/users/me/__ - Получение данных своей учетной записи

Изменение данных своей учетной записи:
Права доступа: Любой авторизованный пользователь
__PATCH /api/v1/users/me/__ - Изменение данных своей учетной записи


#### Ознакомиться с полным функционалом и примерами можно по адресу http://127.0.0.1:8000/redoc (доступен после запуска проекта)

## Авторы
Денис Баринов, Владыко Ирина
- :white_check_mark: [Тузовский Виктор](https://github.com/yumeko6)
- :white_check_mark: [Баринов Денис](https://github.com/PythonGun)
- :white_check_mark: [Владыко Ирина](https://github.com/VladykoIra)
























Проект сделан в рамках учебного процеса по специализации Python-разработчик (back-end) Яндекс.Практикум.