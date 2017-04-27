# ELK

## 우분투에 ELASTICSEARCH 설치

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
