# Строитель
#pattern  #generating #architecture 
```plantuml
interface Builder{
	+ reset()
	+ buildPart1()
	+ buildPart2()
	+ getResult()
}

class Client{
 - builder: Builder
}

class ConcreteBuilder1{
	+ reset()
	+ buildPart1()
	+ buildPart2()
	+ getResult(): Proudct1
}

class ConcreteBuilder2{
	+ reset()
	+ buildPart1()
	+ buildPart2()
	+ getResult(): Proudct2
}

class Product1{}
class Product2{}

Client::build --> Builder
ConcreteBuilder1 ..|> Builder
ConcreteBuilder2 ..|> Builder
ConcreteBuilder1 --> Product1
ConcreteBuilder1 --> Product2
```

###### Плюсы
- Позволяет использовать один и тот же код для создания различных продуктов
- Изолирует сложный код сборки продукта от его основной бизнес логики
- Позволяет создавать продукты итерактивно
###### Минусы
- Builder жустко связан со структурой продукта, что усложняет изменение структуры продукта

### Пример реализации

```python
# Строитель (Builder) - паттерн, порождающий объекты.
# Отделяет конструирование сложного объекта от его представления,
# так что в результате одного и того же процесса конструирования могут получаться разные представления.
# От абстрактной фабрики отличается тем, что делает акцент на пошаговом конструировании объекта.
# Строитель возвращает объект на последнем шаге, тогда как абстрактная фабрика возвращает объект немедленно.
# Строитель часто используется для создания паттерна компоновщик.


class Builder(ABC):
	@abstractmethod
    def build_body(self):
        raise NotImplementedError()

	@abstractmethod
    def build_lamp(self):
        raise NotImplementedError()

	@abstractmethod
    def build_battery(self):
        raise NotImplementedError()

	@abstractmethod
    def create_flashlight(self):
        raise NotImplementedError()

class Flashlight:
    """Карманный фонарик"""
    def __init__(self, body, lamp, battery):
        self._shine = False  # излучать свет
        self._body = body
        self._lamp = lamp
        self._battery = battery

    def on(self):
        self._shine = True

    def off(self):
        self._shine = False

    def __str__(self):
        shine = 'on' if self._shine else 'off'
        return 'Flashlight [%s]' % shine

class Lamp:
    """Лампочка"""
	pass

class Body:
    """Корпус"""
	pass

class Battery:
    """Батарея"""
	pass

class FlashlightBuilder(Builder):
    def build_body(self):
        return Body()

    def build_battery(self):
        return Battery()

    def build_lamp(self):
        return Lamp()

    def create_flashlight(self):
        body = self.build_body()
        lamp = self.build_lamp()
        battery = self.build_battery()
        return Flashlight(body, lamp, battery)

builder = FlashlightBuilder()
flashlight = builder.create_flashlight()
flashlight.on()
print(flashlight)  # Flashlight [on]
```