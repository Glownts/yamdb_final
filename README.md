![Workflow badge](https://github.com/Glownts/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Адрес сервера
158.160.62.199

### Описание
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title).
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку
Произведения делятся на категории: «Книги», «Фильмы», «Музыка».
Список категорий (Category) может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»).

Добавлять произведения, категории и жанры может только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

Добавлять отзывы, комментарии и ставить оценки могут только аутентифицированные пользователи.

## Cистемные требования:
-   python 3.7

### Стек технологий использованный в проекте:
-   Python
-   Django
-   Django REST Framework
-   REST API
-   SQLite
-   Аутентификация по JWT-токену

### Алгоритм регистрации пользователей
Для добавления нового пользователя нужно отправить POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).
Если пользователя создаёт администратор, например, через POST-запрос на эндпоинт api/v1/users/ — письмо с кодом отправлять не нужно (описание полей запроса для этого случая — в документации).

## Запуск проекта
1. Клонируйте репозиторий

git clone https://github.com/Glownts/infra_sp2


2. Включените Docker Desktop


3. Запустите компоновку файла и разверните проект

из директории с Dockerfile: docker -build -t <username/projectname:tag>
из директории с docker-compose: docker-compose up -d --build

4. Выполните сбор статики и миграции

docker-compose exec web python manage.py collectstatic --no-input
docker-compose exec web python manage.py migrate


5. Создайте суперпользователя (при необходимости)

docker-compose exec web python manage.py createsuperuser



## Загрузка данных в БД из csv
Команда python manage.py load_data загружает данные из csv в БД.
Если данные уже есть в БД, выдаст ошибку ALREDY_LOADED_ERROR_MESSAGE.
Загружать данные стоит до создания суперпользователя. Иначе придется
удалять БД и снова делать миграции.


## Документация
Документация будет доступна после запуска проекта по адресу `/redoc/`.

## Шаблон .env-файла

SECRET_KEY='' # секретный ключ
DB_ENGINE='' # тип БД
DB_NAME='' # имя БД
POSTGRES_USER='' # логин для подключения к БД
POSTGRES_PASSWORD='' # пароль для подключения к БД
DB_HOST='' # название сервиса
DB_PORT='' # порт для подключения к БД
