# Фасад
#pattern #structural #architecture 
```plantuml
class Client{}
class Facade{
	+ someOperation()
}

class SomeSubsystem1{}
class SomeSubsystem2{}

Client --> Facade
Facade ..> SomeSubsystem1
Facade ..> SomeSubsystem2
```

###### Плюсы
- Изоляция сложности
###### Минусы
- Фасад может сильно разрастись
###### Пример реализации
```python
# Фасад (Facade) - паттерн, структурирующий объекты.
# Предоставляет унифицированный интерфейс вместо набора интерфейсов некоторой подсистемы.
# Фасад определяет интерфейс более высокого уровня, который упрощает использование подсистемы.

class Paper:
    """Бумага"""

    def __init__(self, count: int):
        self._count = count

    def get_count(self) -> int:
        return self._count

    def draw(self, text) -> None:
        if self._count > 0:
            self._count -= 1
            print(text)

class Printer:
    """Принтер"""

    def error(self, msg: str):
        print(f'Ошибка: {msg}')

    def print_(self, paper: Paper, text: str):
        if paper.get_count() > 0:
            paper.draw(text)
        else:
            self.error('Бумага закончилась')

class Facade:
    def __init__(self, printer: Printer, paper: Paper):
        self._printer = printer
        self._paper = paper

    def write(self, text):
        self._printer.print_(self._paper, text)

f = Facade(printer=Printer(), paper=Paper(count=1))
f.write('Hello world!')  # Hello world!
f.write('Hello world!')  # Ошибка: Бумага закончилась
```
