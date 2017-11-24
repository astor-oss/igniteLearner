# Near cache

Near caches are transparent for you - there is no user API to get reference to or work with near caches. The purpose of a near cache is to "cache the cache", that is to be a local cache for a distributed cache. Ignite transparently fills near cache with the most recent data when you work with the underlying distributed cache. That also means you should not use near cache if you use co-located \(server-side\) data access/computations since near cache is local by definition.

  


There are two kinds of API to create a near cache:

1. Create near cache for existing distributed cache: ignite.
   getOrCreateNearCache\(
   existnigDistibutedCacheName, nearCacheConfiguration\). This will return the exiting distributed cache and start a near cache for it or fail if the distributed cache does not exist.
 
2. Create a new distributed cache with near cache:  ignite.createCache\(
   newDistibutedCacheName, nearCacheConfiguration\)
 
3. ignite.getOrCreateCache\(
   distibutedCacheName, nearCacheConfiguration\) - this behaves like option 1 or 2 depending on whether the distributed cache exists.
 

Thus, answering your question - use any of the API above to create a near cache for "TableA". for example:

  


Cache

&lt;

?, ?

&gt;

 tableA = ignite.getOrCreateCache\("

TableA", new NearCacheConfiguration\)\);

  




