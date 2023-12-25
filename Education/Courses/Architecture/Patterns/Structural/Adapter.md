
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
###### Пример реализации
```python
# Адаптер - паттерн, структурирующий классы и объекты.

# Преобразует интерфейс одного класса в интерфейс другого, который ожидают клиенты.
# Адаптер обеспечивает совместную работу классов с несовместимыми интерфейсами, которая без него была бы невозможна.


class Dog:
    def __init__(self, name):
        self._name = name

    def bark(self):
        return f'{self._name}: гав-гав!'


class Cat:
    def __init__(self, name):
        self._name = name

    def meow(self):
        return f'{self._name}: мяу-мяу!'


class CatAdapter(Dog):
    # благодаря адаптеру мы можем использовать
    # интерфейс класса `Dog`, а реализацию класса `Cat`.

    def __init__(self, name):
        super(CatAdapter, self).__init__(name=name)
        self._cat = Cat(name=name)

    def bark(self):
        # запрос `bark` преобразуется в запрос `meow`
        return self._cat.meow()

dog = Dog('Тузик')
print(dog.bark())  # Тузик: гав-гав!

dog = CatAdapter('Тузик')
print(dog.bark())  # Тузик: мяу-мяу!
```