## Publisher

- App-side - отправляет сообщения
- Connection, channel - термины подключения(1 соединение - 1 каннал)
- Декларированин - что можно создать (деларирует Consumer)
- Всегда пишет в Exchange
- Определяет delivery_mod  (по умолчанию 1 при этом занчении не пишется на диск если передать 0 то сообщение будет сохраняться на диск)
- Confirm,mandatory flags - особый флаги отправки ( требует подтверждения)

## Exchange

- Сущность RabbitMQ
- Точка входа для роутинга сообщений
- Durable/Transient - если Durable после перезагрузки сохраняютня настройки и биндинги
- Нет хранения 
## Binding
- Сущность RabbitMQ
-  Средство привязки очередей к эксчейнджам
- Не обязательно один
- Routing-key/акгументы

## Exchange - типы
- Fanout - публикует во все очереди (быстрый роутинг) - 30k mps
- Direct - статический роутинг  (быстрый роутинг) - routing key - 30k mps
- Topic - роутинг с вилдкардами (медленный) - 5k mps
- Headers - роутинг по заголовкам (самый медленный) - 1k mps
## Queue
- Сущность RabbitMQ
- Хранение сообщений - от настроек publlisher
- Durable - устоичива к перезагрузкам сервера
- Типы очередей
#note Даже если сообщение передали с delivery_mod 2 но очередь была не Durable при перезагрузки сообщения будут утерены

### Message
- Сущность RabbitMQ
- Delivery mode - хранить на диске, устойчива к перезагрузкам сервера
- Основные поля
	- Payload (Рекомеднуемый размер одного сообщения не больше 128MB)
	- Routing Key 
	- Headers
### Consumer
- App-side - получает сообщения
- Connection, channel - термины подключения(1 соединение - 1 каннал)
- Декларированин - что можно создать (деларирует Consumer)
- Consuming - "подписывание" на очередь
- Prefetch cout - коичество неподтвержденных сообщений в момент
- ACK, Negative ACK - подтвеждение отказ от сообщения
- Auto Ack - автоматическое подтверждение

### Docker compose

```yaml
rabbitmq:
	image: rabbitmq:3.10.7-management
	hostname: rabbitmq
	restart: always
	enviroment:
		- RABBITMQ_DEFAULT_USER=rmuser
		- RABBITMQ_DEFAULT_PASS=rpassword
		- RABBITMQ_SERVER_ADDITIONAL_ERL_ARGS=-rabbit disk_free_limit 2147483648
	ports:
		- 15673:15673
	 volumes:
		 - ./rabbitmq:/var/lib/rabbitmq
```

