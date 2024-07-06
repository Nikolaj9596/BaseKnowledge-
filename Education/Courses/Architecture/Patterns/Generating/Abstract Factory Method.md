# Фабричный метод
#pattern #generating #architecture 

```plantuml
class Creator{
	+ someFunc()
	+ createProduct(): Product
}

class ConcreteCretor1{
	+ createProduct(): Product
}

class ConcreteCretor2{
	+ createProduct(): Product
}

class ConcreteProduct2{
}

class ConcreteProduct1{
}

interface Product{
	+ doSmth()
}

Creator ..> Product
ConcreteCretor1 --|> Creator
ConcreteCretor2 --|> Creator
ConcreteCretor1 ..> ConcreteProduct1: create
ConcreteCretor2 ..> ConcreteProduct2: create
ConcreteProduct1 ..> Product
ConcreteProduct2 ..> Product
```

### Пример реализации на Python
```python
# Фабричный метод (Factory Method) - паттерн, порождающий классы.

# Определяет интерфейс для создания объекта, но оставляет подклассам решение о том, какой класс инстанцировать.
# Позволяет делегировать инстанцирование подклассам.

# Абстрактная фабрика часто реализуется с помощью фабричных методов.
# Фабричные методы часто вызываются внутри шаблонных методов.


class Document(ABC):
	@abstractmethod
    def show(self):
        raise NotImplementedError()


class ODFDocument(Document):
    def show(self):
        print 'Open document format'


class MSOfficeDocument(Document):
    def show(self):
        print 'MS Office document format'


class Application(ABC):
	@abstractmethod
    def create_document(self, type_):
        # параметризованный фабричный метод `create_document`
        raise NotImplementedError()


class MyApplication(Application):
    def create_document(self, type_):
        if type_ == 'odf':
            return ODFDocument()
        elif type_ == 'doc':
            return MSOfficeDocument()
        else:
            return Document()


app = MyApplication()
app.create_document('odf').show()  # Open document format
app.create_document('doc').show()  # MS Office document format
try:
    app.create_document('pdf').show()
except:
    print("NotImplementedError")
```
