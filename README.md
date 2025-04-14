SQLiteasy Docs
Forget the endless and repetitive SQLite code. Embrace simplicity and efficiency with SQLiteasy.

Check to docs: 

First step

To start using SQLiteasy you first need to create an sqlite database, to do so, start by

from SQLiteasy import SQLiteasy
database = SQLiteasy("test", entries)

First import the class SQLiteasy, add the database name without the .db then add the dict for the entries parameter (second one), that parameter is a dict, in the following syntax:

entries = {
    "user_name": {"type": "string", "notnull": True},
    "user_email": {"type": "string", "notnull": True},
    "user_isregistered": {"type": "boolean", "notnull": False},
}

Where "notnull" means if the database should say ex: "user_name TEXT NOT NULL", or if should it say "user_age INTEGER"
Create the database

Once you have set the database name, and the entries dictionary, create the data with create_database()

database.create_database()

Finally, everything should look like this:

from SQLiteasy import SQLiteasy
database = SQLiteasy("test", entries)
entries = {
    "user_name": {"type": "string", "notnull": True},
    "user_email": {"type": "string", "notnull": True},
    "user_isregistered": {"type": "boolean", "notnull": False},
}
database.create_database()

Columns (entries) supported

For the entries dictionary, not only "string" and "boolean" is supported as you see on the example. SQLiteasy supports every entrie value that works with sqlite
String value: {"type": "string", ...}
Integer value: {"type": "integer", ...}
Float value: {"type": "float", ...}
Boolean value: {"type": "boolean", ...}
Binary value: {"type": "binary", ...}

    In SQLite, the "float" type is stored as "REAL". For "boolean", SQLite interprets the value as true or false, but ultimately stores it as an "INTEGER": 1 for true and 0 for false. The "binary" type in SQLite is stored as "BLOB" (Binary Large Object). 

Inserting data

Inserting data with SQLiteasy is really easy, once the database is created and the entries dictionary is made, just use insert_database():

from SQLiteasy import SQLiteasy
entries = {
    "user_name": {"type": "string", "notnull": True},
    "user_email": {"type": "string", "notnull": True},
    "user_isregistered": {"type": "boolean", "notnull": False},
}
database = SQLiteasy("test_database", entries)
database.create_database()
database.insert_database(user_name="Josue", user_email="user@gmail.com", user_isregistered=True)

    Important: If you define 3 entry fields—e.g., "user_name", "user_email", and "user_isregistered"—you must provide the same number of values when inserting data. Make sure to pass the parameters for the entries with the names exactly as they were defined. For example: database.insert_database(user_name="Josue", user_email="user@gmail.com", user_isregistered=True). 

Select every row

To select all the data (SELECT * FROM .db), use fetch_database(indent=bool) method.

data = database.fetch_database(indent=bool)

indent=bool is set on True by default. What it does is to print() the data in a best and legible way. You can set it on False, but the print() may be unlegible with all the data. fetch_database() prints the info and return the data selected
Select a row that matches a condition

To select a row that matches a specify condition (SELECT * FROM .db WHERE user_email = ?), use fetch_database_by() method.

user1 = database.fetch_database_by(where="user_email", by="user1@gmail.com", indent=bool)
-- or --
user1 = database.fetch_database_by("user_email", "user1@gmail.com", indent=bool)

This will select the row that "user_email" is "user1@gmail.com"
Select a row that matches two exact conditions

To select a row that matches two exact conditions (SELECT * FROM .db WHERE user_email = ? AND user_isregistered = ?), use fetch_database_by_and() method.

user1 = database.fetch_database_by_and("user_email", "user1@gmail.com", "user_name", "Josue")
-- or --
user1 = database.fetch_database_by_and(where="user_email", by="user1@gmail.com", and_where="user_name", and_by="Josue")

This will select the row that "user_email" is "user1@gmail.com" and "user_name" is "Josue", if one of the conditions is False, there will not be a return, if you want to select by one or two conditions use fetch_database_by_or()
Select a row that matches one or two conditions

To select a row that matches one or two conditions (SELECT * FROM .db WHERE user_email = ? OR user_isregistered = ?), use fetch_database_by_or() method.

user 1 = database.fetch_database_by_or("user_email", "user1@gmail.com", "user_name", "DontMatch")
-- or --
user1 = database.fetch_database_by_or(where="user_email", by="user1@gmail.com", and_where="user_name", and_by="DontMatch")

Update a value in a specific row

To update a value in a specific row, use update_database() method.

database.update_database(column_to_update="user_name", condition_column="user_name", condition_value="josue", new_value="newname")

It is more easy than it seems, column_to_update is the column that we want to update, condition_column is the column where we will look to match a condition, condition_value is the condition that will match the value on the column condition_column, and new_value is the new value that will be updated if the condition was met
Compare SQLiteasy vs SQLite
Creating the database
With SQLite:

import sqlite3
conn = sqlite3.connect("test_database.db")
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS test_table (
"user_name" TEXT NOT NULL,
"user_email" TEXT NOT NULL,
"user_isregistered" INTEGER DEFAULT 0
)''')
conn.commit()
conn.close()

With SQLiteasy:

from SQLiteasy import SQLiteasy
entries = {
"user_name": {"type": "string", "notnull": True},
"user_email": {"type": "string", "notnull": True},
"user_isregistered": {"type": "boolean", "notnull": False},
}
database = SQLiteasy("test_database", entries)
database.create_database()

That looks much more clear and uses fewer lines of code.
Selecting all the data from a .db
With SQLite:

import sqlite3
conn = sqlite3.connect("test_database.db")
c = conn.cursor()
c.execute("SELECT * FROM test_table")
rows = c.fetchall()
for row in rows:
    print(f"\n{row}")
conn.close()

With SQLiteasy:

from SQLiteasy import SQLiteasy
data = database.fetch_database(indent=True)

With SQLiteasy, you can just call the method fetch_database() and it will return all the data from the database.
Selecting a row that matches a condition
With SQLite:

import sqlite3
conn = sqlite3.connect("test_database.db")
c = conn.cursor()
c.execute("SELECT * FROM test_table WHERE = ?", (value,))
rows = c.fetchall()
for row in rows:
    print(f"\n{row}")
conn.close()

With SQLiteasy:

from SQLiteasy import SQLiteasy
user1 = database.fetch_database_by("user_email", "user1@gmail.com", indent=True)

With SQLiteasy, you can just call the method fetch_database_by() and it will return the data that matches the condition.
