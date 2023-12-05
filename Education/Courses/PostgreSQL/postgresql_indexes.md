# Индексы
#postgresql #index

### SELECT amname FROM pg_am;
- B-tree (Сбалансированное дерево) - по умолчанию
- Хеш-индекс
- GIST (обобщенное дерево поиска)
- GIN (обобщенный обратный)
- SP-GIST (GIST с двоичным разбиением пространства)
- BRIN (блочно-диапазонный)

#### Balanced Tree
- Создается по умолчанию (`CREATE INDEX index_name ON table_name (column_name)`)
- Поддерживает операции:
	`<,>,<=,>=,=`
-  Поддерживает LIKE `abc%` (но не `%abc`)
- Индексирует `NULL`
- Сложность поиска `O(logN)`

#### Hash index
- `CREATE INDEX index_name ON table_name USING HASH (column_name);`
- Поддерживает только операцию '='
- Не отражается в журнале предзаписи (`WAL`)
- Сложность поиска `O(1)`

### Специализированные индексы

#### GIST (обобщенное дерево поиска)
