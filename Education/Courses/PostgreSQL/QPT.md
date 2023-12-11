# Course by prostgresql from Postgres Professional
#course #postgresql 

####  Учебный курс QPT. Тема 3: Последовательный доступ
[YouTube Video](https://www.youtube.com/watch?v=BNHLk2mHrsA&list=PLaFqU3KCWw6K2sTAksX5AJq4SQDN5PA1t&index=5)
##### Последовательное чтение всеж страниц 
страницы читаются в кеш (исползуется буферное кольцо)
проверяется видимость версии строк
данные возвращаются в произвольном порядке
время сканирования зависит от физического размера файла

##### Ведущий процесс
Выполняет последовательную часть плана
запускает рабочие процессы и получает от них данные

##### Рабочие процессы
одновременно работабют над параллельной частью плана

##### Дефолтные настройки
max_worker_processes = 8
max_parallel_workers = 8
max_parallel_workers_per_gather = 2

###### Равно нулю (параллельный план не строится)
если размер таблицы < min_parallel_table_scan_size = 8MB
###### Фикировано
Если для таблицы указан параметр хранения parallel_workers
###### Вычисляется по формуле
1 +|log3(размер таблицы min_parallel_table_scan_size) | но не больше, чем max_parallel_workers_per_gather

### Explain

```sql
=> EXPLAIN SELECT * FROM flights;
				QUERY PLAN
-------------------------------------------
Seq Scan on flights (cost=0.00..4564.67 rows=214867 width=63)
```
Выбран метод доступа Seq Scan - последовательное чтение.
В скобкаж приведен важные значения
* cost - оценка стоимости;
* rows - оценка числа строк, возвращаемых операций;
* width - оценка размера одной записи в байтах
Стоимость указывается в некоторых условных единицах.

Приводятся два числа. Первое показывает оценку ресурсов на предварительную подготовку к операции. Для последовательного сканирования этот ноль - чтобы начать возвращать данные, никакой подготовки не требуется.

Второе число показыает общую оценку ресурсов для получения всех данных. Как оно получается ?

Оптимизатор PostgresSQL учитывает две компоненты, дисковый ввод-вывод и ресурсы процессора. Компонента ввода-вывода рассчитывается как просзведение числа сраниц в таблице на условную стоимост чтения одной страницы:

```sql
=> SELECT relpages, current_stting('seq_page_cost'), relpages * current_setting('seq_page_cost')::real AS tatal FROM pg_class WHERE relnam='flights';

relpages|current_setting|total
2416    |1              | 2416
```

 Вторая компонента - русурсы процуссора - складывается из стоимости обработки каждой строки:
```sql
SELECT reltuples, current_setting('cpu_tuple_cost'),
reltuples * current_setting('cpu_tuple_cost')::real AS total
FROM pg_class WHERE relname='flights';

relpages|current_setting|total
214867  |0.01           | 2148.67
```

###### Агрегация
```sql
=> EXPLAIN SELECT count(*) FROM seats;
											QUERY PLAN
---------------------------------------------------------------------------------
Aggregate (cost=24.75...24.76 rows=1 width=8)
 -> Seq Scan on seats (cost=0.00..21.39 rows=1339 width=0)
```

План состоит из двух узлов. Верхний - Aggregate, в котором происходит вычисление count, - получает данные то нижнего - Seq Scan.

Обратите внимание на стоимость узла Aggregate: нижняя цифра практически
равна верхней. Это означает, что узел не может выдать результат, пока не обработает все данные (что вполне логично)

Разницу между оценкой для Aggregate и верхней оценкой для Seq Scan - стоимость работы собственно узла Aggregate. Она вычисляется исходя из оценки ресурсов на выполнение условной операции:
```sql
=> SELECT reltuples, current_setting('cpu_operator_cost'),
reltuples * current_setting('cpu_operator_cost')::real AS tatal
FROM pg_class WHERE relname='seats':

relpages|current_setting|total
1339    |0.0025         | 3.3475
```

### Индексное сканирование

Index Scan - Индексное сканирование
Parallel Index Scan - Паралельное индексное сканирование
Parallel Index Only Scan - Сканирование только индекса и карты видимости

Проверим план запроса с поиском одного значения:

```sql
=> EXPLAIN SELECT * FROM bookings WHERE book_ref = 'CDE08D';
								QUERY PLAN
--------------------------------------------------------
Index Scan using bookings pkey on bookings (cost=0.43..8.45 rows=1 width=21)
	Index Cond: (book_ref = 'CDE08B'::bpchar) //Условие при котором мы осуществляем поиск по индексу
```

Выбран метод доступа Index Scan, указано и