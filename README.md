SQLiteasy Docs: Forget the endless and repetitive SQLite code. Embrace simplicity and efficiency with SQLiteasy.
----
Creating a database<br>
<pre>
from sqliteasy import SQLiteasy
 
entries_to_test = {
    "user_name": {"type": "string", "notnull": True},
    "user_email": {"type": "string", "notnull": True},
    "user_isregistered": {"type": "boolean", "notnull": False}
 }
 
database = SQLiteasy("newdb", entries_to_test)
database.create_database()
</pre>
---
Inserting a new value with SQLiteasy:<br>
<pre>
 database.insert_database("user_name"="New_user", "user_email"="newemail@gmail.com", "user_isregistered"=False)
</pre>
Inserting a new value with just sqlite3 Python default library:<br>
<pre>
conn = sqlite3.connect("newdb.db")
cursor = conn.cursor()
cursor.execute("""
INSERT INTO users (user_name, user_email, user_isregistered)
VALUES (?, ?, ?)
""", ("New_user", "newemail@gmail.com", False))
conn.commit()
conn.close()
</pre>



Check to docs: https://gorgeous-parfait-3d24cd.netlify.app/
 
