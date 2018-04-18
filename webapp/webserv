#!/usr/bin/env python3
"""
Simple search UI for PostgreSQL full-text search
"""

import json
import flask
import os
import urllib.parse, urllib.request
from jinja2 import Environment, FileSystemLoader

JSON_HOST = "http://localhost:8001" # Where the restserv service is running

env = Environment(loader=FileSystemLoader(searchpath="%s/templates" % os.path.dirname((os.path.realpath(__file__)))), trim_blocks=True)
app = flask.Flask(__name__)

@app.route("/")
def index():
    """Serve up the basic search page"""
    template = env.get_template('index.html')
    return(template.render())

@app.route("/search")
def search():
    """Simple search for terms, with optional limit and paging"""
    query = flask.request.args.get('query', '')
    page = flask.request.args.get('page', '')
    jsonu = u"%s/search/%s/" % (JSON_HOST, urllib.parse.quote_plus(query.encode('utf-8')))
    if page:
        jsonu = u"%s%d" % (jsonu, int(page))
    res = json.loads(urllib.request.urlopen(jsonu).read().decode('utf-8'))
    template = env.get_template('results.html')
    return(template.render(
        terms=res['query'].replace('+', ' '),
        results=res,
        request=flask.request
    ))

if __name__ == "__main__":
    app.debug = True
    print(JSON_HOST)
    app.run()
