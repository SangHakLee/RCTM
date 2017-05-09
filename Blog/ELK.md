<!-- TOC -->

- [ELK](#elk)
  - [Data Science](#data-science)
    - [ELK Stack](#elk-stack)
      - [[ELASTICSEARCH](https://en.wikipedia.org/wiki/Elasticsearch)](#elasticsearchhttpsenwikipediaorgwikielasticsearch)
      - [[LOGSTASH](https://wikitech.wikimedia.org/wiki/Logstash)](#logstashhttpswikitechwikimediaorgwikilogstash)
      - [[KIBANA](https://en.wikipedia.org/wiki/Kibana)](#kibanahttpsenwikipediaorgwikikibana)
  - [ELASTICSEARCH Install on ubuntu](#elasticsearch-install-on-ubuntu)
    - [Install JAVA 8](#install-java-8)
    - [Install ELASTICSEARCH](#install-elasticsearch)
      - [ELASTICSEARCH Start | Stop | Check](#elasticsearch-start--stop--check)
      - [ELASTICSEARCH config (External network)](#elasticsearch-config-external-network)
  - [ELASTICSEARCH Basic Concepts](#elasticsearch-basic-concepts)
    - [ELASTICSEARCH vs RDB](#elasticsearch-vs-rdb)
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
    - [Average](#average)
      - [Get avg_points_aggs.json](#get-avg_points_aggsjson)
    - [Max](#max)
      - [Get max_points_aggs.json](#get-max_points_aggsjson)
    - [Min](#min)
      - [Get min_points_aggs.json](#get-min_points_aggsjson)
    - [Sum](#sum)
      - [Get sum_points_aggs.json](#get-sum_points_aggsjson)
    - [Stats](#stats)
      - [Get stats_points_aggs.json](#get-stats_points_aggsjson)
  - [ELASTICSEARCH Aggregation(Bucket)](#elasticsearch-aggregationbucket)
    - [Create Basketball Index](#create-basketball-index)
    - [Mapping Basketball](#mapping-basketball)
    - [Add Basketball Documents](#add-basketball-documents)
    - [Term Aggs - Group by(team)](#term-aggs---group-byteam)
    - [Stats Aggs - Group by(team)](#stats-aggs---group-byteam)
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
  - [KIBANA Visualize 1](#kibana-visualize-1)
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
  - [Practical data analysis using ELK 1 - Population](#practical-data-analysis-using-elk-1---population)
    - [Collect Datas](#collect-datas)
      - [Datas site](#datas-site)
      - [Population analysis Datas](#population-analysis-datas)
      - [Get Ready-to-use Datas](#get-ready-to-use-datas)
    - [Check ELASTICSEARCH & KIBANA are running](#check-elasticsearch--kibana-are-running)
      - [Check KIBANA](#check-kibana)
        - [Running](#running)
        - [Stopped](#stopped)
      - [Check ELASTICSEARCH](#check-elasticsearch)
        - [Running](#running-1)
        - [Stopped](#stopped-1)
    - [Config LOGSTASH](#config-logstash-1)
      - [OR Download logstash.conf file](#or-download-logstashconf-file)
    - [Run LOGSTASH output to ELASTICSEARCH](#run-logstash-output-to-elasticsearch)
    - [Go KIBANA](#go-kibana)
      - [Add pattern](#add-pattern)
    - [Discover Tab](#discover-tab)
    - [Visualize Tab](#visualize-tab)
  - [Practical data analysis using ELK 2 - Stock](#practical-data-analysis-using-elk-2---stock)
    - [Collcet Datas](#collcet-datas)
      - [Datas site](#datas-site-1)
      - [Stock analysis Datas - Facebook](#stock-analysis-datas---facebook)
      - [Get Ready-to-use Datas](#get-ready-to-use-datas-1)
    - [Check ELASTICSEARCH & KIBANA are running](#check-elasticsearch--kibana-are-running-1)
      - [Check ELASTICSEARCH](#check-elasticsearch-1)
      - [Check KIBANA](#check-kibana-1)
    - [Config LOGSTASH](#config-logstash-2)
      - [OR Download logstash_stock.conf file](#or-download-logstash_stockconf-file)
    - [Run LOGSTASH output to ELASTICSEARCH](#run-logstash-output-to-elasticsearch-1)
    - [Go KIBANA](#go-kibana-1)
      - [Add pattern](#add-pattern-1)
    - [Visualize Tab](#visualize-tab-1)
      - [Line chart](#line-chart)
        - [Result](#result-3)
    - [Dashboard Tab](#dashboard-tab)

<!-- /TOC -->

# ELK

## Data Science
[![Data Science](http://img.youtube.com/vi/J2PIBQgEpC4/0.jpg)](https://youtu.be/J2PIBQgEpC4)

### ELK Stack
![elk](http://blog.arungupta.me/wp-content/uploads/2015/07/elk-stack.png)

#### [ELASTICSEARCH](https://en.wikipedia.org/wiki/Elasticsearch)
https://www.elastic.co/kr/products/elasticsearch

[Lucene](https://en.wikipedia.org/wiki/Apache_Lucene) 기반 검색 엔진
[HTTP](https://ko.wikipedia.org/wiki/HTTP) 웹 인터페이스와 [JSON](https://ko.wikipedia.org/wiki/JSON) 문서 객체를 이용해 분산된 multitenant 가능한 검색을 지원한다.

#### [LOGSTASH](https://wikitech.wikimedia.org/wiki/Logstash)
https://www.elastic.co/kr/products/logstash

server-side 처리 파이프라인, 다양한 소스에서 동시에 Data를 수집-변환 후 Stash로 전송

#### [KIBANA](https://en.wikipedia.org/wiki/Kibana)
https://www.elastic.co/kr/products/kibana

ELASTICSEARCH data를 시각화하고 탐색을 지원




## ELASTICSEARCH Install on ubuntu
[![Data Science](http://img.youtube.com/vi/w7aHzIkD3D4/0.jpg)](https://youtu.be/w7aHzIkD3D4)

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
Allow All Host (AWS 같은 클라우드 서비스를 사용하는 경우 외부에서 접속하기 때문에 네트워크 설정 필요)
```bash
vi /etc/elasticsearch/elasticsearch.yml
```
```bash
network.host: 0.0.0.0
```
**이런 경우 앞으로 사용될 `localhost`를 각자의 `IP`로 변경한다.**
**`locahlost:9200` -> `119.10.10.10:9200`**




## ELASTICSEARCH Basic Concepts 
[![Basic Concepts](http://img.youtube.com/vi/B1Aq2GQ4E78/0.jpg)](https://youtu.be/B1Aq2GQ4E78)

### ELASTICSEARCH vs RDB

| ELASTICSEARCH | RDB      |
|---------------|----------|
| Index         | Database |
| Type          | Table    |
| Document      | Row      |
| Field         | Column   |
| Mapping       | Schema   |

---

http://d2.naver.com/helloworld/273788

|    RDB   |     ELASTICSEARCH     |
|:--------:|:---------------------:|
| Database |         Index         |
| Table    | Type                  |
| Row      | Document              |
| Column   | Field                 |
| Schema   | Mapping               |
| Index    | Everything is indexed |
| SQL      | Query                 |




## ELASTICSEARCH CRUD
[![ELASTICSEARCH CRUD](http://img.youtube.com/vi/lt6oPHjZMXg/0.jpg)](https://youtu.be/lt6oPHjZMXg)
> ELASTICSEARCH에 Document(Row)를 삽입, 삭제, 조회

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
[![ELASTICSEARCH Update](http://img.youtube.com/vi/dHcvUgvwPsc/0.jpg)](https://youtu.be/dHcvUgvwPsc)
> ELASTICSEARCH에 Document(Row)를 수정

```bash
curl -XPOST localhost:9200/classes/class/1/ -d '{"title":"Algorithm", "professor":"John"}'
```
- `classes` : Index ( RDB's Database )
- `class` : Type ( RDB's Table )
- `1` : Document ( RDB's Row )

### Add Field
RDB's Column

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
[![ELASTICSEARCH Bulk](http://img.youtube.com/vi/t0H4dmxmOjA/0.jpg)](https://youtu.be/t0H4dmxmOjA)

> File로 된 여러 개의 Documents를 한 번에 저장

```bash
curl -XPOST localhost:9200/_bulk?pretty --data-binary @classes.json
```

### Get classes.json
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch02/classes.json#
```




## ELASTICSEARCH Mapping
[![ELASTICSEARCH Mapping](http://img.youtube.com/vi/uzPTOgXe7-Q/0.jpg)](https://youtu.be/uzPTOgXe7-Q)
> RDB's Schema. 효율적인 검색을 위해서 데이터의 타입을 정의하는 것

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
```json
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
> Mapping 없이 Index만 생성했기 때문에, `classes.mappings` 의 값이 빈 객체이다.


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
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch02/classes.json
```
```bash
curl -XPOST localhost:9200/_bulk?pretty --data-binary @classes.json
```

##### Verify Documents
```bash
curl -XGET localhost:9200/classes/class/1?pretty
```




## ELASTICSEARCH Search
[![ELASTICSEARCH Search](http://img.youtube.com/vi/nOjGsbXl7ao/0.jpg)](https://youtu.be/nOjGsbXl7ao)

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
[![ELASTICSEARCH Aggregation(Metric)](http://img.youtube.com/vi/BSLtEku8gKA/0.jpg)](https://youtu.be/BSLtEku8gKA)
> 평균, 합, 최소, 최대 등 산술 분석을 제공

### Add Documents
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch03/simple_basketball.json
```

```bash
curl -XPOST localhost:9200/_bulk --data-binary @simple_basketball.json
```

### Average
```
curl -XGET localhost:9200/_search?pretty --data-binary @avg_points_aggs.json
```

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
> 평균, 합, 최소, 최대 한 번에 도출

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
[![ELASTICSEARCH Aggregation(Bucket)](http://img.youtube.com/vi/KzJiO8IEgBk/0.jpg)](https://youtu.be/KzJiO8IEgBk)
> RDB's `Group by`. 데이터를 일정 기준으로 묶어서 결과를 도출.

### Create Basketball Index
```bash
curl -XPUT localhost:9200/basketball
```

### Mapping Basketball
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch04/basketball_mapping.json
```

```bash
curl -XPUT localhost:9200/basketball/record/_mapping -d @basketball_mapping.json
```

### Add Basketball Documents
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch04/twoteam_basketball.json
```

```bash
curl -XPOST localhost:9200/_bulk --data-binary @twoteam_basketball.json
```

### Term Aggs - Group by(team)
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch04/terms_aggs.json
```

```bash
curl -XGET localhost:9200/_search?pretty --data-binary @terms_aggs.json
```

### Stats Aggs - Group by(team)
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch04/stats_by_team.json
```

```bash
curl -XGET localhost:9200/_search?pretty --data-binary @stats_by_team.json
```


---



## KIBANA Install on ubuntu
[![KIBANA](http://img.youtube.com/vi/CV_SuOxk_nM/0.jpg)](https://youtu.be/CV_SuOxk_nM)

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


#### Network
AWS 같은 클라우드 서비스를 사용하는 경우 localhost가 아닌 내부 IP 설정

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
[![KIBANA Management](http://img.youtube.com/vi/7mwK04buD4E/0.jpg)](https://youtu.be/7mwK04buD4E)
> ELASTICSEARCH 저장된 데이터 중 어떤 Index( RDB's Database )를 시각화할지 정한다.

### Step 1. Set Basketball Data
```bash
curl -XDELETE localhost:9200/basketball

curl -XPUT localhost:9200/basketball

wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch04/basketball_mapping.json

curl -XPUT localhost:9200/basketball/record/_mappin -d @basketball_mapping.json

wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch05/bulk_basketball.json

curl -XPOST localhost:9200/_bulk --data-binary @bulk_basketball.json
```

### Step 2. Access KIBANA
> AWS 같은 클라우드를 사용하면 `localhost` 를 각자의 IP 주소로 바꾼다.

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
[![KIBANA Discover](http://img.youtube.com/vi/ebXczuiMbEQ/0.jpg)](https://youtu.be/ebXczuiMbEQ)
> ELASTICSEARCH 의 저장된 데이터를 JSON, Table 형식으로 보여준다. Filter를 이용해서 원하는 정보만 볼 수 있다.

Go To `Discover` Tab (http://localhost:5601/app/kibana#/discover)

### Step 1. Time Range
![kibana-discover-1](https://cloud.githubusercontent.com/assets/9030565/25625100/87f5e020-2f96-11e7-87e8-da06c3b25669.jpg)

### Step 2. Select Item
![kibana-discover-2](https://cloud.githubusercontent.com/assets/9030565/25625340/3c4d6980-2f97-11e7-943b-9c52e69459d5.jpg)

### Step 3. Filtering
![kibana-discover-3](https://cloud.githubusercontent.com/assets/9030565/25625501/a3cc6174-2f97-11e7-9c36-d90b4a3d6753.png)

### Step 4. Toggle
![kibana-discover-4](https://cloud.githubusercontent.com/assets/9030565/25625632/fb6be706-2f97-11e7-87c7-ed9f71eb9329.png)




## KIBANA Visualize 1
[![KIBANA Visualize 1](http://img.youtube.com/vi/QwYGzRouTsA/0.jpg)](https://youtu.be/QwYGzRouTsA)
> 막대 차트와 파이 차트로 데이터 시각화를 실습한다.

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
[![KIBANA Visualize 2](http://img.youtube.com/vi/jJ8BOb2IZnc/0.jpg)](https://youtu.be/jJ8BOb2IZnc)
> Map 차트를 이용한 데이터 시각화를 실습한다.

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
[![KIBANA Dashboard](http://img.youtube.com/vi/IIl1P7ux9o8/0.jpg)](https://youtu.be/IIl1P7ux9o8)

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

`Create a dashboard` -> `Add` -> `Select Dashboard` -> `Save`

![kibana-visualize-10](https://cloud.githubusercontent.com/assets/9030565/25628552/9429126c-2fa1-11e7-82aa-288162f5ed2b.jpg)


---


## LOGSTASH Install on ubuntu
[![LOGSTASH](http://img.youtube.com/vi/FpEubrKOoVE/0.jpg)](https://youtu.be/FpEubrKOoVE)
> LOGSTASH는 `ELK` 스택에서 Input에 해당한다. 
> 다양한 형태의 데이터를 받아들여서 사용자가 지정한 형식에 맞게 필터링한 후 ELASTICSEARCH로 보낸다.


![elk](http://blog.arungupta.me/wp-content/uploads/2015/07/elk-stack.png)
> Logstash is an open source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to your favorite “stash.”

### Install LOGSTASH
**Java must required first!!**

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




## Practical data analysis using ELK 1 - Population
[![Population](http://img.youtube.com/vi/iq3t7X_9URA/0.jpg)](https://youtu.be/iq3t7X_9URA)
> 현재까지 구성된 `ELK` 스택을 이용해서 세계 인구 분석을 실습한다.

### Collect Datas

#### Datas site
https://catalog.data.gov/dataset

#### Population analysis Datas
https://catalog.data.gov/dataset/population-by-country-1980-2010-d0250

#### Get Ready-to-use Datas
> Site에서 받은 데이터는 약간의 수정이 필요하다. 아래의 데이터는 바로 사용할 수 있는 데이터.

```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch06/populationbycountry19802010millions.csv
```

### Check ELASTICSEARCH & KIBANA are running

#### Check KIBANA
``` bash
ps -ef | grep kibana
```

##### Running
```bash
root     29968 29933  9 16:58 pts/0    00:00:06 /usr/share/kibana/bin/../node/bin/node --no-warnings /usr/share/kibana/bin/../src/cli
root     30036 30018  0 16:59 pts/1    00:00:00 grep --color=auto kibana
```

##### Stopped
```bash
root     29957 29933  0 16:57 pts/0    00:00:00 grep --color=auto kibana
```

`Restart`
```bash
sudo /usr/share/kibana/bin/kibana
```

#### Check ELASTICSEARCH
```bash
service elasticsearch status

## OR

curl -XGET 'localhost:9200'
```

##### Running
```bash
● elasticsearch.service - Elasticsearch ....

## OR

{
  "name" : "lPQjk0j",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "MORDslcmSKywLz4ReZtsIA",
  "version" : {
    "number" : "5.3.1",
    "build_hash" : "5f9cf58",
    "build_date" : "2017-04-17T15:52:53.846Z",
    "build_snapshot" : false,
    "lucene_version" : "6.4.2"
  },
  "tagline" : "You Know, for Search"
}
```

##### Stopped
```bash
curl: (7) Failed to connect to localhost port 9200: Connection refused
```

`Restart`
```bash
sudo service elasticsearch start
```

### Config LOGSTASH
> 받은 파일을 LOGSTASH를 이용해서 필터링한 후 ELASTICSEARCH에 넣어준다.

```bash
vi logstash.conf
```
```bash
input {
  file {
    path => "/home/minsuk/Documents/git-repo/BigData/ch06/populationbycountry19802010millions.csv"
    start_position => "beginning"  
    sincedb_path => "/dev/null"  
  }
}
filter {
  csv {
      separator => ","
      columns => ["Country","1980","1981","1982","1983","1984","1985","1986","1987","1988","1989","1990","1991","1992","1993","1994","1995","1996","1997","1998","1999","2000","2001","2002","2003","2004","2005","2006","2007","2008","2009","2010"]
  }
  mutate {convert => ["1980", "float"]}
  mutate {convert => ["1981", "float"]}
  mutate {convert => ["1982", "float"]}
  mutate {convert => ["1983", "float"]}
  mutate {convert => ["1984", "float"]}
  mutate {convert => ["1985", "float"]}
  mutate {convert => ["1986", "float"]}
  mutate {convert => ["1987", "float"]}
  mutate {convert => ["1988", "float"]}
  mutate {convert => ["1989", "float"]}
  mutate {convert => ["1990", "float"]}
  mutate {convert => ["1991", "float"]}
  mutate {convert => ["1992", "float"]}
  mutate {convert => ["1993", "float"]}
  mutate {convert => ["1994", "float"]}
  mutate {convert => ["1995", "float"]}
  mutate {convert => ["1996", "float"]}
  mutate {convert => ["1997", "float"]}
  mutate {convert => ["1998", "float"]}
  mutate {convert => ["1999", "float"]}
  mutate {convert => ["2000", "float"]}
  mutate {convert => ["2001", "float"]}
  mutate {convert => ["2002", "float"]}
  mutate {convert => ["2003", "float"]}
  mutate {convert => ["2004", "float"]}
  mutate {convert => ["2005", "float"]}
  mutate {convert => ["2006", "float"]}
  mutate {convert => ["2007", "float"]}
  mutate {convert => ["2008", "float"]}
  mutate {convert => ["2009", "float"]}
  mutate {convert => ["2010", "float"]}
}
output {  
    elasticsearch {
        hosts => "localhost"
        index => "population"
    }
    stdout {}
}
```

- input -> file -> path
  - Edit `Your` own file path
  - e.g.) **"/root/populationbycountry19802010millions.csv"**

#### OR Download logstash.conf file
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch06/logstash.conf
```

### Run LOGSTASH output to ELASTICSEARCH
/usr/share/logstash/bin/logstash -f ./logstash.conf

### Go KIBANA
http://localhost:5601/app/kibana#/management?_g=()

#### Add pattern
![population-analysis-1](https://cloud.githubusercontent.com/assets/9030565/25779116/9b082ef8-334b-11e7-8384-4cb7977d416d.jpg)

### Discover Tab
http://localhost:5601/app/kibana#/discover?_g=(refreshInterval:(display:Off,pause:!f,value:0),time:(from:now-1y,mode:quick,to:now))&_a=(columns:!(_source),index:population,interval:auto,query:'',sort:!('@timestamp',desc))))

![population-analysis-2](https://cloud.githubusercontent.com/assets/9030565/25779142/0d52b078-334c-11e7-9745-01e6237e61a3.jpg)

### Visualize Tab



## Practical data analysis using ELK 2 - Stock
[![Population](http://img.youtube.com/vi/3iA-ncqAqYE/0.jpg)](https://youtu.be/3iA-ncqAqYE)
> 현재까지 구성된 `ELK` 스택을 이용해서 주식 분석을 실습한다.

http://blog.webkid.io/visualize-datasets-with-elk/

### Collcet Datas

#### Datas site
https://finance.yahoo.com

#### Stock analysis Datas - Facebook
https://finance.yahoo.com/quote/FB/history?period1=1336316400&period2=1494082800&interval=1d&filter=history&frequency=1d

#### Get Ready-to-use Datas
> Site에서 받은 데이터는 약간의 수정이 필요하다. 아래의 데이터는 바로 사용할 수 있는 데이터.

```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch06/table.csv
```

### Check ELASTICSEARCH & KIBANA are running

#### Check ELASTICSEARCH
```bash
service elasticsearch status
```

#### Check KIBANA
``` bash
ps -ef | grep kibana
```

### Config LOGSTASH
```bash
vi logstash_stock.conf
```
```bash
input {
  file {
    path => "/home/minsuk/Documents/git-repo/BigData/ch06/table.csv"
    start_position => "beginning"
    sincedb_path => "/dev/null"    
  }
}
filter {
  csv {
      separator => ","
      columns => ["Date","Open","High","Low","Close","Volume","Adj Close"]
  }
  mutate {convert => ["Open", "float"]}
  mutate {convert => ["High", "float"]}
  mutate {convert => ["Low", "float"]}
  mutate {convert => ["Close", "float"]}
}
output {  
    elasticsearch {
        hosts => "localhost"
        index => "stock"
    }
    stdout {}
}
```
- input -> file -> path
  - Edit `Your` own file path
  - e.g.) **"/root/table.csv"**

#### OR Download logstash_stock.conf file
```bash
wget https://raw.githubusercontent.com/minsuk-heo/BigData/master/ch06/logstash_stock.conf
```

### Run LOGSTASH output to ELASTICSEARCH
/usr/share/logstash/bin/logstash -f ./logstash_stock.conf

### Go KIBANA
http://localhost:5601/app/kibana#/management?_g=()

#### Add pattern
![stock-analysis-1](https://cloud.githubusercontent.com/assets/9030565/25779296/ba32ffa2-334f-11e7-9374-1178adef72b4.jpg)

### Visualize Tab

#### Line chart
![stock-analysis-2](https://cloud.githubusercontent.com/assets/9030565/25779314/3bfa2e16-3350-11e7-92e7-12dcef6ff49b.jpg)

##### Result
![stock-analysis-3](https://cloud.githubusercontent.com/assets/9030565/25779332/6bd71112-3350-11e7-8bab-e3722974922d.jpg)


### Dashboard Tab
![stock-analysis-4](https://cloud.githubusercontent.com/assets/9030565/25779363/0bc7af60-3351-11e7-9899-12b8e39f5961.jpg)
