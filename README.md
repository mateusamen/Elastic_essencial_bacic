# Elastic_essencial_basic
Some basic commands on Elastic, CRUD operations, Index API, Search API, Queries and Filters, Analyzer and Aggregations. All information is provided by Semantix Academy.


## CRUD operations

-Add document in Index produto. Here we are CREATING an index for use in this practice, and we will use four documents with id from 1 to 4. We use ```POST``` method as shown bellow. The syntax is ```POST <target>/_doc/<id> ```

```
POST produto/_doc/1
{
  "nome": "mouse",
  "qtd": 50,
  "descricao": "com fio USB, compatível com Windows, Mac e Linux"
}
POST produto/_doc/2
{
  "nome": "hd",
  "qtd": 20,
  "descricao": "Interface USB 2.0, 500GB, Sistema: Windows 10, Windows 8, Windows 7 "
}
POST produto/_doc/3
{
   "nome": "memória ram", 
   "qtd": 10, 
   "descricao": "8GB, DDR4"
}
POST produto/_doc/4
{
  "nome": "cpu",
  "qtd": 15,
  "descricao": "i5, 2.5Ghz"
}
```
-Now we want to READ the document with id=3. We do it using ```GET``` method, as shown here: ```GET produto/_source/3```.
The output is:
![output01](https://user-images.githubusercontent.com/62483710/122819982-7e017080-d2b1-11eb-840d-a6119f74d50d.PNG)

-Now we want to update the attribute qtd=30 for document with id=3. We use: 
```
POST produto/_update/3
{
  "doc":{
    "qtd" : 30
  }
}
```

-To count the number os documents in index produto we use ```GET produto/_count```.

-Now we will do some queries:
First query will be search a document with attibute ```"nome":"mouse"```. The query will be the following:
```GET produto/_search?q=nome:mouse```

The output:

![output1](https://user-images.githubusercontent.com/62483710/122823153-64fabe80-d2b5-11eb-8279-900e254358d0.PNG)

To UPDATE the index produto by inserting a field data of type date:

```
PUT produto/_mapping
{
  "properties":{
    "data":{"type":"date"}
  }
}
```
To read information from index produto, document with id=6

```GET produto/_source/6```

output:

![readme_1_source](https://user-images.githubusercontent.com/62483710/124340203-e7bf2b80-db89-11eb-9faf-b5a4bdaa0718.PNG)

---
### Reindex

Change data type of attribute "qtd", from Long to Short. This will be done with 5 steps:

 1- Get mapping from original index:
 
 ```GET produto/_mapping```
 
 2- from the output, get properties and copy the information
 
 3- Create new Index:
 
 ```GET produto/_mapping```
 
 4- Define mapping of nex index and paste properties from original index, changing data type from "qtd" to Short
 
 ```PUT produto2/_mapping
{
  "properties": {
    "data": {
      "type": "date"
    },
    "descricao": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "nome": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "properties": {
      "properties": {
        "data": {
          "properties": {
            "type": {
              "type": "text",
              "fields": {
                "keyword": {
                  "type": "keyword",
                  "ignore_above": 256
                }
              }
            }
          }
        }
      }
    },
    "qtd": {
      "type": "short"
    }
  }
}
```

 5- Reindex:
 
 ```
 POST _reindex
{
  "source":{
    "index": "produto"
  }
  , "dest": {
    "index": "produto2"
  }
}
```
---

## Query and Filters

Search in index produto for *mouse* or *teclado*:

```
GET produto/_search
{
  "query": {
    "terms": {
      "nome": ["mouse","teclado"]
    }
  }
}
```

Search in index produto for *mouse* or *teclado* with constant score

```
GET produto/_search
{
  "query":{
    "constant_score": {
      "filter": {
        "terms": {
          "nome": [
            "mouse",
            "teclado"
          ]
        }
      },
      "boost": 1.2
    }
  }
}
```

Search for document that had field = "usb":

```
GET produto/_search
{
  "query": {
    "match": {
      "descricao": "USB"
    }
  }
}
```

---
### Bool Query

Search for documents that has word "USB" and don't have the word "LINUX"

```
GET produto/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {"descricao":"usb"}
        }
      ],
      "must_not": [
        {
          "match": {"descricao":"linux"}
        }
      ]
    }
  }
}
```

Search documents that hads word *"memoria"* in atributte *nome* or has the word *"USB"* and don't have the word *"LINUX"* in attribute *"descricao"*

```
GET produto/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "nome": "memoria"
          }
        },
        {
          "match": {
            "descricao": "usb"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "descricao": "linux"
          }
        }
      ]
    }
  }
}
```

Search for documents that has the word *"Windows"* and *"Linux"* in attribute *descricao*

```
GET produto/_search
{
  "query": {
    "match": {
      "descricao": {
        "query": "Windows Linux",
        "operator": "and"
      }
    }
  }
}
```

Search for documents that has the word *"Windows"*, *"Linux"* or *"USB"* in attribute *descricao*

```
GET produto/_search
{
  "query": {
    "match": {
      "descricao": {
        "query": "Windows Linux usb",
        "minimum_should_match": 1
      }
    }
  }
}
```

---

### Range Query

Show documents with attribute *"Median Age"* greater than 70:

```
GET populacao/_search
{
  "query": {"range": {
    "Median Age": {
      "gt":70
      }
    }
  }
}
```

---
### Query timestamp

for this part, download index ["bolsa"](https://academy.semantix.com.br/courses/43/files/1874/download)

visualize in index *bolsa* documents with date greater than 2019-01-01 and less than 2019-03-01

```
GET bolsa/_search
{
  "query":{
    "range": {
      "@timestamp": {
        "gt": "2019-01-01",
        "lt": "2019-04-01"
      }
    }
  }
}
```
Visualize in index *bolsa* documents with date greater than 2019-04-01 and less than today

```
GET bolsa/_search
{
  "query":{
    "range": {
      "@timestamp": {
        "gt": "2019-01-01",
        "lt": "now",
        "time_zone":"+03:00"
      }
    }
  }
}
```
## Monitoring using Beats

Monitoring logs in .logs file using file beat. For Ubuntu, download file beats using:

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.1.2-linux-x86_64.tar.gz
tar xzvf filebeat-8.1.2-linux-x86_64.tar.gz
```

You should download any .logs file to continue with this tutorial. Once downloaded, copy the path of the file to be moniroting. You can find the path uding ```pwd``` command.

Now we must config the filebeat.yml file. You can do that using ```vi filebeat.yml```. 
Once inside, in Filebeat.input, change the value of Enable to True and paste the path of the desired file to be monitorired.

![input vi yml beat](https://user-images.githubusercontent.com/62483710/161452615-f35ace08-ca98-421a-8a2b-b2f844bf52c7.PNG)

When finished editing, sae and quit, then test if filebeat is ok, inside *filebeat-8.1.2-linux-x86_64* directory run the command ´´´ ./filebeat test config´´´.

Config OK is returned is case os success.

to test for connection, run ``` ./filebeat test output```

![output](https://user-images.githubusercontent.com/62483710/161452844-9bdfdee0-077b-43f9-9a1a-5cf220d17099.PNG)

 to run filbeat: ``` sudo ./filebeat -e```.
 
 


