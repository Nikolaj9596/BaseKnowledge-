# Мост
#architecture #structural #pattern 

```plantuml

class Client{}

class Abstraction{
	- impl: Implementation
	+ method(data)
}

interface Implementation{
	+ method1()
	+ method2()
	+ method3()
}

interface ConcreteImplementation{}

Client --> Abstraction
Abstraction o--> Implementation
ConcreteImplementation ..|> Implementation
```

###### Плюсы
- Реализует принцип открфтости/закрытости
- Улучшает масштабируемость
###### Минусы
- Усложняет структуру и читаемость кода из-за введения дополнительных классов