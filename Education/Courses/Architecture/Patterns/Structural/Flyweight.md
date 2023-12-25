# Легковес/Приспособленец
#pattern #structural #architecture 

```plantuml
class Client{}
class Item{
	- uniqueState
	- flyweight
	+ someOperation()
}
class FlyweightFactory{
	- cache: Flyweight[]
	+ getFlyweight(key)
}

class Flyweight{
	 - repeatingState
	 - someOperation(uniqueState)
}

Client *--> Item
Item --> FlyweightFactory
Item --> Flyweight
FlyweightFactory o--> Flyweight
```
###### Плюсы
- Экономия оперативной памяти
###### Минусы
- Расход процессорного времени(на поиск в кэше/словаре)
- Уменьшение связности
###### Пример реализации
```plantuml

```