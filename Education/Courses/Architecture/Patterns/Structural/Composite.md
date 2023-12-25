# Комопновщик
#pattern #architecture #structural 
```plantuml
class Client{}
interface Component{
	+ execute()
}
class Leaf{}
class Composite{
	- children: Component[]
	+ add(Component)
	+ remove(Component)
	+ execute()
}

Client --> Component
Leaf ..|> Component
Composite ..|> Component
Composite o--> Component
```

###### Плюсы
- Упрощает работу со сложным деревом компонентов
- Облегчает добавление новых типов компонентов
###### Минусы
- Общий тип компонентов
###### Пример реализации
```python
```