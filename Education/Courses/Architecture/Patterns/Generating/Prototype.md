# Прототип
#pattern #generating #architecture 

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

### Пример реализации
```python
# Прототип - паттерн, порождающий объекты.

# Задает виды создаваемых объектов с помощью экземпляра-прототипа
# и создает новые объекты путем копирования этого прототипа.

import copy

class Prototype:
    def __init__(self):
        self._objects = {}

    def register(self, name, obj):
        self._objects[name] = obj

    def unregister(self, name):
        del self._objects[name]

    def clone(self, name, attrs):
        obj = copy.deepcopy(self._objects[name])
        obj.__dict__.update(attrs)
        return obj


class Bird:
    """Птица"""
	pass


prototype = Prototype()
prototype.register('bird', Bird())

owl = prototype.clone('bird', {'name': 'Owl'})
print(type(owl), owl.name)  # <class '__main__.Bird'> Owl

duck = prototype.clone('bird', {'name': 'Duck'})
print(type(duck), duck.name)  # <class '__main__.Bird'> Duck
```