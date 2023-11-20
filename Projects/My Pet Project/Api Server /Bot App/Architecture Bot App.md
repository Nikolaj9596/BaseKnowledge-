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

enum Indicator {
  RSI
  Moving_Average
  MACD
  Bollinger_Bands
  Stochastic_Oscillator
  Relative_Volume
  Average_True_Range
  Ichimoku_Cloud
  Fibonacci_Retracement
  Volume_Oscillator
  Williams_Percent_R
  Parabolic_SAR
  Average_Directional_Index
  On_Balance_Volume
  Money_Flow_Index
  Chaikin_Oscillator
  Commodity_Channel_Index
  Rate_of_Change
  Relative_Strength
  Accumulation_Distribution
  Pivot_Points
  Exponential_Moving_Average
  Volume_Weighted_Average_Price
  Fibonacci_Fan
  Williams_Alligator
  Demarker_Indicator
  Trix_Indicator
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


enum TradingFrequency{
  HIGH_FREQUENCY_TRADING
  LOW_FREQUENCY_TRADING
}

class PlatformTradingAccount{
  + id: int
}

class PlatformTrading{
  + id: int
}

class TrdingSession{
  + id: int
  + created: datetime
  + updated: datetime
  + name: str
  + start_time: time
  + end_time: time
}

class Assets{
  + id: int
  + created: datetime
  + updated: datetime
  + name: str
  + min_lot_size: Decimal
  + max_lot_size: Decimal
  + trading_platforms: PlatformTrading
  + sync_on_platform: boolean
  - _value: json
}

class ConditionStratege{
  + id: int
  + created: datetime
  + updated: datetime
  + name: str
  + condition_type: ConditionType
  + hight_timeframe: TimeFrame
  + low_timeframe: TimeFrame
  + description: string
  + indicator: list[Indicator]
  + assets: Assets
  + min_lot_size: Optional[Decimal]
  + max_lot_size: Optional[Decimal]
  + percentage_of_total_portfolio: float
  + use_current_market_price: boolean
  + use_limit_orders: boolean
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

class UserStratege{
  + id: int
  + name: str
  + created: datetime
  + updated: datetime
  + trading_session: TrdingSession
  + condition: ConditionStratege
  + use_existing_stratege: boolean
  + startage: TradingStrategy
  + assets: list[Assets]
  - value: json
}

class BotReport{
  + id: int
  + created: datetime
  + updated: datetime
  + report: Image
  + bot: Bot
}

class Bot {
  + id: integer
  + platform_trading_account: PlatformTradingAccount
  + stratege:UserStratege 
  + status: Status
  + is_active: boolean
  + created: datetime
  + updated: datetime
  + limit_num_of_transactions_per_day: int
  + limit_num_of_unsuccessful_transaciton_per_day: int
}

Bot::paltform_trading_account "1"-->"1...*" PlatformTradingAccount::id
Bot::stratege "1"-->"1..*" UserStratege::id
Bot::status --> Status

BotReport::bot "1..*"-->"1" Bot::id
Assets::trading_platforms"*..*"-->"*..*"PlatformTrading::id

ConditionStratege::condition_type --> ConditionType
ConditionStratege::hight_timeframe --> TimeFrame
ConditionStratege::low_timeframe --> TimeFrame
ConditionStratege::indicator --> Indicator 
ConditionStratege::trading_frequency --> TradingFrequency 

UserStratege::trading_session "*..*"-->TrdingSession::id
UserStratege::assets "*..*"-->Assets::id
UserStratege::condition "*..*"-->"*..*"ConditionStratege::id
UserStratege::stratege --> TradingStrategy


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
