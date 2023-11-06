### High-level package architecture 

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

folder "user_and_session" {
  [User]
  [Session]
  [PushToken]
}

folder "profile" {
  [Profile]
}

folder "trading_platform_account" {
  [Trading Platform Account]
}

[PushToken] --> [Session]
[Session] --> [User]
[Profile] --> [User]
[Trading Platform Account] --> [User]
```
