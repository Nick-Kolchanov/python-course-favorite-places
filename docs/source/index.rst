Любимые места
=============

Сервис для сохранения информации о любимых местах.

Зависимости
===========

Установите требуемое ПО:

1. Docker для контейнеризации – |link_docker|

.. |link_docker| raw:: html

   <a href="https://www.docker.com" target="_blank">Docker Desktop</a>

2. Для работы с системой контроля версий – |link_git|

.. |link_git| raw:: html

   <a href="https://github.com/git-guides/install-git" target="_blank">Git</a>

3. IDE для работы с исходным кодом (необязательно) – |link_pycharm|

.. |link_pycharm| raw:: html

    <a href="https://www.jetbrains.com/ru-ru/pycharm/download" target="_blank">PyCharm</a>

Установка
=========

Клонируйте репозиторий проекта в свою рабочую директорию:

.. code-block:: console
    git clone https://github.com/Nick-Kolchanov/python-course-favorite-places.git

1. Чтобы сконфигурировать проект, скопируйте и переименуйте файл `.env.sample` в `.env`.

    .. code-block:: console
        cp .env.sample .env

    Этот файл содержит переменные окружения, значения которых будут общими для всего приложения.
    Файл примера (`.env.sample`) содержит набор переменных со значениями по умолчанию.
    Созданный файл можно настроить в зависимости от окружения.

    .. warning::
        Никогда не добавляйте в систему контроля версий заполненный файл `.env` для предотвращения компрометации информации о конфигурации приложения.

2. Соберите Docker-контейнер с помощью Docker Compose:
    .. code-block:: console
        docker compose build
    Данную команду необходимо выполнять повторно в случае обновления файла `requirements.txt`.

3. Для корректного функционирования приложения, необходимо создать базу данных и таблицы в ней с помощью миграций:
    .. code-block:: console
        docker compose run favorite-places-app alembic upgrade head

4. После сборки контейнеров можно их запустить командой:
    .. code-block:: console
        docker compose up

    Когда запуск завершится, сервер начнет работать по адресу `http://0.0.0.0:8010`.


Использование
=============

Чтобы запустить собранные контейнеры используется команда:
.. code-block:: console
    docker compose up

Работа с базой данных
---------------------

Для инициализирования миграций используйте команду
.. code-block:: console
    docker compose exec favorite-places-app alembic init -t async migrations

Она создаст директорию с конфигурационными файлами миграций.
Чтобы создать новые файлы миграций после обновления моделей, применяется команда
.. code-block:: console
    docker compose run favorite-places-app alembic revision --autogenerate  -m "your description"

Чтобы запустить созданные миграции:
.. code-block:: console
    docker compose run favorite-places-app alembic upgrade head

Автоматизация
=============

В проекте есть `Makefilr`, содержащий некоторый набор команд:

1. Собрать контейнер:
    .. code-block:: console
        make build

2. Сгенерировать документацию:
    .. code-block:: console
        make docs-html

3. Форматировать код:
    .. code-block:: console
        make format

4. Воспользоваться линтером:
    .. code-block:: console
        make lint

5. Провести автотесты:
    .. code-block:: console
        make test

6. Провести форматирование, анализ линтером и тесты в одной команде:
    .. code-block:: console
        make all

Тестирование
============

Автотесты можно запустить командой:
.. code-block:: console
    make test

Отчет о тестировании располагается в `src/htmlcov/index.html`
