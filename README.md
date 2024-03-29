
![Yamdb Workflow Status](https://github.com/absolut83/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg?branch=master&event=push)
# API_YAMDB 
REST API проект для сервиса YaMDb — сбор отзывов о фильмах, книгах или музыке. 

Проект развернут по адресу: http://absolut83.ddns.net/redoc/
## Описание 
 
Проект YaMDb собирает отзывы пользователей на произведения. 
Произведения делятся на категории: «Книги», «Фильмы», «Музыка». 
Список категорий  может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»). 
### Как запустить проект: 
Все описанное ниже относится к ОС Linux. 
Клонируем репозиторий и и переходим в него: 
```bash 
git clone git@github.com:absolut83/yamdb_final.git
cd yamdb_final 
cd api_yamdb 
``` 
 
Создаем и активируем виртуальное окружение: 
```bash 
python3 -m venv venv 
source /venv/bin/activate (source /venv/Scripts/activate - для Windows) 
python -m pip install --upgrade pip 
``` 
 
Ставим зависимости из requirements.txt: 
```bash 
pip install -r requirements.txt 
``` 

Переходим в папку с файлом docker-compose.yaml: 
```bash 
cd infra 
``` 
 
Поднимаем контейнеры (infra_db_1, infra_web_1, infra_nginx_1): 
```bash 
docker-compose up -d --build 
``` 

Выполняем миграции: 
```bash 
docker-compose exec web python manage.py makemigrations reviews 
``` 
```bash 
docker-compose exec web python manage.py migrate --run-syncdb
``` 

Создаем суперпользователя: 
```bash 
docker-compose exec web python manage.py createsuperuser 
``` 

Собираем статику: 
```bash 
docker-compose exec web python manage.py collectstatic --no-input 
``` 

Создаем дамп базы данных (нет в текущем репозитории): 
```bash 
docker-compose exec web python manage.py dumpdata > dumpPostrgeSQL.json 
``` 

Останавливаем контейнеры: 
```bash 
docker-compose down -v 
``` 

### Шаблон наполнения .env (не включен в текущий репозиторий) расположенный по пути infra/.env 
``` 
DB_ENGINE=django.db.backends.postgresql 
DB_NAME=postgres 
POSTGRES_USER=postgres 
POSTGRES_PASSWORD=postgres 
DB_HOST=db 
DB_PORT=5432 
``` 
### Документация API YaMDb 
Документация доступна по эндпойнту: http://absolut83.ddns.net/redoc/ 

## Об авторе
### Виталий Яремчук

absolut83@mail.ru

Telegram - @kuvalda684

#### Используемые технологии
- python 3.8.7
- django 3.0.5
- django rest framework 3.11.0
- simplejwt 4.6.0
- PostgreSQL 12.4
- nginx 1.19.3
- docker
