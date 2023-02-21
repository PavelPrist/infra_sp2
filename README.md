# api_yamdb
api_yamdb
Проект YaMDb

Проект YaMDb собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок автоматически высчитывается средняя оценка произведения.

----------

### Документация к проекту

    Документация для API после установки доступна по адресу

    http://localhost/redoc/

----------
### Стек технологий использованный в проекте:
* Python 3.7
* Django 2.2
* Django Rest Framework
* Simple-JWT
* PostreSQL
* Nginx
* gunicorn
* Docker
* DockerHub
* Django==3.2
* djangorestframework==3.12.4
* djangorestframework-simplejwt==5.2.2

### Запуск проекта в dev-режиме

----------
1. Клонировать репозиторий и перейти в него в командной строке:
```bash
git clone git@github.com:PavelPrist/infra_sp2.git

cd yamdb_final
```

2. Cоздать и открыть файл ```.env``` с переменными окружения:
```bash
cd infra

touch .env
```

3. Заполнить ```.env``` файл с переменными окружения по примеру:
```bash
echo DB_ENGINE=django.db.backends.postgresql >> .env

echo DB_NAME=postgres l >> .env

echo POSTGRES_PASSWORD=postgres >> .env

echo POSTGRES_USER=postgres  >> .env

echo DB_HOST=db  >> .env

echo DB_PORT=5432  >> .env
```

4. Установка и запуск приложения в контейнерах :
```bash
docker-compose up -d --build
```

5. Запуск миграций, создание суперюзера, сбор статики и заполнение БД:
```bash
docker-compose exec web python manage.py migrate

docker-compose exec web python manage.py createsuperuser

docker-compose exec web python manage.py collectstatic --no-input

docker-compose exec web python manage.py loaddata fixtures.json
```

6. Импорт данных из csv для наполнения базы:

    В терминале наберите команду:
    ```bash
    docker exec -it yamdb_web bash
    ```
    Запустите команду импорта:
    ```bash
    python manage.py load_data
    ```

----------

### Примеры работы с API для всех пользователей

Для неавторизованных пользователей работа с API доступна в режиме чтения, что-либо изменить или создать не получится.

```
Права доступа: Доступно без токена.
GET /api/v1/categories/ - Получение списка всех категорий
GET /api/v1/genres/ - Получение списка всех жанров
GET /api/v1/titles/ - Получение списка всех произведений
GET /api/v1/titles/{title_id}/reviews/ - Получение списка всех отзывов
GET /api/v1/titles/{title_id}/reviews/{review_id}/comments/ - Получение списка всех комментариев к отзыву
Права доступа: Администратор
GET /api/v1/users/ - Получение списка всех пользователей
```

----------

### Пользовательские роли

- Аноним — может просматривать описания произведений, читать отзывы и комментарии.
- Аутентифицированный пользователь (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
- Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
- Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- Суперюзер Django — обладает правами администратора (admin)

----------

### Регистрация нового пользователя
Получить код подтверждения на переданный email.
Права доступа: Доступно без токена.
Использовать имя 'me' в качестве username запрещено.
Поля email и username должны быть уникальными.

Регистрация нового пользователя:

```
POST /api/v1/auth/signup/
```

```json
{
  "email": "string",
  "username": "string"
}

```

Получение JWT-токена:

```
POST /api/v1/auth/token/
```

```json
{
  "username": "string",
  "confirmation_code": "string"
}
```
_______

### Автор
Павел Сердюков.