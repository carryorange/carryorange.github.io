# Notes on Caching

## Caching Patterns
* Look-aside caching
  * Read
    * If cache miss
      1. application reads from db
      2. application writes to cache for future reference
  * Write
    * Option 1: application writes to both cache and db
    * Option 2: application writes to db, and invalidates cache
* Inline caching
  * Read
    * If cache miss
      1. Cache reads from db (read through), and caches it
      2. Cache sends result to application
  * Write
    * Write through: application writes to cache, cache write to db **synchrously**
    * Write behind: application writes to cache, cache writes to db **asynchrously**

Summary:
* Look-aside caching: application manages the cache.
  * Primarily used for data that does not change often (so less involved application code to manage the cache)


## Eviction policy
* LRU
* LFU
* ...

## CDN (Content Delivery Network)
Mainly used for serving **static** contents (There are also dynamic content caching technologies)