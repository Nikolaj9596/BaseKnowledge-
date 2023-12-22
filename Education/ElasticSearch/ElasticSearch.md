# Примеры работы с ElasticSearch
#elasticsearch
## Добавление документа в индекс
под документом понимаем какую-то единицу данных

Документ записывается в произвольной структуре
`PUT` **my_blogs/_doc/1**
```json
{
	"title": "Fighting Ebola with Elastic",
	"category": "User Stories",
	"author": {
		"first_name": "Emily",
		"last_name": "Mother"
	}
}
```

#### Response
```json
{
	"_index": "my_blogs",
	"_type": "_doc",
	"_id": "1",
	"_version": 1,
	"result": "created",
	"_shards": {
		"total": 2,
		"successful": 1,
		"failed": 0
	},
	"_seq_no": 0,
	"_primary_term": 1
}
```

### Обновление документа

`POST` **my_blogs/_oc/1/_update
```json
{
	"doc": {
		"date": "September 26, 2016"
	}
}
```

### Удаление документа

`DELETE` **my_blogs/_doc/1**

### Поиск документа

`GET` **my_blogs/_search**
```json
{
	"took": 1
	"time_out": false,
	"_shards": {
		"total": 5,
		"successful": 5,
		"skipped": 0,
		"failed": 0
	},
	"hits": {
		"total": 2,
		"max_score": 1.0,
		"hits":[...]
	}
}
```

### Создание индекса
- Если индекс не создан и мы добавляем документ то индекс создается автоматически
- при создании индекса можно указать настройки и маппинг
`PUT` **/my_blogs**
```json
{
	"settings": {...},
	"mappings": {...}
}
```

### Mapping
`PUT` **my_index**
```json
{
	"mappings": {
		"doc":{
			"properties":{
				"title": {"type": "text"},
				"name": {"type": "text"},
				"age": {"type": "integer"},
				"created": {
					"type": 
				}
			}	
		}
	}
}
```