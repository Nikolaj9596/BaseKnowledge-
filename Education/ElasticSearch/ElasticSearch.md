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
					"type": "date",
					"format": "strict_date_optional_time||epoch_millis"
				}
			}	
		}
	}
}
```

### Search Query

`GET` **my_blogs/_search**
```json
{
	"query": {
		"match": {
			"FIELD": "TEXT"
		}
	}
}
```

#### Response Search
```json
"hits": {
	"total": 241,
	"max_score": 8.810195,
	"hits": [
		{
			"_index": "blogs",
			"_type": "_doc",
			"_id": "oi3uowfolueoiwjfeoiwo",
			"_score": 8.810195,
			"_source": {
				"title": "Ingest Node: A Client's Perspecdtive"
			}
		},
		{
			"_index": "blogs",
			"_type": "_doc",
			"_id": "jdflfsjdlksflkdfjlks",
			"_score": 8.310195,
			"_source": {
				"title": "Elasticsearch for Apache Hadoop"
			}
		}
	]
}
```

По умолчанию возвращается максимум 10 объектов
- **total** - Общее кол-во найденых объектов
- **_score** - Релевантность, чем выше это значение тем выше объект будет в поиске

#### Search by 2 words
`GET` **blogs/_search**
```json
{
	"query": {
		"match": {
			"content": {
				"query": "ingest nodes",
				"operator": "and"
			}	
		}
	}
}
```

#### Search with indication minimum_should_match
`GET` **blogs/_search**
```json
{
	"query": {
		"match": {
			"content": {
				"query": "ingest nodes logstash",
				"minimum_should_match": 2
			}	
		}
	}
}
```

### Search by phrase