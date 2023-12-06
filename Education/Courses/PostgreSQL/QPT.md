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

