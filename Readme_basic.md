==================================================================
## MONGODB SHARDING SETUP - COMPLETE GUIDE
==================================================================

### Requirements
1. Same LAN or Network
2. Same MongoDB Version

### Port Selections 
```bash 
Config: 27019, 27020, 27021
Shards: 27017, 27018, 27022, 27023, 27024, 27025
Router: 27030
```
===============================================
## STEP 0: CREATE DATA DIRECTORIES
===============================================

### Create directories for config servers
```bash
sudo mkdir -p /data/configdb1
sudo mkdir -p /data/configdb2 #--> optional 
```
### Create directories for shard 1
```bash
sudo mkdir -p /data/shard1a
sudo mkdir -p /data/shard1b
```
### Create directories for shard 2 (Optional)
```bash
sudo mkdir -p /data/shard2a
sudo mkdir -p /data/shard2b
```
============================================
## STEP 1: START CONFIG SERVERS
============================================

### Terminal 1 - Config Server 1
```bash
mongod --configsvr --replSet configReplSet --port 27019 --dbpath /data/configdb1 --logpath /data/configdb1/mongod.log --fork
```
### Terminal 2 - Config Server 2 (Optional)
```bash
mongod --configsvr --replSet configReplSet --port 27020 --dbpath /data/configdb2 --logpath /data/configdb2/mongod.log --fork
```

### Initialize config server replica set
```bash
mongosh --port 27019 --eval '
rs.initiate({
  _id: "configReplSet",
  configsvr: true,
  members: [
    { _id: 0, host: "localhost:27019" },
    { _id: 0, host: "localhost:27020" } #--> optional 
  ]
});
```

============================================
## STEP 2: START SHARD 1 SERVERS
============================================


### Shard 1 - Primary
```bash
mongod --shardsvr --replSet shard1 --port 27017 --dbpath /data/shard1a --logpath /data/shard1a/mongod.log --fork
```
### Shard 1 - Secondary
```bash
mongod --shardsvr --replSet shard1 --port 27018 --dbpath /data/shard1b --logpath /data/shard1b/mongod.log --fork
```

### Initialize Shard 1 replica set
```bash
mongosh --port 27017 --eval '
rs.initiate({
  _id: "shard1",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" }
  ]
});
```

======================================================
## STEP 3: START MONGOS (QUERY ROUTER)
======================================================

```bash
mongos --configdb configReplSet/localhost:27019 --port 27030 --logpath /data/mongos.log --fork
```

if using more then 1 config server use the below command to run the route
```bash
mongos --configdb configReplSet/localhost:27019,localhost:27020 --port 27030 --logpath /data/mongos.log --fork
```

============================================
## STEP 4: ADD SHARDS TO CLUSTER
============================================
```bash
mongosh --port 27030 --eval '
sh.addShard("shard1/localhost:27017,localhost:27018");
sh.addShard("shard2/localhost:27022,localhost:27023");
'
```

### command to enable sharding on a particular database
```bash
mongosh --port 27030 --eval '
// Enable sharding for database
sh.enableSharding("myDatabase");

// Create collection with shard key
sh.shardCollection("myDatabase.users", { _id: "hashed" });
'
```
==============================================================
## ⚠️ Common error codes 
==============================================================

| **Code** | **Meaning / Common Cause** |
|-----------|-----------------------------|
| **100** | Missing log or data directory, bad permissions. |
| **14 / 20** | `--dbpath` doesn’t exist or not writable. |
| **48** | Port already in use. |
| **110** | Replica set name mismatch or connection issue. |


!! Note :- <br>
--> Keep in mind to remove the directory conflict if starting a new setup for sharding.<br>
--> Also make for using Sharding on a <b>LAN</b> use the <b>IP</b> instead of localhost as mongo does not support both local and ips at a same time<br>
--> Use Routers Mongosh to run the mongosb query and also make sure the used the correct shard key.<br>
--> If using more then one PC/Laptop then make sure the MongoDB version is same and also you need to resetup in case of any IP change.so use the static ip instead of dynamic ones.<br>
