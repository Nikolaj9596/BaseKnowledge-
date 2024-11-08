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
`GET` **blogs/_search**
```json
{
	"query": {
		"match_phrase": {
			"content": {
				"query": "open data",
				"slop": 1
			}	
		}
	}
}
```
```bash
GET blogs/_search?q=content:"open data"~1
```
- Поиск по фразе
- **slop** - Этот параметр указывает что между словами в фразе может стоять одно слово
### Search by range
`GET` **blogs/_search**
```json
{
	"query": {
		"range": {
			"publish_date": {
				"gte": "2017-12-01",
				"lt": "2018-01-01"
			}	
		}
	}
}
```

### The bool query

- **must** - The query **must** appear in matching documents and will contribute to the _score
-  **must_not** - The query **must not** appear in matching documents
- **should** - The query should optionally appear in matching documents, and matches contribute to the _score
- **filter** - The query **must** appear in matching documents, but it will have no effect on the _score

###### Syntax
`GET` **my_index/_search**
```json
{
	"query": {
		"bool": {
			"must": [{}],
			"must_not": [{}],
			"filter": [{}],
			"should": [{}],
		}
	}
}
```

#### The must_not Clause
`GET` **my_index/_search**
```json
{
	"query": {
		"bool": {
			"must": {
				"match": {
					"content": "logstash"
				}
			},
			"must_not": {
				"match": {
					"category": "releases"
				}
			},
		}
	}
}
```

#### Should
Не влияет на сам поиск но добавляет score

`GET` **blogs/_search**
```json
{
	"query": {
		"bool": {
			"must": {
				"match_phrase": {
					"content": "elastic stack"
				}
			},
			"should": {
				"match_phrase": {
					"author": "shay banon"
				}
			},
		}
	}
}
```

#### Filter 

`GET` **blogs/_search**
```json
{
	"query": {
		"bool": {
			"must": {
				"match_phrase": {
					"content": "elastic stack"
				}
			},
			"filter": {
				"range": {
					"publish_date": {
						"lt": "2017-01-01"
					}
				}
			},
		}
	}
}
```

#### Must + Should
поиск будет более релевантный

`GET` **blogs/_search**
```json
{
	"query": {
		"bool": {
			"must": [
				"match": {"title": "elastic"}
			],
			"should": [
				{"match": {"title": "stack"}},
				{"match": {"title": "query"}},
				{"match": {"title": "speed"}}
			]
		}
	}
}
```