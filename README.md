![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) ![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)

## Учебный проект - Яндекс.Практика - Taski
Taski - Приложение для планирования своих задач.

###Установка
Клонируем репозиторий:
```python
git@github.com:Maksim1301/taski.git
```
Переходим в директорию проекта и запускаем его:
```python
cd taski
```
Создайте файл .env и заполните его своими данными:
```python
POSTGRES_USER=django_user
POSTGRES_PASSWORD=mysecretpassword
POSTGRES_DB=django
DB_HOST=db
DB_PORT=5432
```
### Деплой на сервере
Подключитесь к удаленному серверу

```python
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
```
Создайте на сервере директорию taski файл .env 

```python
mkdir taski
```

Запускаем проект в режиме демона:
```python
docker compose -f docker-compose.production.yml up -d
```
Далее нужно собрать статику проекта и выполнить миграции:
```python
docker compose -f docker-compose.production.yml exec backend python manage.py migrate

docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic

docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
Проект настроен на работу на 8000 порту поэтому необходимо изменить конфиг Nginx:
```python
nano /etc/nginx/sites-enabled/default
```
```python
location / {
    proxy_pass http://127.0.0.1:8000;
}
```
Проверьте работоспособность конфига и перезапустите Nginx:

```python
sudo nginx -t 
sudo service nginx reload
```