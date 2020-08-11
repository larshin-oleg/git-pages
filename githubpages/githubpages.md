# Настройка git-pages

## Создание репозитория:

Залогинимся в нашем аккаунте github. На вкладке «Repositories» выберем «New»:

![](media/35ce995cebe89d1c9abf568fdd8e277c.png)

Введем название нового репозитория и выберем настройки приватности:

![](media/7f94ca297d9d09e373a544afd48b17f2.png)

## Первый коммит:

Нам необходимо сделать первый коммит в репозиторий, прежде чем появится
возможность активировать git-pages.

В папке с подготовленными md-файлами вызовем командную строку Git. Для загрузки
файлов в репозиторий необходимо выполнить следующие команды:

Инициализация локального репозитория (выполняется только 1 раз):

git init

Добавляем файлы для загрузки в репозиторий:

git add .

Готовим коммит:

Git commit –m "Комментарий к коммиту"

git remote add origin git\@github.com:larshin-oleg/git-pages.git

Адрес репозитория смотрим в на вкладке "Code"

![](media/41950e0b1212ff1a4fc3cd7a1f7e2728.png)

Загружаем файлы в репозиторий командой:

git push origin master

## Активация git-pages:

Для включения git-pages перейдем в настройки репозитория. В разделе "Options"
найдем раздел «GitHub Pages» и выберем ветку, которая будет отображаться в
git-pages (в нашем случае это ветка master).

![](media/5d926023197ae7e2982b4129beed8ea6.png)

После применения настроек видим адрес, на котором располагается наша
документация:

![](media/e95d4c2dee42e5b0f4d14cbf5b365734.png)

Для перехода к документации необходимо приписать в конец адреса название нашей
стартовой страницы. В моем случае полный адрес выглядит так:

<https://larshin-oleg.github.io/git-pages/Home>