# Django-manual
### Мануал по созданию и запуску проектов на Django
###### Создание виртуального окружения:
```shell
python -m venv venv
```

###### Активация виртуального окружения на Mac:
```shell
source venv/bin/activate
```

###### Активация виртуального окружения на Windows:
```shell
venv\Scripts\activate
```

###### Обновление пакетов виртуального окружения:
```shell
pip install --upgrade pip
```

## Настройка нового проекта

###### Создание нового проекта:
```shell
django-admin startproject project_name
```

###### Создание приложения проекта:
```shell
python manage.py startapp app_name
```

###### Регистрация нового приложение в конфигурационном файле(settings.py):
```text
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_name',
]
```

###### Установка зависимостей в ручную или автоматически через команду:
```shell
pip install -r requirements.txt
```

###### Создание миграции для БД:
```shell
python manage.py makemigrations
```

###### Миграция данных для таблиц:
```shell
python manage.py migrate
```

###### Загрузить данные в БД:
```shell
python manage.py loaddata data.json
```

###### Создать суперпользователя:
```shell
python manage.py createsuperuser
```

###### Запуск локального сервера:
```shell
python manage.py runserver
```


# Запуск на сервере REG.RU через SSH

###### Подключение по SSH:
```text
 ssh: u0000000@ip_address_server
pass: ****************
```

###### Перейти в корневой каталог пользователя:
```shell
cd ~
```

###### Получить путь к директории в которой находишься:
```shell
pwd
```

###### Активировать виртуальное окружение:
```shell
source djangoenv/bin/activate
```

###### Перейти в директорию www, где находятся проекты сайтов:
```shell
cd www/
```

###### Клонировать проект:
```shell
git clone https://ghp_exampleQNiTlf3FnDlmpcegO9PnDIM0oEojE@github.com/git_username/project_name.git
```

###### Перейти в директорию клонированного проекта:
```shell
cd domain_example.ru/
```

###### Обновить проект
```shell
git pull
```

###### Если вы изменили файлы проекта и хотите увидеть изменения, вам необходимо перезапустить проект. Для этого создайте файл:
```shell
touch .restart-app
```
###### в корневой директории вашего сайта. После перезапуска проекта файл будет удалён автоматически.


# Как [установить](https://help.reg.ru/support/hosting/php-asp-net-i-skripty/kak-ustanovit-django-na-hosting) Django на хостинг REG.RU:
Примечание: мануал для запуска проекта на хостинге во избежание лишних затрат на облачный VPS(Virtual Private Server).

1. Войдите в панель управления хостингом.
2. Перейдите в раздел Сайты, выберите домен, для которого вы хотите установить Django, и нажмите Изменить.
3. В разделе «Дополнительные возможности» включите CGI-скрипты, Python, выберите Версию Python.
4. Подключитесь к хостингу по SSH.
5. Перейдите в каталог вашего пользователя с помощью команды:
```shell
cd ~
```

- Убедитесь, что вы в нужном каталоге, выполнив команду:
```shell
pwd
```

6. Чтобы узнать доступные версии Python, выполните команду:
```shell
ls -la /opt/python/*/bin/python
```

- Для создания виртуального окружения выполните команду:
```shell
/opt/python/python-3.9.0/bin/python -m venv djangoenv
```

7. Активируйте ваше виртуальное окружение с помощью команды:
```shell
source djangoenv/bin/activate
```

8. Обновите pip, установите пакеты Django и mysqlclient с помощью команды:
- если вы используете Python 3.7.6 и ниже:
```shell
pip install --upgrade pip && pip install django && CFLAGS="-std=c99" pip install mysqlclient
```

- если вы используете версию Python 3.8.6 и выше:
```shell
pip install --upgrade pip && pip install django==4.1.10 && CFLAGS="-std=c99" pip install mysqlclient
```

9. Перейдите в корневой каталог вашего сайта с помощью команды:
```shell
cd www/domain_example.ru/
```

- Убедитесь, что вы в нужном каталоге, выполнив команду:
```shell
pwd
```

10. Важно: перед созданием проекта удалите все файлы и папки из каталога вашего сайта.
- Создайте новый проект в корневом каталоге:
```shell
django-admin startproject project_name
```

- Перейдите в каталог проекта
```shell
cd project_name
```

11. Откройте настройки конфигурационного файла(settings.py) вашего проекта:
- Для запуска локально или на сервере:
```text
#ALLOWED_HOSTS = [] # Local
ALLOWED_HOSTS = ['domain_example.ru', 'www.domain_example.ru'] # Server
```

- Настройки по умолчанию для sqlite3 можно оставить закомментированными.
- Это позволяет не настраивать mysql на локальной машине, а тестировать на sqlite3 БД.
- Добавить ниже новые mysql для работы на сервере:
```text
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
```

- Для работы с файлами шаблонов изменить 'DIRS': [os.path.join(BASE_DIR, 'templates'), ],
```text
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates'), ],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

- Для работы со статичными файлами:
```text
# Ниже секции STATIC_URL добавьте новую секцию: 
STATIC_ROOT = 'static/'
# Или
STATICFILES_DIRS = [
    BASE_DIR / "static/",
]
```

12. Создайте каталог стилей для администратора со статическими файлами командой:
```shell
python manage.py collectstatic
```

13. Выполните миграцию в MySQL с помощью команды:
```shell
python manage.py migrate
```

14. Создайте в корневом каталоге вашего сайта конфигурационный файл passenger_wsgi.py:
```text
# -*- coding: utf-8 -*-
import os, sys
sys.path.insert(0, '/var/www/u0000000/data/www/domain_example.ru/project_name')
sys.path.insert(1, '/var/www/u0000000/data/djangoenv/lib/python3.9/site-packages')
os.environ['DJANGO_SETTINGS_MODULE'] = 'project_name.settings'
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```


# SSH and Requirements
#### При необходимости может размещаться в README.md
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

# Рекомендации
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
