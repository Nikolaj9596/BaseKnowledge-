## Стэк
- Django
- DRF
- Docker
- Docker-compose
- Celery
- RabbitMQ

#### Бизнес логика:
1. Регистрация пользователей
2. Модуль для конфигураций и констант
3. Система управления разрешениями доступа
4. Cервис переводов
5. Мултиязычность
6. Профиль пользователя 
7. Уведомления
9. Биржи
10.  FAQ
11. Инструкции и Обучение
12. Панель Управления ботами
	1. Возможность создать ботов
	2. Менять стратегию
	3. Отслеживать состояние
	4. Отслеживать все проведенные сделки и их логику
13. Система оплаты ботов и прочих услуг
	1. Возможность оплатить криптой
	2. Возможность оплатить картой
	3. Подключить систему быстрых платежей
	4. Транзакции
	5. Кошелек пользователя
14. Панель администратора
	1. Мониторинг транзакций 
	2. Мониторинг ботов
	3. Мониторинг состояния микросервисов
	4. Статистика
		 1.  Кол-во отключения от торгов бота
		 2. Профит и Убыток за разные периуды
		 3. Общая статистика сколько боты принесли на аренде за разные периуды
		 4. Самые успешные стратегии
15. Система лояльности (драг. метал)
16. Система для продвижения и реклам

### Высокоуровневая абстрактная схема API сервера
```plantuml
skinparam interface {
  backgroundColor RosyBrown
  borderColor orange
}

skinparam component {
  FontSize 13
  BackgroundColor<<Apache>> Red
  BorderColor<<Apache>> #FF6655
  FontName Courier
  BorderColor black
  BackgroundColor gold
  ArrowFontName Impact
  ArrowColor #FF6655
  ArrowFontColor #777777
}

package "Config App" {
  [Configurate]
}

package "Monitoring App" {
  [Monitoring]
  [Statistics]
}

package "Educational App" {
  [Instruction]
  [Course]
  [FAQ]
}

package "Auth App" {
  [Authorization and authentication module]
}

package "User App" {
  [User Push Token Session]
  [Profile]
  [Trading Platform Account]
}

package "Payment App" {
  [Wallet]
  [Transaction]
}

package "Notification App" {
  [Notification]
}

package "Trading Platform App" {
  [Trading Platform]
}

package "Trading Bot App" {
  [Bot]
  [Stratage]
}


[Authorization and authentication module] --> [User Push Token Session]
[User Push Token Session] --> [Notification]
[Trading Platform Account] --> [Trading Platform]
[User Push Token Session] --> [Profile]
[User Push Token Session] --> [Trading Platform Account]
[Transaction] --> [Wallet]
[User Push Token Session] --> [Wallet]
[Bot] --> [Stratage]
[Bot] --> [Trading Platform]

```
### Описание Архитектуры

1. [Архитектура модуля авторизации и аутификации](Architecture%20Auth%20App.md)
2. [Архитектура модуля ролей и доступов пользователя](Architecture%20User%20App.md)
3. [Архитектура модуля торговых платформ](Architecture%20Trading%20Platform%20App.md)
4. [Архитектура модуля ботов](Architecture%20Bot%20App.md)