Package<br/>

```Python
from flask import Flask, render_template, redirect, g, request, url_for, jsonify, json
import urllib
import requests  # similar purpose to urllib.request, just more convenient
import os
```
<br/>leave the application front-end and overall structure unchanged, but connect with backend by using API URL

```Python
app = Flask(__name__)
# make sure to replace localhost with the actual IP of the backend service after you deploy the backend service on Google Cloud
# for example, like this: TODO_API_URL = "http://123.456.789.123:6000"
#TODO_API_URL = "http://localhost:6000"

TODO_API_URL = "http://" + os.environ['TODO_API_IP'] + ":6000"
#the IP will change in the cloud, os.environ will help to get correct IP
```
<br/> Shows all item form old list. POST, Delete and mark as done function

```Python
@app.route("/")
def show_list(): # this is the counterpart of show_list() from homework 3
    resp = requests.get(TODO_API_URL+"/api/items")
    resp = resp.json()
    return render_template('index.html', todolist=resp)
    
@app.route("/add", methods=['POST'])
def add_entry(): # this is the counterpart of add_entry() from homework 3
    requests.post(TODO_API_URL+"/api/items", json={
                  "what_to_do": request.form['what_to_do'], "due_date": request.form['due_date']})
    return redirect(url_for('show_list'))


@app.route("/delete/<item>")
def delete_entry(item): # this is the counterpart of delete_entry(...) from homework 3
    item = urllib.parse.quote(item) # this takes care of spaces in the item
    requests.delete(TODO_API_URL+"/api/items"+item)
    return redirect(url_for('show_list'))



@app.route("/mark/<item>")
def mark_as_done(item): # this is the counterpart of mark_as_done(...) from homework 3
    item = urllib.parse.quote(item)
    requests.put(TODO_API_URL+"/api/items"+item)
    return redirect(url_for('show_list'))
```

#### Running the application
```Python
if __name__ == "__main__":
    app.run("0.0.0.0")
```
