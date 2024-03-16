# Django-manual
### Мануал по созданию и запуску проектов на Django

###### Последовательность создания и запуска проекта:
```shell
# В зависимости от платформы, может использоваться python или python3
# Создание виртуального окружения
python -m venv venv
# Активация виртуального окружения
source venv/bin/activate #Mac
venv\Scripts\activate #Windows

# Обновление пакетов виртуального окружения
pip install --upgrade pip


# Настройка нового проекта
#################################################################
# Создание нового проекта
django-admin startproject project_name

# Создание приложения проекта
python manage.py startapp app_name

# Регистрация нового приложение в конфигурационном файле

# settings.py
#-----------------------------------------
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_name',
]
#-----------------------------------------
#################################################################


# Установка зависимостей в ручную или автоматически через команду
pip install -r requirements.txt

# Создание миграции для БД
python manage.py makemigrations

# Миграция данных для таблиц
python manage.py migrate

# Загрузить имеющиеся или тестовые данные в БД
python manage.py loaddata data.json

# Создать суперпользователя
python manage.py createsuperuser

# Запуск локального сервера
python manage.py runserver
```


###### Запуск на сервере REG.RU через SSH:
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
Примечание: мануал для запуска проекта на хостинге во избежание лишних затрат на облачный VPS(Virtual Private Server)
```shell
# 1.Войдите в панель управления хостингом.
# 2.Перейдите в раздел Сайты, выберите домен, для которого вы хотите установить Django, и нажмите Изменить.
# 3.В разделе «Дополнительные возможности» включите CGI-скрипты, Python, выберите Версию Python
# 4.Подключитесь к хостингу по SSH.
# 5.Перейдите в каталог вашего пользователя с помощью команды:
cd ~

# Убедитесь, что вы в нужном каталоге, выполнив команду:
pwd

#6. Чтобы узнать доступные версии Python, выполните команду:
ls -la /opt/python/*/bin/python

# Для создания виртуального окружения выполните команду:
/opt/python/python-3.9.0/bin/python -m venv djangoenv

# 7. Активируйте ваше виртуальное окружение с помощью команды:
source djangoenv/bin/activate

# 8.Обновите pip, установите пакеты Django и mysqlclient с помощью команды:
#если вы используете Python 3.7.6 и ниже:
pip install --upgrade pip && pip install django && CFLAGS="-std=c99" pip install mysqlclient
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
# 11.Откройте настройки(settings.py) вашего проекта локально или через панель ISP менеджера:

# settings.py
#------------------------------------------------------------------------------------------

#ALLOWED_HOSTS = [] # Local
ALLOWED_HOSTS = ['domain_example.ru', 'www.domain_example.ru'] # Server

# Local
# DATABASES = {
#     'default': {
#         'ENGINE': 'django.db.backends.sqlite3',
#         'NAME': BASE_DIR / 'db.sqlite3',
#     }
# }

# Server
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
# Или
STATICFILES_DIRS = [
    BASE_DIR / "static/",
]
#------------------------------------------------------------------------------------------

# 12.Создайте каталог стилей для администратора со статическими файлами командой:
python manage.py collectstatic

# 13.Выполните миграцию в MySQL с помощью команды:
python manage.py migrate

# 14.Создайте в корневом каталоге вашего сайта конфигурационный файл passenger_wsgi.py и запишите в нем следующее:

# passenger_wsgi.py
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

#### Подключение по SSH и зависимости для работы проекта. При необходимости может размещаться в README.md:
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

# Рекомендации:
###### Шаблоны Templates
```text
Папку templates с шаблонами лучше размещать внутри приложения. в корневой каталог были вынесены только base-шаблоны. 
Захотел удалить приложение — удали одну папку, откати settings.py и маршрутизация в urls. Готово.
```

###### Виртуальное окружение Virtualenv 
```text
Virtualenv + Python requirements. Virtualenv будет изолировать настройки и зависимости Python/Django для каждого 
отдельного проекта. Это значит, что изменения одного сайта не затронут другие сайты. Также это может оказаться удобным, 
когда на сервере необходимо держать разные версии Django или Python.
```
