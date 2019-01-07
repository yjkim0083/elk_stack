# Elasticsearch Index

## Create  
```
curl -XPUT http://localhost:9200/classes
```
## Create Document
```
curl --header "content-type: application/JSON" -XPOST http://localhost:9200/classes/class/1 -d '
{"title":"Algorithm", "professor": "john"}'
```

## Create Index, type, document from File
```
curl --header "content-type: application/JSON" -XPOST http://localhost:9200/classes/class/1/ -d @oneclass.json
```

## Delete Index
```
curl -XDELETE http://localhost:9200/classes
```

## Update
```
curl --header "content-type: application/JSON" -XPOST http://localhost:9200/classes/class/1/_update?pretty -d '
{"doc":{"unit":1}}'

curl --header "content-type: application/JSON" -XPOST http://localhost:9200/classes/class/1/_update?pretty -d '
{"doc":{"unit":2}}'

curl --header "content-type: application/JSON" -XPOST http://localhost:9200/classes/class/1/_update?pretty -d '
{"script":"ctx._source.unit += 5"}'
```

## Bulk
```
curl --header "content-type: application/JSON" -XPOST http://localhost:9200/_bulk?pretty --data-binary @"./BigData/ch02/classes.json"
```

- classes.json
```
{ "index" : { "_index" : "classes", "_type" : "class", "_id" : "1" } }
{"title" : "Machine Learning","Professor" : "Minsuk Heo","major" : "Computer Science","semester" : ["spring", "fall"],"student_count" : 100,"unit" : 3,"rating" : 5, "submit_date" : "2016-01-02", "school_location" : {"lat" : 36.00, "lon" : -120.00}}
{ "index" : { "_index" : "classes", "_type" : "class", "_id" : "2" } }
{"title" : "Network","Professor" : "Minsuk Heo","major" : "Computer Science","semester" : ["fall"],"student_count" : 50,"unit" : 3,"rating" : 4, "submit_date" : "2016-02-02", "school_location" : {"lat" : 36.00, "lon" : -120.00}}
```

## Mapping
- 관계형 데이터 베이스에서 **Schema** 와 동일함

## Create Mapping
```
curl --header "content-type: application/JSON" -XPUT 'http://localhost:9200/classes/class/_mapping' -d @"./BigData/ch02/classesRating_mapping.json"
```

- classesRating_mapping.json
```
{
        "class" : {
                "properties" : {
                        "title" : {
                                "type" : "text"
                        },
                        "professor" : {
                                "type" : "text"
                        },
                        "major" : {
                                "type" : "text"
                        },
                        "semester" : {
                                "type" : "text"
                        },
                        "student_count" : {
                                "type" : "integer"
                        },
                        "unit" : {
                                "type" : "integer"
                        },
                        "rating" : {
                                "type" : "integer"
                        },
                        "submit_date" : {
                                "type" : "date",
                                "format" : "yyyy-MM-dd"
                        },
                        "school_location" : {
                                "type" : "geo_point"
                        }
                }
        }
}

```

## Search

### 전체 조회
```
curl -XGET localhost:9200/basketball/record/_search?pretty
```

### 옵션 조회
```
curl -XGET 'localhost:9200/basketball/record/_search?q=points:30&pretty'

curl --header "content-type: application/JSON" -XGET localhost:9200/basketball/record/_search -d '
{
    "query": {
        "term" : {
            "points": 30
        }
    }
}'
```
