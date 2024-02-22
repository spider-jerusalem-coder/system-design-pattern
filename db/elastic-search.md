# Elastic Search

### 1. What are the challeneges with Elastic search ?
* No support for seconary index

### 2. What are the benefits of Elastic search ?
* Quad tree


## Phases
1. Character filters: (transform the original string by replacing or adding characters.)
2. Tokenizer: (splitting the string into words)
3. Token filters: (stop word removal and stemming)

### With default analyzer
```
http localhost:9200/_analyze <<< '{
  "text": "lost in translation"
}'
```
```
{
	"tokens": [
    	{
        	"end_offset": 4,
        	"position": 0,
        	"start_offset": 0,
        	"token": "lost",
        	"type": "<ALPHANUM>"
    	},
    	{
        	"end_offset": 7,
        	"position": 1,
        	"start_offset": 5,
        	"token": "in",
        	"type": "<ALPHANUM>"
    	},
    	{
        	"end_offset": 19,
        	"position": 2,
        	"start_offset": 8,
        	"token": "translation",
        	"type": "<ALPHANUM>"
    	}
	]
}
```

### With english analyzer
```
http localhost:9200/_analyze <<< '{
  "analyzer": "english",
  "text": "lost in translation"
}'
```

```
{
	"tokens": [
    	{
        	"end_offset": 4,
        	"position": 0,
        	"start_offset": 0,
        	"token": "lost",
        	"type": "<ALPHANUM>"
    	},
    	{
        	"end_offset": 19,
        	"position": 2,
        	"start_offset": 8,
        	"token": "translat",
        	"type": "<ALPHANUM>"
    	}
	]
}
```

### Bulk upload
```
http PUT localhost:9200/_bulk <<< '
  { "index" : { "_index" : "gifs", "_id" : "1" } }
  { "caption": "Happy birthday my love" }
  { "index" : { "_index" : "gifs", "_id" : "2" } }
  { "caption": "happy birthday to me" }
  { "index" : { "_index" : "gifs", "_id" : "3" } }
  { "caption": "happy birthday my friend" }
'
```

### Query
```
http GET localhost:9200/gifs/_search <<< '{
  "query": {
    "match_phrase" : {
      "caption" : "Happy birthday to"
    }
  }
}'
```

### Result
```
[
  {
    "_id": "2",
    "_index": "gifs",
    "_score": 0.28852317,
    "_source": {
      "caption": "happy birthday to me"
    },
    "_type": "_doc"
  },
  {
    "_id": "1",
    "_index": "gifs",
    "_score": 0.25748682,
    "_source": {
      "caption": "Happy birthday my love"
    },
    "_type": "_doc"
  },
  {
    "_id": "3",
    "_index": "gifs",
    "_score": 0.25748682,
    "_source": {
      "caption": "happy birthday my friend"
    },
    "_type": "_doc"
  }
]
```
