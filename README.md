
==================================================================
## MONGODB SHARDING SETUP - COMPLETE GUIDE
==================================================================

### Port Selections 
For Config-server use -> [ ] <br>
For Shard port use -> [ ] <br>
For Route port use -> [ ] <br>

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
### Create directories for shard 2
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
## STEP 4: START MONGOS (QUERY ROUTER)
======================================================

```bash
mongos --configdb configReplSet/localhost:27019 --port 27030 --logpath /data/mongos.log --fork
```


