# Django-manual
Мануал для запуска проектов на Django

#### Создание проекта:
```shell
#
# Загрузить данные в БД
python manage.py loaddata data.json
# Создать суперпользователя
python manage.py createsuperuser
```

#### Запуск на Mac:
```shell
python3 -m venv venv
source venv/bin/activate 
pip install --upgrade pip
pip install -r requirements.txt
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py createsuperuser
python3 manage.py runserver
```
#### Запуск на Windows:
```shell
python -m venv venv
venv\Scripts\activate 
pip install --upgrade pip
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

#### Запуск на сервере REG.RU через SSH:
```shell
# Подключение по SSH:
 ssh: u0000000@ip_address_server
pass: ****************
cd ~
pwd
source djangoenv/bin/activate
cd www/
git clone https://ghp_exampleQNiTlf3FnDlmpcegO9PnDIM0oEojE@github.com/git_username/project_name.git
cd domain_example.ru/
pwd
git pull
# Если вы изменили файлы проекта и хотите увидеть изменения, вам необходимо перезапустить проект. Для этого создайте файл: 
touch .restart-app
# в корневой директории вашего сайта. После перезапуска проекта файл будет удалён автоматически.
```

#### [Как установить Django на хостинг REG.RU](https://help.reg.ru/support/hosting/php-asp-net-i-skripty/kak-ustanovit-django-na-hosting):
```shell
# 5.Перейдите в каталог вашего пользователя с помощью команды:
cd ~

# Убедитесь, что вы в нужном каталоге, выполнив команду:
pwd

# Чтобы узнать доступные версии Python, выполните команду:
ls -la /opt/python/*/bin/python

# Для создания виртуального окружения выполните команду:
/opt/python/python-3.9.0/bin/python -m venv djangoenv

# 7. Активируйте ваше виртуальное окружение с помощью команды:
source djangoenv/bin/activate

# 8.Обновите pip, установите пакеты Django и mysqlclient с помощью команды:
# если вы используете версию Python 3.8.6 и выше:
pip install --upgrade pip && pip install django==4.1.10 && CFLAGS="-std=c99" pip install mysqlclient

# 9.Перейдите в корневой каталог вашего сайта с помощью команды:
cd www/domain_example.ru/

# Убедитесь, что вы в нужном каталоге, выполнив команду:
pwd

# 10.Важно: перед созданием проекта удалите все файлы и папки из каталога вашего сайта.
# Создайте новый проект в корневом каталоге:
django-admin startproject project_name
# Перейдите в каталог проекта
cd project_name
# 11.Откройте настройки вашего файла через панель ISP менеджера:

# project_name/project_name/settings.py
ALLOWED_HOSTS = ['domain_example.ru', 'www.domain_example.ru']

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'u0000000_exampledb',
        'USER': 'u0000000_name',
        'PASSWORD': '****************',
        'HOST': 'localhost',
        'OPTIONS': {
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
        },
    }
}
# Ниже секции STATIC_URL добавьте новую секцию: 
STATIC_ROOT = 'static/'

# 12.Создайте каталог стилей для администратора со статическими файлами командой:
python manage.py collectstatic

# 13.Выполните миграцию в MySQL с помощью команды:
python manage.py migrate

# 14.Создайте в корневом каталоге вашего сайта конфигурационный файл с именем passenger_wsgi.py и запишите в нем следующее:
#------------------------------------------------------------------------------------------
# -*- coding: utf-8 -*-
import os, sys
sys.path.insert(0, '/var/www/u0000000/data/www/domain_example.ru/project_name')
sys.path.insert(1, '/var/www/u0000000/data/djangoenv/lib/python3.9/site-packages')
os.environ['DJANGO_SETTINGS_MODULE'] = 'project_name.settings'
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
#------------------------------------------------------------------------------------------
```

#### Удалённое подключение по SSH и установка зависимостей в ручном режиме:

---
При необходимости может размещаться в README.md
###### SSH:
```shell
 ssh: username@ip_address_server
pass: ****************
```
###### Requirements:
```shell
pip install package-name
pip install package-name==0.00.00
```
