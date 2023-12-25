
# Адаптер
#pattern #structural #architecture 

```plantuml

class Client{}

interface Target{
	+ method(data)
}

class Adapter{
	- adapter: SpecificService
	+ method(data)
}

class SpecificService{
	+ serviceMethod(specificData)
}

Client --> Target
Adapter ..|> Target
Adapter --> SpecificService
```

###### Плюсы
- Создает "мост" между интерфейсами
- Изолирует от клиента подробности преобразования различных интерфейсов
###### Минусы
- Сложно преобразовать параметры вызовов функций, если в адаптируемом интерфейсе другие абстракции