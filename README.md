# python-dbconnect

Python wrapper for MySQL connections with MySQLdb. Simplifies connections and queries; adds logging, debugging and caching.

This wrapper abstracts commonly used MySQLdb functions

For example, a simple query becomes:
~~~python
import dbconnect

db = dbconnect.db()
names = db.getDict("SELECT * FROM names WHERE type = %s", (person,))
~~~

The getDict() method returns a tuple of dictionaries, with one dictionary per row of data that matched the query. You can also return a single value using getOne():

~~~python
name = db.getOne("SELECT name FROM names LIMIT 1")
~~~

To add add caching to the query:

~~~python
db = dbconnect.db()
db.cache(60)                                  # Cache TTL in seconds
names = db.getDict("SELECT * FROM names WHERE type = %s", (person,))
print db.cacheUsed()                          # Return True if the query delivered cached results
~~~

You can do INSERT, REPLACE, UPDATE and DELETE queries with the run() method:

~~~python
names = ['Joe', 'John']
db.run("INSERT INTO names(name) VALUES(%s)", (name[0], ))     # Execute one query
db.runMany("INSERT INTO names(name) VALUES(%s)", names))      # Execute query for a list of values
~~~

Need the primary key ID for the last value you just inserted?

~~~python
insert_id = db.lastInsertID()
~~~

To add some basic query logging and debugging:

~~~python
db = dbconnect.db()
db.logSlowQueries(t = 10)     # Log queries that take more than t seconds
db.logErrors()                # Also log any errors in your query
db.setLogFile('query.log')    # Set the log file (default is /queries.log)
~~~

If you want to do something sort of reckless, you can ignore MySQL errors (with or without logging them):
~~~python
db.ignoreErrors()
~~~

And you can use the caching layer for caching other parts of your script, too:
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
