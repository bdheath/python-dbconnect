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
db.cache(60)    # Cache TTL in seconds
names = db.getDict("SELECT * FROM names WHERE type = %s", (person,))
print db.cacheUsed()    # Return True if the query delivered cached results
'''
