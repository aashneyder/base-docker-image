# base-docker-image

There are will be test proj.
Goals:
1. Learn how to create base image using debootstrap and docker
2. Learn how to use that image in docker-compose.yml

Что было сделано:
1) Создан каталог ~/base-docker-image/my-debian-image, где происходило создание базового образа на основе debian:
- sudo debootstrap --arch=amd64 buster rootfs
- каких-либо донастроек для ФС не производилось
- собран базовый образ debian docker build -t my-debian-image:v1 -f Dockerfile-base .
2) в каталоге проекта base-docker-image:
- файл main.py с полезной нагрузкой проекта: скрипт вывода сообщения на экран, скрипт должен запускаться в контейнере
- Dockerfile для создания образа рабочего контейнера (на основе базового из п.1)
- сборка образа docker build -t test-img-proj:v1 .
- файл docker-compose.yml для запуска контейнера через docker-compose up

Запуск прошел успешно, контейнер вполнил скрипт и завершился без проблем.
