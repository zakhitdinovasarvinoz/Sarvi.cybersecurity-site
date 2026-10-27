# CyberSecurity Website

Учебный проект по кибербезопасности и контейнеризации.

## Технологии
- HTML и CSS
- Docker
- Nginx
- OCI Container Standards

## Запуск

Сборка Docker-образа:

    docker build -t cybersecurity-site .

Запуск контейнера:

    docker run -d --name cybersecurity-container -p 8080:80 cybersecurity-site

Открыть сайт: http://localhost:8080

## Остановка

    docker stop cybersecurity-container
