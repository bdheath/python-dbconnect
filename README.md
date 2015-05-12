# python-dbconnect

Python wrapper for MySQL connections with MySQLdb. Simplifies connections and queries; adds logging, debugging and caching.

This wrapper abstracts commonly used MySQLdb functions

For example, a simple query becomes:
~~~python
import dbconnect

db = dbconnect.db()
names = db.getDict("SELECT * FROM names WHERE type = %s", (person,))
~~~

To add add caching to the query:

~~~python
db = dbconnect.db()
db.cache(60)            # Cache TTL in seconds
names = db.getDict("SELECT * FROM names WHERE type = %s", (person,))
print db.cacheUsed()    # Return True if the query delivered cached results
~~~

To add some basic query logging and debugging:

~~~python
db = dbconnect.db()
db.logSlowQueries(t = 10)     # Log queries that take more than t seconds
db.logErrors()                # Also log any errors in your query
db.setLogFile('query.log')    # Set the log file (default is /queries.log)
~~~

And you can use the caching layer for other things:
~~~python
import object_cache

cache = object_cache.objectCache(60)            # Cache TTL

if cache.read('key'):                           # Returns False if there is no non-expired cached value
  print cache.content()                         # Returns the cached content
else:
  output = 'Now this will go in the cache'
  print output
  cache.write(output)                           # Save this to the cache

~~~
