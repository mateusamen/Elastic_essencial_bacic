# Elastic_essencial_bacic
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
 
 4- Define mapping of nex index and paste properties from original index, changing data type os "qtd" to Short
 
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
