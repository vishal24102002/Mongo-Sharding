# sharding
# ============================================
# MONGODB SHARDING SETUP - COMPLETE GUIDE
# ============================================

# STEP 0: CREATE DATA DIRECTORIES
# ============================================
echo "Creating data directories..."

# Create directories for config servers
sudo mkdir -p /data/configdb1
sudo mkdir -p /data/configdb2
sudo mkdir -p /data/configdb3

# Create directories for shard 1
sudo mkdir -p /data/shard1a
sudo mkdir -p /data/shard1b

# Create directories for shard 2
sudo mkdir -p /data/shard2a
sudo mkdir -p /data/shard2b

