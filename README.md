# Elastic_essencial_bacic
Some basic commands on Elastic, CRUD operations, Index API, Search API, Queries and Filters, Analyzer and Aggregations. All information is provided by Semantix Academy.

-Add document in Index produto. Here we are creating an index for use in this practice, and we will use four documents numbered 1 to 4. We use ```POST``` method as shown bellow. 

```POST produto/_doc/1
{
  "nome": "mouse",
  "qtd": 50,
  "descricao": "com fio USB, compatível com Windows, Mac e Linux"
}```
```POST produto/_doc/2
{
  "nome": "hd",
  "qtd": 20,
  "descricao": "Interface USB 2.0, 500GB, Sistema: Windows 10, Windows 8, Windows 7 "
}```
```POST produto/_doc/3
{
   "nome": "memória ram", 
   "qtd": 10, 
   "descricao": "8GB, DDR4"
}```
```POST produto/_doc/3
{
   "nome": "memória ram", 
   "qtd": 10, 
   "descricao": "8GB, DDR4"
}```
