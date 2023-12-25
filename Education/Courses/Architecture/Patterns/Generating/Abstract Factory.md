# Абстрактная фабрика
#pattern #generating #architecture 

```plantuml
class Client{
	+ factory: AbstractFactory
}

class ConcreteFactory1{
	+ createProductA(): ProductA
	+ createProductB(): ProductB
}
class ConcreteFactory2{
	+ createProductA(): ProductA
	+ createProductB(): ProductB
}

interface AbstractFactory{
	+ createProductA(): ProductA
	+ createProductB(): ProductB
}

class ConcreteProductA1{}
class ConcreteProductA2{}
interface AbstractProductA{}

class ConcreteProductB1{}
class ConcreteProductB2{}
interface AbstractProductB{}

Client --> AbstractFactory
ConcreteFactory1 ..|> AbstractFactory
ConcreteFactory2 ..|> AbstractFactory

ConcreteFactory1 ..> ConcreteProductA1
ConcreteFactory1 ..> ConcreteProductB1
ConcreteFactory2 ..> ConcreteProductA2
ConcreteFactory2 ..> ConcreteProductB2

ConcreteProductA1 ..|> AbstractProductA
ConcreteProductA2 ..|> AbstractProductA
ConcreteProductB1 ..|> AbstractProductB
ConcreteProductB2 ..|> AbstractProductB
```

###### Плюсы
- Гарантирует сочетаемость создаваемых продуктов
- Выделяет код производства продуктов в одно место, упрощая поддержку кода
- Упрощает добавление новых продуктов в программу
###### Минусы
- Требует наличия всех типов продуктов в каждой реализации