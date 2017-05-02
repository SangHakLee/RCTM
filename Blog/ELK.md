<!-- TOC -->

- [ELK](#elk)
  - [ELASTICSEARCH Install on ubuntu](#elasticsearch-install-on-ubuntu)
    - [Install JAVA 8](#install-java-8)
    - [Install ELASTICSEARCH](#install-elasticsearch)
  - [ELASTICSEARCH Basic Concept](#elasticsearch-basic-concept)
    - [ELASTICSEARCH VS RDB](#elasticsearch-vs-rdb)
  - [ELASTICSEARCH CRUD](#elasticsearch-crud)
    - [Verify Index](#verify-index)
    - [Create Index](#create-index)
    - [Delete Index](#delete-index)
    - [Create Document](#create-document)
    - [Create Document from File](#create-document-from-file)
      - [oneclass.json](#oneclassjson)
  - [ELASTICSEARCH Update](#elasticsearch-update)
    - [Add Field](#add-field)
      - [Way 1. Basic](#way-1-basic)
      - [Way 2. Script](#way-2-script)
  - [ELASTICSEARCH Bulk](#elasticsearch-bulk)
    - [Get classes.json](#get-classesjson)
  - [ELASTICSEARCH Mapping](#elasticsearch-mapping)
    - [Step 1. Create Index](#step-1-create-index)
      - [Error](#error)
      - [Success](#success)
    - [Step 2. Create Mapping](#step-2-create-mapping)
      - [2-1 Get classesRating_mapping.json](#2-1-get-classesrating_mappingjson)
      - [2-2 Mapping](#2-2-mapping)
      - [2-3 Verify Mapping](#2-3-verify-mapping)
        - [Success](#success-1)
      - [2-4 Add Documents](#2-4-add-documents)
        - [Verify Documents](#verify-documents)
  - [ELASTICSEARCH Search](#elasticsearch-search)
    - [Get simple_basketball.json](#get-simple_basketballjson)
    - [Add Documents](#add-documents)
    - [Search](#search)
      - [Search - Basic](#search---basic)
        - [Success](#success-2)
      - [Search - Uri](#search---uri)
        - [Success](#success-3)
      - [Search - Request body](#search---request-body)
  - [ELASTICSEARCH Aggregation(Metric)](#elasticsearch-aggregationmetric)
    - [Add Documents](#add-documents-1)
    - [AVG Aggregation](#avg-aggregation)
      - [Get avg_points_aggs.json](#get-avg_points_aggsjson)
    - [Average](#average)
    - [Max](#max)
      - [Get max_points_aggs.json](#get-max_points_aggsjson)
    - [Min](#min)
      - [Get min_points_aggs.json](#get-min_points_aggsjson)
    - [Sum](#sum)
      - [Get sum_points_aggs.json](#get-sum_points_aggsjson)
    - [Stats](#stats)
      - [Get stats_points_aggs.json](#get-stats_points_aggsjson)
  - [ELASTICSEARCH Aggregation(Bucket)](#elasticsearch-aggregationbucket)
  - [KIBANA Install on ubuntu](#kibana-install-on-ubuntu)
    - [Install KIBANA](#install-kibana)
    - [Config](#config)
      - [External network](#external-network)
    - [Start KIBANA](#start-kibana)
  - [KIBANA Management](#kibana-management)
    - [Step 1. Set Basketball Data](#step-1-set-basketball-data)
    - [Step 2. Access KIBANA](#step-2-access-kibana)
      - [Create Index Patterns](#create-index-patterns)
      - [Created](#created)
  - [KIBANA Discover](#kibana-discover)
    - [Step 1. Time Range](#step-1-time-range)
    - [Step 2. Select Item](#step-2-select-item)
    - [Step 3. Filtering](#step-3-filtering)
    - [Step 4. Toggle](#step-4-toggle)
  - [KIBANA Visualize](#kibana-visualize)
    - [Vertical bar chart](#vertical-bar-chart)
      - [metrics](#metrics)
      - [buckets](#buckets)
      - [Result](#result)
    - [Pie bar chart](#pie-bar-chart)
      - [metrics](#metrics-1)
      - [buckets](#buckets-1)
      - [Result](#result-1)
  - [KIBANA Visualize 2](#kibana-visualize-2)
    - [Create Mapping](#create-mapping)
    - [Add Documents](#add-documents-2)
    - [Verify Documents](#verify-documents-1)
    - [Kibana Management](#kibana-management)
    - [Kibana Visualize - Tile map](#kibana-visualize---tile-map)
      - [buckets](#buckets-2)
  - [KIBANA Dashboard](#kibana-dashboard)
    - [Kibana Visualize](#kibana-visualize)
      - [metrics](#metrics-2)
      - [buckets](#buckets-3)
      - [Result](#result-2)
      - [Save](#save)
    - [Create Dashboard](#create-dashboard)
  - [LOGSTASH Install on ubuntu](#logstash-install-on-ubuntu)
    - [Install LOGSTASH](#install-logstash)
    - [Config LOGSTASH](#config-logstash)
    - [Run LOGSTASH](#run-logstash)

<!-- /TOC -->

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
 
 #### ELASTICSEARCH config (External network)
 Allow All Host
 ```bash
vi /etc/elasticsearch/elasticsearch.yml

 ```bash
 network.host: 0.0.0.0
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
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.3.1-amd64.deb
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

#### External network
```bash
ifconfig | grep inet
```
It will returns
> inet addr:192.169.212.10  Bcast:192.169.212.255  Mask:255.255.255.0
> inet6 addr: fe80::d00d:d9ff:fecc:9dfd/64 Scope:Link
> inet addr:127.0.0.1  Mask:255.0.0.0
> inet6 addr: ::1/128 Scope:Host
```bash
server.host: 192.169.212.10
```


### Start KIBANA
```bash
sudo /usr/share/kibana/bin/kibana
```

## KIBANA Management
### Step 1. Set Basketball Data
```bash
curl -XDELETE localhost:9200/basketball

curl -XPUT localhost:9200/basketball

wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch04/basketball_mapping.json

curl -XPUT localhost:9200/basketball/record/_mappin -d @basketball_mapping.json

wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch05/bulk_basketball.json

curl -XPOST localhost:9200/_bulk --data-binary @bu k_basketball.json
```

### Step 2. Access KIBANA
http://localhost:5601

Go To `Management` Tab (http://localhost:5601/app/kibana#/management/kibana/index?_g=())

#### Create Index Patterns

- **Index name or pattern**
  - basketball*
- **Time-field name**
  - submit_date

![kibana-management-1](https://cloud.githubusercontent.com/assets/9030565/25624702/32eca740-2f95-11e7-9c01-44954a09b44b.jpg)

#### Created
![kibana-management-2](https://cloud.githubusercontent.com/assets/9030565/25624795/85eb9226-2f95-11e7-941e-7a8fc3d2f3b9.jpg)

## KIBANA Discover

Go To `Discover` Tab (http://localhost:5601/app/kibana#/discover)

### Step 1. Time Range
![kibana-discover-1](https://cloud.githubusercontent.com/assets/9030565/25625100/87f5e020-2f96-11e7-87e8-da06c3b25669.jpg)

### Step 2. Select Item
![kibana-discover-2](https://cloud.githubusercontent.com/assets/9030565/25625340/3c4d6980-2f97-11e7-943b-9c52e69459d5.jpg)

### Step 3. Filtering
![kibana-discover-3](https://cloud.githubusercontent.com/assets/9030565/25625501/a3cc6174-2f97-11e7-9c36-d90b4a3d6753.png)

### Step 4. Toggle
![kibana-discover-4](https://cloud.githubusercontent.com/assets/9030565/25625632/fb6be706-2f97-11e7-87c7-ed9f71eb9329.png)

## KIBANA Visualize

Go To `Visualize` Tab (http://localhost:5601/app/kibana#/visualize)

### Vertical bar chart

#### metrics
- **Y-Axis**
  - Aggregation
    - Average
  - Field
    - points
  - Custom Label
    - avg
#### buckets
- **X-Axis**
  - Aggregation
    - Terms
  - Field
    - name.keyword
  - Order by
    - metric: avg

![kibana-visualize-1](https://cloud.githubusercontent.com/assets/9030565/25626401/5e9ff342-2f9a-11e7-9ee5-91bb62d17e29.jpg)

#### Result
![kibana-visualize-2](https://cloud.githubusercontent.com/assets/9030565/25626455/8acf1416-2f9a-11e7-9700-53016bbcb6a2.jpg)

### Pie bar chart

#### metrics
- **Slice Size**
  - Aggregation
    - Sum
  - Field
    - points
#### buckets
- **Split Slices**
  - Aggregation
    - Terms
  - Field
    - team.keyword
  - Order by
    - metric: Sum of points

![kibana-visualize-3](https://cloud.githubusercontent.com/assets/9030565/25626669/3c753ec0-2f9b-11e7-8dc1-47783657cd63.jpg)

#### Result
![kibana-visualize-4](https://cloud.githubusercontent.com/assets/9030565/25626742/72bd0e9a-2f9b-11e7-9c17-34ea238477ec.jpg)

## KIBANA Visualize 2

### Create Mapping
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch02/classesRating_mapping.json

curl -XPUT localhost:9200/classes/class/_mapping -d @classesRating_mapping.json
```

### Add Documents
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch02/classes.json#

curl -XPUT localhost:9200/_bulk?pretty --data-binary @classes.json
```

### Verify Documents
```bash
curl -XGET localhost:9200/classes/class/1?pretty
```

### Kibana Management
- **Index name or pattern**
  - classes*
- **Time-field name**
  - submit_date

![kibana-visualize-5](https://cloud.githubusercontent.com/assets/9030565/25627365/4a678874-2f9d-11e7-8629-771e1ac304b4.jpg)

### Kibana Visualize - Tile map

#### buckets
- **Geo Coordinates**
  - Aggregation
    - Geohash
  - Field
    - school_location

![kibana-visualize-6](https://cloud.githubusercontent.com/assets/9030565/25627596/116dd342-2f9e-11e7-8c6c-44cdc6774989.jpg)

## KIBANA Dashboard

### Kibana Visualize

`Vertical bar chart` -> `classes`

#### metrics
- **Y-Axis**
  - Aggregation
    - Average
  - Field
    - points
#### buckets
- **X-Axis**
  - Aggregation
    - Terms
  - Field
    - Professor.keyword
  - Order
    - Descending
  - Size
    - 16

![kibana-visualize-7](https://cloud.githubusercontent.com/assets/9030565/25628101/fb9024ce-2f9f-11e7-96b0-027f32d1d929.jpg)

#### Result
![kibana-visualize-8](https://cloud.githubusercontent.com/assets/9030565/25628219/60b419d2-2fa0-11e7-8ac7-40596ea24f73.jpg)

#### Save
![kibana-visualize-9](https://cloud.githubusercontent.com/assets/9030565/25628266/92ffab0e-2fa0-11e7-9494-5329fb89bdc7.jpg)

### Create Dashboard

Go To `Dashboard` Tab (http://localhost:5601/app/kibana#/dashboards)

`Create a dashboard` -> `Add` -> `Select Dashboard` -> 

![kibana-visualize-10](https://cloud.githubusercontent.com/assets/9030565/25628552/9429126c-2fa1-11e7-82aa-288162f5ed2b.jpg)

---

## LOGSTASH Install on ubuntu
![elk](http://blog.arungupta.me/wp-content/uploads/2015/07/elk-stack.png)

> Logstash is an open source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to your favorite “stash.”

### Install LOGSTASH
**Java must required!!**
```bash
wget https://artifacts.elastic.co/downloads/logstash/logstash-5.3.1.deb
dpkg -i logstash-5.3.1.deb
```
- Install path: /usr/share/logstash

### Config LOGSTASH
```bash
vi logstash-simple.conf
```

```bash
input {
        stdin { }
}
output {
        stdout { }
}
```

### Run LOGSTASH
```
sudo /usr/share/logstash/bin/logstash -f ./logstash-simple.conf
```
