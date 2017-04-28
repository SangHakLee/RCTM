# ELK

## ELASTICSEARCH Install on ubuntu

### Install JAVA 8
```bash
sudo add-apt-repository -y ppa:webupd8team/java
sudo apt-get update
sudo apt-get -y install oracle-java8-installer
java -version
```

### Install ELASTICSEARCH
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.1.deb
dpkg -i elasticsearch-5.3.1.deb
sudo systemctl enable elasticsearch.service
```
- Install path: /usr/share/elasticsearch
- Config file: /etc/elasticsearch
- Init script: /etc/init.d/elasticsearch

 #### ELASTICSEARCH Start | Stop | Check
 ```bash
 sudo service elasticsearch start
 sudo service elasticsearch stop
 curl -XGET 'localhost:9200' # check run
 ```

## ELASTICSEARCH Basic Concept
TODO
### ELASTICSEARCH VS RDB


## ELASTICSEARCH CRUD
| ELASTICSEARCH | RDB    | CRUD   |
|---------------|--------|--------|
| GET           | SELECT | READ   |
| PUT           | UPDATE | UPDATE |
| POST          | INSERT | CREATE |
| DELETE        | DELETE | DELETE |

### Verify Index
```bash
curl -XGET localhost:9200/classes
curl -XGET localhost:9200/classes?pretty
```
- `?pretty`: JSON fotmatting 

### Create Index
```bash
curl -XPUT localhost:9200/classes
```
- `Success`: {"acknowledged":true,"shards_acknowledged":true}

### Delete Index
```bash
curl -XDELETE localhost:9200/classes
```
- `Success`: {"acknowledged":true}

### Create Document
```bash
curl -XPOST localhost:9200/classes/class/1/ -d '{"title":"A", "professor":"J"}'
```
- `Success`: {"_index":"classes","_type":"class","_id":"1","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"created":true}

### Create Document from File
```bash
curl -XPOST localhost:9200/classes/class/1/ -d @oneclass.json
```
- `Fail`: {"error":{"root_cause":[{"type":"null_pointer_exception","reason":null}],"type":"null_pointer_exception","reason":null},"status":500}
- `Success`: {"_index":"classes","_type":"class","_id":"1","_version":2,"result":"updated","_shards":{"total":2,"successful":1,"failed":0},"created":false}

#### oneclass.json
```json
{
  "title" : "Machine Learning",
  "Professor" : "Minsuk Heo",
  "major" : "Computer Science",
  "semester" : ["spring", "fall"],
  "student_count" : 100,
  "unit" : 3,
  "rating" : 5
}
```
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch01/oneclass.json
```

## ELASTICSEARCH Update
```bash
curl -XPOST localhost:9200/classes/class/1/ -d '{"title":"Algorithm", "professor":"John"}'
```

### Add Field

#### Way 1. Basic
```bash
curl -XPOST localhost:9200/classes/class/1/_update -d '{"doc":{"unit":1}}'
curl -XGET localhost:9200/classes/class/1?pretty 
```
|                   Before                  |                         After                        |
|:-----------------------------------------:|:----------------------------------------------------:|
| {"title":"Algorithm", "professor":"John"} | {"title":"Algorithm", "professor":"John", "unit": 1} |

#### Way 2. Script
```bash
curl -XPOST localhost:9200/classes/class/1/_update -d '{"script":"ctx._source.unit +=5"}'
```

## ELASTICSEARCH Bulk
```bash
curl -XPOST localhost:9200/_bulk?pretty --data-binary @classes.json
```

### Get classes.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch02/classes.json#
```

## ELASTICSEARCH Mapping
RDB's Schema

Set Data type

### Step 1. Create Index
```bash
curl -XPUT localhost:9200/classes
```

#### Error 
```json
{
  "error": {
    "root_cause": [
      {
        "type": "index_already_exists_exception",
        "reason": "index [classes/5mBFGPEwRpOdJP08VNDJug] already exists",
        "index_uuid": "5mBFGPEwRpOdJP08VNDJug",
        "index": "classes"
      }
    ],
    "type": "index_already_exists_exception",
    "reason": "index [classes/5mBFGPEwRpOdJP08VNDJug] already exists",
    "index_uuid": "5mBFGPEwRpOdJP08VNDJug",
    "index": "classes"
  },
  "status": 400
}
```

```bash
curl -XDELETE localhost:9200/classes
curl -XPUT localhost:9200/classes
```

#### Success
```
{
  "classes" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "creation_date" : "1493388598544",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "N2GHTYgFQqOy_DoP9zuISA",
        "version" : {
          "created" : "5030199"
        },
        "provided_name" : "classes"
      }
    }
  }
}
```

### Step 2. Create Mapping
#### 2-1 Get classesRating_mapping.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch02/classesRating_mapping.json
```

#### 2-2 Mapping
```
curl -XPUT localhost:9200/classes/class/_mapping -d @classesRating_mapping.json
```

#### 2-3 Verify Mapping
```bash
curl -XGET localhost:9200/classes?pretty
```
##### Success
```json
{
  "classes" : {
    "aliases" : { },
    "mappings" : {
      "class" : {
        "properties" : {
          "major" : {
            "type" : "text"
          },
          "professor" : {
            "type" : "text"
          },
          "rating" : {
            "type" : "integer"
          },
          "school_location" : {
            "type" : "geo_point"
          },
          "semester" : {
            "type" : "text"
          },
          "student_count" : {
            "type" : "integer"
          },
          "submit_date" : {
            "type" : "date",
            "format" : "yyyy-MM-dd"
          },
          "title" : {
            "type" : "text"
          },
          "unit" : {
            "type" : "integer"
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "creation_date" : "1493388598544",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "N2GHTYgFQqOy_DoP9zuISA",
        "version" : {
          "created" : "5030199"
        },
        "provided_name" : "classes"
      }
    }
  }
}
```

#### 2-4 Add Documents
```bash
curl -XPOST localhost:9200/_bulk?pretty --data-binary @classes.json
```

##### Verify Documents
```bash
curl XGET localhost:9200/classes/class/1?pretty
```

## ELASTICSEARCH Search

### Get simple_basketball.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/simple_basketball.json#
```

### Add Documents
```bash
curl -XPOST localhost:9200/_bulk --data-binary @simple_basketball.json
```

### Search

#### Search - Basic
```bash
curl -XGET localhost:9200/basketball/record/_search?pretty
```

##### Success
```json
{
  "took" : 254,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "basketball",
        "_type" : "record",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "team" : "Chicago Bulls",
          "name" : "Michael Jordan",
          "points" : 20,
          "rebounds" : 5,
          "assists" : 8,
          "submit_date" : "1996-10-11"
        }
      },
      {
        "_index" : "basketball",
        "_type" : "record",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "team" : "Chicago Bulls",
          "name" : "Michael Jordan",
          "points" : 30,
          "rebounds" : 3,
          "assists" : 4,
          "submit_date" : "1996-10-11"
        }
      }
    ]
  }
}
```

#### Search - Uri 
```bash
curl XGET localhost:9200/basketball/record/_search?q=points:30&pretty
```

##### Success
```json
{
  "took": 15,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "failed": 0
  },
  "hits": {
    "total": 1,
    "max_score": 1.0,
    "hits": [
      {
        "_index": "basketball",
        "_type": "record",
        "_id": "1",
        "_score": 1.0,
        "_source": {
          "team": "Chicago Bulls",
          "name": "Michael Jordan",
          "points": 30,
          "rebounds": 3,
          "assists": 4,
          "submit_date": "1996-10-11"
        }
      }
    ]
  }
}
```

#### Search - Request body
```bash
curl XGET localhost:9200/basketball/record/_search -d '{"query":{"term":{"points":30}}}'

curl XGET localhost:9200/basketball/record/_search -d '{
  "query": {
    "term": {
      "points": 30
    }
  }
}'
```

[search-request-body](https://www.elastic.co/guide/en/elasticsearch/reference/5.3/search-request-body.html)


## ELASTICSEARCH Aggregation(Metric)

### Add Documents
```bash
curl -XPOST localhost:9200/_bulk --data-binary @simple_basketball.json
```

### AVG Aggregation
#### Get avg_points_aggs.json 
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/avg_points_aggs.json
```

```json
{
  "size": 0,
  "aggs": {
    "avg_score": {
      "avg": {
        "field": "points"
      }
    }
  }
}
```

### Average
```
curl -XGET localhost:9200/_search?pretty --data-binary @avg_points_aggs.json
```

### Max
```
curl -XGET localhost:9200/_search?pretty --data-binary @max_points_aggs.json
```

#### Get max_points_aggs.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/max_points_aggs.json
```

```json
{
  "size": 0,
  "aggs": {
    "max_score": {
      "max": {
        "field": "points"
      }
    }
  }
}
```

### Min
```
curl -XGET localhost:9200/_search?pretty --data-binary @min_points_aggs.json
```

#### Get min_points_aggs.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/min_points_aggs.json
```

```json
{
  "size": 0,
  "aggs": {
    "min_score": {
      "min": {
        "field": "points"
      }
    }
  }
}
```

### Sum
```
curl -XGET localhost:9200/_search?pretty --data-binary @sum_points_aggs.json
```

#### Get sum_points_aggs.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/sum_points_aggs.json
```

```json
{
  "size": 0,
  "aggs": {
    "sum_score": {
      "sum": {
        "field": "points"
      }
    }
  }
}
```

### Stats
```
curl -XGET localhost:9200/_search?pretty --data-binary @stats_points_aggs.json
```

#### Get stats_points_aggs.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/stats_points_aggs.json
```

```json
{
  "size": 0,
  "aggs": {
    "stats_score": {
      "stats": {
        "field": "points"
      }
    }
  }
}
```

## ELASTICSEARCH Aggregation(Bucket)

TODO

---

## KIBANA Install on ubuntu

### Install KIBANA
```bash
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.3.2-amd64.deb
dpkg -i kibana-5.3.1.deb
```

### Config
```bash
vi /etc/kibana/kibana.yml
```

```
#server.host: "localhost"

#elasticsearch.url: "http://localhost:9200"
```

### Start KIBANA
```bash
sudo /usr/share/kibana/bin/kibana
```

## KIBANA Management
