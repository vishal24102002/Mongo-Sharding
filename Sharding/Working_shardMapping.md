==============================================================
## mongodb sharding working
==============================================================

The basic mapping for the shard keys is stored in config inside the <b>config</b> ( Database ) inside chunks collections

 " I have used single shard key but will update this file when working on the multi shard keys "

### Make sure you run these commands on the router only 

Step 1 : To get the structure of the mapping collection use 
```bash
use config
db.chunks.findOne()
```

Step 2 : To get the uuid for the mapping
```bash
db.collections.findOne({ _id: "DataBase.Collection" })
```

Step 3 : To get sharding maps

- in case of normal shard key
```bash
db.chunks.findOne({
...   uuid: UUID("use your uui found in above step"),
...   "min.area_id": { $lte: single side },
...   "max.area_id": { $gt: ObjectId("68073a75655244ce3a9dc8bb") }
... })
```
=============================================== OR ==============================================

- in case you have used hashed for shard key 
```bash
db.chunks.find(
...   { uuid: UUID("use your uui found in above step") },
...   { min: 1, max: 1, shard: 1 }
... ).sort({ "min.area_id": 1 }).pretty()
```
