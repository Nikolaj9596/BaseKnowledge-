---
id: architecture hosting trading bots
aliases:
  - Architecture hosting trading bots
tags:
  - university
  - architecture
---
# Функционал

1. Пользователь 
	1. Регистция пользователя по email
	2. Профиль пользователя
	3. Аккаунты на торговых площадках
2. Тарговая платформа 
3. Криптовалюта
4. Тарговый боты
	1. Настройки таргового бота
5. Система аренды торгового бота
6. Статистика

## Структура проекта 

├── Core - ***Директория с доменными сущностями и бизнес логикой***
│   ├── Profile.Application - ***Бизнес логика***
│   └── Profile.Domain - ***Сущность***
├── Infrastructure
	└── Profile.Persistentce - ***Логика работы с базй данный***
├── Presintation
	└── ProfileWebApi - ***ASP.Net проект***
└── Tests


