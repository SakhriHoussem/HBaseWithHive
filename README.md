# HBase With Hive

## Learn How Hive Work With HBase in Simple Example

We will import DATA from ['Hiking' TABLE created on HBase](https://github.com/SakhriHoussem/HBaseInPractice).

```shell
# Hive startup commande
hive
```

```shell
# show TABLES in Hive 
SHOW TABLES;
```

```shell
# Hive shutdown
exit
```

## Create HBase TABLE 'hiking'  :

The syntax to Create TABLE :
```shell 
CREATE EXTERNAL TABLE table_name(key String, col1 string, col2 int...)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES("hbase.columns.mapping"=":key,columnFamily:colx...") #columnFamily:col in HBase
TBLPROPERTIES("hbase.table.name"="table_name"); #TABLE in HBase
```
### Example : 
For each family column in HBase, We will create TABLE in Hive

```shell 
# import and create external table InfoTechnique
CREATE EXTERNAL TABLE InfoTechnique(key int,distance float, Altitude int)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES("hbase.columns.mapping"=":key,infoTechnique:distance,infoTechnique:Altitude")
TBLPROPERTIES("hbase.table.name"="hiking");
```

```shell
# import and create external table InfoTechnique
CREATE EXTERNAL TABLE infoHiking(key INT,name STRING, region STRING, suiteHiking INT)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES("hbase.columns.mapping"=":key,infoHiking:name,infoHiking:region,infoHiking:suiteHiking")
TBLPROPERTIES ("hbase.table.name" = "hiking");
```
### Queries : 

get from TABLE hiking 4 :
```sql
SELECT info.key ,info.name ,info.region, tech.distance, tech.Altitude, info.suiteHiking
FROM InfoTechnique tech
FULL JOIN  InfoHiking info ON (tech.key = info.key)
WHERE info.key=4;
``` 

get InfoHiking of hiking 5 : 
```sql
SELECT key, name, region, suiteHiking
FROM   InfoHiking info
WHERE key=5;
```

get the distance of the hiking 'Murdjadu Mountain' :
```sql
SELECT tech.distance
FROM InfoTechnique tech FULL 
JOIN  InfoHiking info ON (tech.key = info.key)
WHERE info.name='Montagne de Murdjadju';
```

get the distance of the hiking in the region of 'Tizi Ouzou' :
```sql
SELECT tech.distance FROM InfoTechnique tech
FULL JOIN  InfoHiking info ON (tech.key = info.key)
WHERE info.region='Tizi Ouzou';
```
