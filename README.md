<h1 align="center"><a target="_blank" href="https://github.com/PivnoyFei/yamdb_final/">Проект yamdb_final</a></h1>

![Django-app workflow](https://github.com/PivnoyFei/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Описание
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

> Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

### Стек:
```bash Python 3.7, Django 2.2.19, DRF, Simple-JWT, PostgreSQL 13.0, Docker, Docker Hub, nginx, gunicorn 20.0.4, GitHub Actions, Yandex.Cloud.```

### Шаблон создания файла .env расположенный по пути infra/.env
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY=key
```

### Запуск проекта:
Клонируем репозиторий и переходим в него:
```bash
git clone https://github.com/PivnoyFei/yamdb_final
cd yamdb_final
```
Создаем и активируем виртуальное окружение для linux:
```bash
python3 -m venv venv
source venv/bin/activate
```
для Windows
```bash
python -m venv venv
source venv/Scripts/activate
```
Обновиляем pip:
```bash
python -m pip install --upgrade pip
```
Ставим зависимости из requirements.txt:
```bash
pip install -r api_yamdb/requirements.txt
```
Запускаем автотесты:
```bash
pytest
```

Отредактируйте файл ```nginx/default.conf``` и в строке ```server_name``` впишите IP виртуальной машины (сервера).
Скопируйте файлы ```docker-compose.yaml``` и ```nginx/default.conf``` из вашего проекта на сервер:

### Зайдите в репозиторий на локальной машине и отправьте файлы на сервер.
```bash
scp docker-compose.yaml <username>@<host>:/home/<username>/docker-compose.yaml
sudo mkdir nginx
scp default.conf <username>@<host>:/home/<username>/nginx/default.conf
```
В репозитории на Гитхабе добавьте данные в ```Settings - Secrets - Actions secrets```:

```
DOCKER_USERNAME - имя пользователя в DockerHub
DOCKER_PASSWORD - пароль пользователя в DockerHub

HOST - ip-адрес сервера
USER - пользователь
SSH_KEY - приватный ssh-ключ (публичный должен быть на сервере)
PASSPHRASE - кодовая фраза для ssh-ключа

DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY - секретный ключ приложения django

TELEGRAM_TO - id своего телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN - токен бота (получить токен можно у @BotFather, /token, имя бота)
```
### Как запустить проект на сервере:
Установите Docker и Docker-compose:
```bash
sudo apt install docker.io
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### После успешного деплоя:
Запуск docker-compose:
```bash
docker-compose up -d --build
```
Соберите статические файлы (статику):
```bash
sudo docker-compose exec web python manage.py collectstatic --no-input
```
Примените миграции:
```bash
sudo docker-compose exec web python manage.py makemigrations
sudo docker-compose exec web python manage.py migrate --noinput
```
Команда для заполнения базы начальными данными (необязательно):
```bash
docker-compose exec web python manage.py loaddata fixtures.json
```
Создайте суперпользователя Django:
```bash
sudo docker-compose exec web python manage.py createsuperuser
```

### Разработчики проекта
[Смелов Илья](https://github.com/PivnoyFei)
- тимлид, разработка ресурсов Auth и Users
- Докеризация, GitHub Actions.
- Автоматический деплой на боевой сервер YandexCloud.

[Юрий Кузнецов](https://github.com/KuznetsovYury)
- разработка ресурсов Categories, Genres и Titles

[Наумчук Владимир](https://github.com/arcievil)
- разработка ресурсов Review и Comments
