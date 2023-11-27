#### Class Diagram
```plantuml
@startuml

enum Status{
  CREATED
  WATING
  WORK
  STOP
  ERROR
}

enum ConditionType{
  FOR_ENTRY
  FOR_EXIT
}

enum TimeFrame{
  1_MIN
  5_MIN
  15_MIN
  30_MIN
  1_HOUR
  4_HOUR
  1_DAY
  1_WEEK
  1_MONTH
}

enum TradingStrategy {
  Scalping
  DayTrading
  SwingTrading
  PositionTrading
  TrendFollowing
  MeanReversion
  BreakoutTrading
  Arbitrage
  HighFrequencyTrading
  AlgorithmicTrading
  SentimentAnalysis
  QuantitativeModeling
  SmartMoney
}

enum TypeMarket{
  SPOT
  FUTURES
}


enum TradingFrequency{
  HIGH_FREQUENCY_TRADING
  LOW_FREQUENCY_TRADING
}

enum OrderTypeMarker{
  AUTO
  CURRENT_MARKET_PRICE
  LIMIT_ORDERS
}

class PlatformTradingAccount{
  + id: integer
}

class PlatformTrading{
  + id: integer
}

class Client{
  + id: integer
}

class TradingSession{
  + id: integer
  + created: datetime
  + updated: datetime
  + name: str
  + start_time: time
  + end_time: time
  + all_time: boolean
  + is_global: boolean
  + creator: Client
  + priority: integer
}

class Assets{
  + id: int
  + created: datetime
  + updated: datetime
  + name: str
  + min_lot_quantity: float 
  + min_lot_price: float
  + max_lot_quantity: float
  + max_lot_price: float
  + trading_platforms: PlatformTrading
  + sync_on_platform: datetime 
  + type_market: TypeMarket
  + icon: Image
  - _value: json
}

class TradingSignalsIndicator{
  + id: int
  + created: datetime
  + updated: datetime
  + name: str
}

class ConditionStrategy{
  + id: int
  + created: datetime
  + updated: datetime
  + name: str
  + description: string
  + is_show_all: boolean
  + creator: Optional[Client]
  + condition_type: ConditionType
  + hight_timeframe: TimeFrame
  + low_timeframe: TimeFrame
  + indicator: list[TradingSignalsIndicator]
  + assets: Assets
  + min_lot_size: Optional[float]
  + max_lot_size: Optional[float]
  + percentage_of_total_portfolio: float
  + order_type_marker: OrderTypeMarker
  + trading_frequency: TradingFrequency
  ....
  Risk management in %
  ....
  + stop_loss: float
  + take_profit: float
  ....
  The value field can store additional, 
  more specific settings for certain assets
  ....
  - value: json
}

class UserStrategy{
  + id: int
  + name: str
  + created: datetime
  + updated: datetime
  + condition: ConditionStrategy
  + creator: Client
  ....
  + use_existing_stratege: boolean
  + trading_session: TradingSession
  + strategy: TradingStrategy
  - value: json
}

class BotReport{
  + id: integer
  + created: datetime
  + updated: datetime
  + report: Image
  + bot: Bot
}

class Bot {
  + id: integer
  + platform_trading_account: PlatformTradingAccount
  + stratege: UserStrategy 
  + status: Status
  + is_active: boolean
  + created: datetime
  + updated: datetime
  + limit_num_of_transactions_per_day: int
  + limit_num_of_unsuccessful_transaction_per_day: int
}

Bot::paltform_trading_account "1"-[#green]->"1...*" PlatformTradingAccount::id
Bot::stratege "1"-[#green]->"1..*" UserStrategy::id
Bot::status -[#green]-> Status
Bot::owner "1..*"-[#green]->"*..1" Client 

BotReport::bot "1..*"-[#black]->"*..1" Bot::id
TradingSession::creator "1..*"-[#gray]->"*..1" Client::id
Assets::trading_platform "1"-[#pink]->"1..*"PlatformTrading::id
Assets::type_market-[#pink]->TypeMarket

ConditionStrategy::condition_type -[#red]-> ConditionType
ConditionStrategy::hight_timeframe -[#red]-> TimeFrame
ConditionStrategy::low_timeframe -[#red]-> TimeFrame
ConditionStrategy::indicator "*..*"-[#red]->"*..*" TradingSignalsIndicator 
ConditionStrategy::trading_frequency -[#red]-> TradingFrequency 
ConditionStrategy::order_type_marker -[#red]-> OrderTypeMarker
ConditionStrategy::creator "1..*" -[#red]-> "*..1"Client

UserStrategy::trading_session "*..*"-[#yellow]->TradingSession::id
UserStrategy::condition "*..*"-[#yellow]->"*..*"ConditionStrategy::id
UserStrategy::stratege -[#yellow]-> TradingStrategy
UserStrategy::creator "1..**"-[#yellow]->"**..1" Client::id

@enduml
```

#### Business logic 

1. Основная сущность которая видна пользователю и с которой пользователь взамодействует
2. Пользователь может создавать определенное кол-во ботов
3. К боту призвязывается акаунт платформы пользователя 
4. Бот может быть только определенного типа при создании нужно проверить правильного ли типа бот
5. К боту привязывается стратеги согласно которой он будет торговать
6. Добавить уведомления пользователю при создании бота
7. Пользователь может сменить акаунт и стратегию у бота
