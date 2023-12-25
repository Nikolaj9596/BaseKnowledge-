# Декоратор
#pattern #structural #architecture 

```plantuml
class Client{}
interface Component{
	+ execute()
}

class ConcreteComponent{
	+ execute()
}

class BaseDecorator{
	- wrappee: Component
	+ BaseDecorator(Component)
	+ execute()
}
class ConcreateDecorators{
	+ execute()
	+ extra()
}

Client --> Component
ConcreteComponent ..|> Component
BaseDecorator ..|> Component
BaseDecorator::BaseDecorator o--> Component
ConcreateDecorators --|> BaseDecorator
```
###### Плюсы
- Большая гибкость при сравнении с наследованием
- Декомпозиция функционала
- Позволяет подменять обязанности на лету
###### Минусы
- Сложности при конфигурировании многократно задекорированные объекты
###### Пример реализации
```python
# Декоратор (Decorator, Wrapper) - паттерн, структурирующий объекты.
# Динамически добавляет объекту новые обязанности.
# Является гибкой альтернативой порождению подклассов с целью расширения функциональности.

class Man:
    """Человек"""

    def __init__(self, name):
        self.name = name

    def say(self):
        print(f'Привет! Меня зовут {self.name}!')

class Jetpack:
    """Реактивный ранец"""

    def __init__(self, man: Man):
        self._man = man

    def __getattr__(self, item):
        return getattr(self._man, item)

    def fly(self):
        # расширяем функциональность объекта добавляя возможность летать
        print(f'{self._man.name} летит на реактивном ранце!')

man = Man('Виктор')
man_jetpack = Jetpack(man)
man_jetpack.say()  # Привет! Меня зовут Виктор!
man_jetpack.fly()  # Виктор летит на реактивном ранце!

```