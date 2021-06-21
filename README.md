# Elastic_essencial_bacic
Some basic commands on Elastic, CRUD operations, Index API, Search API, Queries and Filters, Analyzer and Aggregations. All information is provided by Semantix Academy.

-Add document in Index produto. Here we are creating an index for use in this practice, and we will use four documents with id from 1 to 4. We use ```POST``` method as shown bellow. The syntax is ```POST <target>/_doc/<id> ```

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
-Now we want to verify the document with id=3. We do it using ```GET``` method, as shown here: ```GET produto/_source/3```.
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

