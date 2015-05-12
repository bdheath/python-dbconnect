# python-dbconnect

Python wrapper for MySQL connections with MySQLdb. Simplifies connections and queries; adds logging, debugging and caching.

This wrapper abstracts commonly used MySQLdb functions

For example, a simple query becomes:
~~~
import dbconnect

db = dbconnect.db()
names = db.getDict("SELECT * FROM names WHERE type = %s", (person,))
~~~
