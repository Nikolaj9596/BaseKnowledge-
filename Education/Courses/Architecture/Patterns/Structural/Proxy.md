# Заместитель
#pattern #structural #architecture 
```plantuml
class Client{}

interface IService{
	+ operation()
}
class Proxy{
	- service: Service
	+ operation()
}
class Service{
	+ operation()
}

Client --> IService
Proxy ..|> IService
Service ..|> IService
Proxy o--> Service
```

###### Плюсы
- Контроль над проксируемым сервисом, незаметный для клиента
###### Минусы
- Может увеличивать время отклика сервиса
###### Пример реализации
```python
# Заместитель (Proxy, Surrogate) - паттерн, структурирующий объекты.
# Является суррогатом другого объекта и контролирует доступ к нему.
from abc import ABC, abstractmethod
from functools import partial

class ImageBase(ABC):
    """Абстрактное изображение"""
    @classmethod
    def create(cls, width: int, height: int):
        """Создает изображение"""
        return cls(width, height)

    @abstractmethod
    def draw(self, x: float, y: float, color: str):
        """Рисует точку заданным цветом"""
        raise NotImplementedError()

    @abstractmethod
    def fill(self, color: str):
        """Заливка цветом"""
        raise NotImplementedError()

    @abstractmethod
    def save(self, filename: str):
        """Сохраняет изображение в файл"""
        raise NotImplementedError()

class Image(ImageBase):
    """Изображение"""
    def __init__(self, width: int, height: int):
        self._width = int(width)
        self._height = int(height)

    def draw(self, x: float, y: float, color: str) -> None:
        print(f'Рисуем точку; координаты: ({x}, {y}); цвет: {color}')

    def fill(self, color: str) -> None:
        print(f'Заливка цветом {color}')

    def save(self, filename: str):
        print(f'Сохраняем изображение в файл {filename}')

class ImageProxy(ImageBase):
    """
    Заместитель изображения.
    Откладывает выполнение операций над изображением до момента его сохранения.
    """
    def __init__(self, *args, **kwargs):
        self._image = Image(*args, **kwargs)
        self.operations = []

    def draw(self, *args):
        func = partial(self._image.draw, *args)
        self.operations.append(func)

    def fill(self, *args):
        func = partial(self._image.fill, *args)
        self.operations.append(func)

    def save(self, filename):
        # выполняем все операции над изображением
        map(lambda f: f(), self.operations)
        # сохраняем изображение
        self._image.save(filename)

img = ImageProxy(200, 200)
img.fill('gray')
img.draw(0, 0, 'green')
img.draw(0, 1, 'green')
img.draw(1, 0, 'green')
img.draw(1, 1, 'green')
img.save('image.png')

# Заливка цветом gray
# Рисуем точку; координаты: (0, 0); цвет: green
# Рисуем точку; координаты: (0, 1); цвет: green
# Рисуем точку; координаты: (1, 0); цвет: green
# Рисуем точку; координаты: (1, 1); цвет: green
# Сохраняем изображение в файл image.png
```