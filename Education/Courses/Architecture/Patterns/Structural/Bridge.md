
# Мост

#architecture #structural #pattern 

```plantuml
class Client{}

class Abstraction{
	- impl: Implementation
	+ method(data)
}

interface Implementation{
	+ method1()
	+ method2()
	+ method3()
}

interface ConcreteImplementation{}

Client --> Abstraction
Abstraction o--> Implementation
ConcreteImplementation ..|> Implementation
```

###### Плюсы
- Реализует принцип открытости/закрытости
- Улучшает масштабируемость

###### Минусы
- Усложняет структуру и читаемость кода из-за введения дополнительных классов
```python
# Мост (Bridge) - паттерн, структурирующий объекты.
# Основная задача - отделить абстракцию от её реализации так,
# чтобы то и другое можно было изменять независимо.
from abc import ABC, abstractmethod
class ITVBase(ABC):
    """Абстрактный телевизор"""

    @abstractmethod
    def tune_channel(self, channel):
        raise NotImplementedError()

class SonyTV(ITVBase):
    """Телевизор Sony"""
    def tune_channel(self, channel):
        print(f'Sony TV: выбран {channel} канал')

class SharpTV(ITVBase):
    """Телевизор Sharp"""
    def tune_channel(self, channel):
        print(f'Sharp TV: выбран {channel} канал')

class IRemoteControlBase(ABC):
    """Абстрактный пульт управления"""
    def __init__(self):
        self._tv = self.get_tv()

    @abstractmethod
    def get_tv(self, tv: ITVBase):
        raise NotImplementedError()

    @abstractmethod
    def tune_channel(self, channel):
        self._tv.tune_channel(channel)

class RemoteControl(IRemoteControlBase):
    """Пульт управления"""
    def __init__(self):
        super(RemoteControl, self).__init__()
        self._channel = 0  # текущий канал

    def get_tv(self, tv: type[ITVBase] = SharpTV):
        return tv

    def tune_channel(self, channel):
        super(RemoteControl, self).tune_channel(channel)
        self._channel = channel

    def next_channel(self):
        self._channel += 1
        self.tune_channel(self._channel)

    def previous_channel(self):
        self._channel -= 1
        self.tune_channel(self._channel)

remote_control = RemoteControl()
remote_control.tune_channel(5)  # Sharp TV: выбран 5 канал
remote_control.next_channel()  # Sharp TV: выбран 6 канал
```
