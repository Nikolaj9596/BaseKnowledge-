---
id: 1731013233-test-grpcserver
aliases:
  - Test GrpcServer
  - Test GrpsServer
tags:
  - python
  - grpc
  - test
---
# Test GrpsServer

| Тестирование gRPC запросов может быть организовано с использованием библиотеки grpcio-testing, 
| которая позволяет создавать фикстуры для имитации gRPC-сервера, каналов, а также для обработки gRPC-запросов в тестах.
| Ниже приведены полезные фикстуры для тестирования gRPC-клиентов и серверов, написанных на Python.

1. Установка зависимостей
Если у вас еще нет необходимых библиотек, установите их:
```bash
pip install grpcio grpcio-testing
```
2. Полезные фикстуры для тестирования gRPC
Предположим, что у вас есть сгенерированные файлы для gRPC, такие как service_pb2 и service_pb2_grpc.
Эти файлы содержат определения gRPC-сервисов и сообщений, которые вы используете в вашем приложении.

2.1 Фикстура grpc_server для тестового gRPC-сервера
Эта фикстура создает тестовый gRPC-сервер и регистрирует на нем ваши сервисы.
Это особенно полезно для интеграционных тестов, где вам нужно проверить взаимодействие клиента и сервера.

```python
import pytest
import grpc
from grpc import aio
from concurrent import futures
from service_pb2_grpc import YourServiceServicer, add_YourServiceServicer_to_server

class MockYourService(YourServiceServicer):
    """Реализация Mock-сервиса для тестирования"""
    async def YourMethod(self, request, context):
        # Имитация поведения метода
        return YourResponse(message="Mock response")

@pytest.fixture
async def grpc_server():
    server = aio.server(futures.ThreadPoolExecutor(max_workers=10))
    add_YourServiceServicer_to_server(MockYourService(), server)
    port = server.add_insecure_port("[::]:0")
    await server.start()
    yield server, port
    await server.stop(None)

```
2.2 Фикстура grpc_channel для gRPC-канала к тестовому серверу
Эта фикстура создает канал к серверу, развернутому в grpc_server, чтобы тестировать клиентские запросы.

```python
@pytest.fixture
async def grpc_channel(grpc_server):
    _, port = grpc_server
    async with grpc.aio.insecure_channel(f"localhost:{port}") as channel:
        yield channel
```
2.3 Фикстура grpc_client для создания клиента
Эта фикстура создает клиента для gRPC-сервиса.
Он подключается к каналу, созданному фикстурой grpc_channel, чтобы отправлять запросы на сервер.

```python
from service_pb2_grpc import YourServiceStub

@pytest.fixture
async def grpc_client(grpc_channel):
    return YourServiceStub(grpc_channel)
```
3. Пример теста gRPC-клиента
Теперь, когда все фикстуры готовы, можно написать тест,
который использует эти фикстуры для отправки запросов и проверки ответа:

```python
import pytest
from service_pb2 import YourRequest

@pytest.mark.asyncio
async def test_your_method(grpc_client):
    request = YourRequest(parameter="test")
    response = await grpc_client.YourMethod(request)
    assert response.message == "Mock response"

```
