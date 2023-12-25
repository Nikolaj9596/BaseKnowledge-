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
```python
# Приспособленец (Flyweight) - паттерн, структурирующий объекты.
# Использует разделение для эффективной поддержки множества мелких объектов.
import weakref

class Color:
    """Приспособленец"""

    def __init__(self, name: str):
        self.name = name

    def __str__(self):
        return self.name

class ColorFactory:
    """Фабрика приспособленцев"""

    _colors = weakref.WeakValueDictionary()

    @classmethod
    def get_color(cls, name: str):
        value = cls._colors.get(name)
        if value is None:
            value = Color(name)
            cls._colors[name] = value
        return value

class PlaceMark:
    """Метка на карте"""

    def __init__(self, latitude: float, longitude: float, color_name: str):
        # координаты - внутреннее состояние (т.к. они уникальны для каждой метки)
        self._latitude = latitude
        self._longitude = longitude
        # цвет - внешнее состояние которое может быть общим у разных меток
        self._color = ColorFactory.get_color(color_name)

    def __str__(self) -> str:
        return f'Цвет: {self._color}; Координаты: ({self._latitude}, {self._longitude})'

plmrk0 = PlaceMark(-74.007121, 40.714551, 'green')  # Нью-Йорк
plmrk1 = PlaceMark(37.617761, 55.755773, 'green')  # Москва

print(plmrk0)  # Цвет: green; Координаты: (-74.0071, 40.7146)
print(plmrk1)  # Цвет: green; Координаты: (37.6178, 55.7558)
print(plmrk0._color is plmrk1._color)  # True

```