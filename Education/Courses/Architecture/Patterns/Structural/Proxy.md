# Заместитель
#pattern #structural #architecture 
```plantuml
class Client{}

interface IService{
	+ operation()
}
class Proxy{
	- service: Service
	+ operation()
}
class Service{
	+ operation()
}

Client --> IService
Proxy ..|> IService
Service ..|> IService
Proxy o--> Service
```

###### Плюсы
- Контроль над проксируемым сервисом, незаметный для клиента
###### Минусы
- Может увеличивать время отклика сервиса
###### Пример реализации
```python

```