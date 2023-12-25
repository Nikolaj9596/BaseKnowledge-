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
# Компоновщик (Composite) - паттерн, структурирующий объекты.
# Компонует объекты в древовидные структуры для представления иерархий часть-целое.
# Позволяет клиентам единообразно трактовать индивидуальные и составные объекты.


# Класс представляющий одновременно примитивы и контейнеры
from abc import ABC, abstractmethod


class IComposite(ABC):
    @abstractmethod
    def draw(self):
        raise NotImplementedError()

    @abstractmethod
    def add(self, obj):
        raise NotImplementedError()

    @abstractmethod
    def remove(self, obj):
        raise NotImplementedError()

    @abstractmethod
    def get_child(self, index):
        raise NotImplementedError()

class IComponent(ABC):
    @abstractmethod
    def draw(self):
        raise NotImplementedError()


class Line(IComponent):
    def draw(self):
        print('Линия')


class Rectangle(IComponent):
    def draw(self):
        print('Прямоугольник')


class Text(IComponent):
    def draw(self):
        print('Текст')


class Picture(IComposite):
    def __init__(self):
        self._children = []

    def draw(self):
        print('Изображение')
        # вызываем отрисовку у вложенных объектов
        for obj in self._children:
            obj.draw()

    def add(self, obj):
        if isinstance(obj, IComposite) and obj not in self._children:
            self._children.append(obj)

    def remove(self, obj):
        index = self._children.index(obj)
        del self._children[index]

    def get_child(self, index):
        return self._children[index]


pic = Picture()
pic.add(Line())
pic.add(Rectangle())
pic.add(Text())
pic.draw()
# Изображение
# Линия
# Прямоугольник
# Текст

line = pic.get_child(0)
line.draw()  # Линия
```