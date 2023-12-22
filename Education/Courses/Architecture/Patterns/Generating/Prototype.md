### Прототип

```plantuml
interface Prototype{
	+ clone()
}
class ConcretePrototype{
	+ clone()
}
class Client{}

Client --> Prototype
ConcretePrototype ..|> Prototype
```

###### Плюсы
- Ускоряет создание объектов
- Общий интерфейс клонирования объектов
- Меньше повторяющегося кода инициализации объектов
###### Минусы
- Сложности при клонировании объектов, имеющих ссылки на другие

### 