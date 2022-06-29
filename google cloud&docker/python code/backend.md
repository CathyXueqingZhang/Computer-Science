Packages
```Python
# RESTful API
import json
import sqlite3
import urllib
from flask import Flask, g, request, jsonify, Response
```

### Initialization
the data service through a typical web service (API) instead of supporting directly by a database. We read data from Json instead of directly from the db.
```Python
DATABASE = 'todolist.db'

app = Flask(__name__)
app.config.from_object(__name__)
```

### Functions
Get, Post,Delete and Put
```Python
@app.route("/api/items")  # default method is GET
def get_items(): # this is the counterpart of show_list() from homework 3
    db = get_db()
    cur = db.execute('SELECT what_to_do, due_date, status FROM entries')
    entries = cur.fetchall()
    tdlist = [dict(what_to_do=row[0], due_date=row[1], status=row[2])
              for row in entries]
    response = Response(json.dumps(tdlist),  mimetype='application/json')
    return response
  
  @app.route("/api/items", methods=['POST'])
def add_item(): # this is the counterpart of add_entry() from homework 3
    db = get_db()
    db.execute('insert into entries (what_to_do, due_date) values (?, ?)',
               [request.json['what_to_do'], request.json['due_date']])
    db.commit()
    return jsonify({"result": True})


@app.route("/api/items/<item>", methods=['DELETE'])
def delete_item(item): # this is the counterpart of delete_entry() from homework 3
    item = urllib.parse.unquote(item) #remove space
    db = get_db()
    db.execute("DELETE FROM entries WHERE what_to_do='"+item+"'")
    db.commit()
    return jsonify({"result": True})


@app.route("/api/items/<item>", methods=['PUT'])
def update_item(item): # this is the counterpart of mark_as_done() from homework 3
    item = urllib.parse.unquote(item) #remove space
    db = get_db()
    db.execute("UPDATE entries SET status='done' WHERE what_to_do='"+item+"'")
    db.commit()
    return jsonify({"result": True})
```
### Get Database and run applications
```Python
def get_db():
    """Opens a new database connection if there is none yet for the
    current application context.
    """
    if not hasattr(g, 'sqlite_db'):
        g.sqlite_db = sqlite3.connect(app.config['DATABASE'])
    return g.sqlite_db


@app.teardown_appcontext
def close_db(error):
    """Closes the database again at the end of the request."""
    if hasattr(g, 'sqlite_db'):
        g.sqlite_db.close()


if __name__ == "__main__":
    app.run("0.0.0.0", port=6000)
```

