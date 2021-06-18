# test_app

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
Пользовательский интерфейс системы представляет собой приложение
построенное при помощи фреймворка [Vue 3](https://v3.vuejs.org/) и [Vuetify](https://vuetifyjs.com/en/)

## Структура приложения

Бóльшая часть проекта представляет собой автоматически сгенерированный `vue-cli` проект. Исходный код приложения находится в src и состоит из:

### Компоненты (components)

* Dialog.vue - диалоговое окно для создания объекта;
* DataTable.vue - таблица с карточками обьектов, в таблице есть возможность сортировки обьектов по выбранному полю по возрастанию и убыванию;

### Dockerfile

Пользовательский интерфейс строится в ходе многоступенчатой сборки nginx, после чего готовое приложение копируется в Nginx.

### Остальное

* App.vue - центр приложения, т.к. именно объект описываемый в данном файле подставляется в [DOM](https://habr.com/ru/post/243815/) index.html. 
* Dockerfile - Dockerfile
* babel.config.js - конфигурация [Babel](https://babeljs.io/docs/en/)
* package.json и package-lock.json - NPM-зависимости проекта
* vue.config.js - директивы [Webpack](https://habr.com/ru/post/514838/) для сбора проекта в production-режиме.

## Алгоритм работы с репозиторием

1. Запуск приложений в Docker Compose `docker-compose up -d --build`
2. Завершение работы `docker-compose down`

### Требования

На машине с доступом в Интернет должны быть установлены:

* git
* docker (пользователь, выполняющий скрипт, должен находиться в группе docker)
* docker-compose