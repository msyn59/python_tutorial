# Django チュートリアル with PostgreSQL

## 環境構築

### Django インストール

``` cmd
pip install Django
```

pipバージョンが古かったので更新

``` cmd
python -m pip install --upgrade pip
```

### DB設定 (posgraSQL psycopg)

[psycopg](http://initd.org/psycopg/download/)

``` cmd
pip install -U pip # make sure to have an up-to-date pip
pip install psycopg2
```

### Django 動作確認

``` cmd
>>> import django
>>> print(django.get_version())
2.1.7
```

## チュートリアル

### バージョン確認

``` cmd
python -m django --version
2.1.7
```

## 1.プロジェクトの作成

``` cmd
django-admin startproject mysite
```

### pylint インストール

manage.py を参照したら vscode に pylint がインストールされていないと警告がでたのでインストール

``` cmd
python -m pip install -U pylint --user
```

また、インストール先に PATH がとおっていないと警告がでたので PATH を通す

pylint のみの場合 django のモジュールでエラーがでるので pylint-django をインストールする

``` cmd
pip install pylint-django
```

vscode の setting.json に 下記を追加

``` json
"python.linting.pylintArgs": [
    "--load-plugins=pylint_django"
]
```

### 開発用サーバーで動作確認

``` cmd
python manage.py runserver
```

## アプリケーションの作成

``` cmd
cd mysyte
python manage.py startapp polls
```

mysyte/polls が作成される

### はじめてのビュー作成

polls/views.py を編集

``` py
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

ビューとURLの対応づけをするためには URLconf が必要 polls ディレクトリに URLconf を作るために urls.py を作成する

polls/urls.py

``` py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

URLconf に polls.urls モジュールの記述を反映させる

mysite/urls.py

``` py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

追加したビュー、URLconf の対応を確認

開発用サーバーを起動後にサイトを確認

``` cmd
python manage.py runserver
````

[localhost](localhost:8000/polls)

## 2.Databaseの設定

mysite/settings.py を編集

``` py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'trydjango',
        'USER': 'trydjango',
        'PASSWORD': 'trydjango',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}

LANGUAGE_CODE = 'ja'

TIME_ZONE = 'Asia/Tokyo'

```

database の migrate

``` py
python manage.py migrate
```

postgresql のユーザーを作っていなかったので pgAdmin で作成

ユーザー : trydjango
パスワード : trydjango
role : postgres

migrate の再実行

``` py
python manage.py migrate
```
