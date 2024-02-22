# Cache

## Estimation
* 20% of the data can service 80% of the request
* Cache access time for 99%(percentile): 5ms
* Cache access time = latency of hit * 0.99 + latency of miss * 0.01

## Different types of cache usage
1. Write through cache
2. Write back cache
3. Write around cache

### Write through cache
* System writes both to the DB as well as cache
* Used for highly consisent system

### Write around cache
* System writes into DB and not into cache
* Used for weak consistent system
* When data is not avaiable in cache, then inserted into the cache
* In case of cache miss, the request is slower

### Write back cache
* System writes into cache and not into DB, periodically data is updated from cache to DB
* Used for heavy write system

## Horizontal scale policy
* Add a new node with no migration or warming of the cache

## Invalidate cache
[Airbnb](https://recaptech.substack.com/p/recap-himeji-a-scalable-centralized) uses spinaltap which takes change data capture from an RDB and invaldated the cache


## Real world
* Memcached
  * Share nothing architecture
  * Well suited for large sized object
* Redis

## Challenges
* User have two different behaviours (Cache hit and Cache miss)
* Extra added latency for cache miss
