#### Class Diagram
```plantuml
@startuml
enum Status{
  WATING
  PROCESSING
  WORK
  STOP
  ERROR
}

class ProfileTradingAccount{
  + id: int
}

class Stratege{
  + id: int
}

class Statistic{
  + id: int
}

class Reports{
  + id: int
}

class Bot {
  + id: int
  + profile_trading_account: ProfileTradingAccount
  + stratege: Stratege
  + status: Status
  + is_active: bool
  + created: datetime
  + updated: datetime
  + statistic: Statistic
  + reports: Reports
}

Bot::profile_trading_account "1"->"1" ProfileTradingAccount::id
Bot::stratege -> Stratege::id
Bot::status -> Status
Bot::statistic "1"->"1..*" Statistic::id
Bot::reports "1"->"1..*" Reports::id
@enduml

```

#### Business logic 

1. Основная сущность которая видна пользователю и с которой пользователь взамодействует
2. Пользователь может создавать определенное кол-во ботов
3. К боту призвязывается акаунт платформы пользователя 
4. Бот может быть только определенного типа при создании нужно проверить правильного ли типа бот
5. К боту привязывается стратеги соглано которой он будет торговать
6. Добавить уведомления пользователю при создании бота
7. Пользователь может сменить акаунт и стратегию у бота
