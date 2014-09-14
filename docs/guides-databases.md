---
layout: 2ColLeft
title: Working with databases
sidebar: guides_nav.html
---

Creating a database
----------

PouchDB databases come in two main flavors: local and remote.

Local databases
--------

To create a local database, you simply call `new PouchDB` and give it a name:

```js
var db = new PouchDB('kittens');
```

You can see a **[live example](http://bl.ocks.org/nolanlawson/bddac54b92c2d8d39241)** of this code.

{% include alert_start.html variant="info" %}

<strong>Protip:</strong> you can download that live example to follow along at home! Just run the following commands:
<p/>
<code>
<br/>git clone https://gist.github.com/bddac54b92c2d8d39241.git kittens
<br/>cd kittens
<br/>python -m SimpleHTTPServer
</code>
<p/>
Now the site is up and running at <a href='http://localhost:8000'>http://localhost:8000</a>.

{% include alert_end.html %}

Remote databases
--------

To create a remote database, you call `new PouchDB` and give it a path to a database in CouchDB/IrisCouch/Cloudant/wherever.

```js
var db = new PouchDB('http://localhost:5984/kittens');
```

If the remote database doesn't exist, then PouchDB will create it for you.

You can verify that your database is working by visiting the URL  [http://localhost:5984/kittens](http://localhost:5984/kittens).  You should see something like this:

```js
{"db_name":"kittens","doc_count":0,"doc_del_count":0,"update_seq":0,"purge_seq":0,"compact_running":false,"disk_size":79,"data_size":0,"instance_start_time":"1410722558431975","disk_format_version":6,"committed_update_seq":0}
```

If instead you see:

```js
{"error":"not_found","reason":"no_db_file"}
```

Then check to make sure that your remote PouchDB has started up correctly. Common errors (such as CORS) are [listed here](/errors.html).

Get basic info about the database
---------

You can se basic information about the database by using the `info()` method.

```js
db.info().then(function (info) {
  console.log(info);
})
```

The local database should show something like:

```js
{"doc_count":0,"update_seq":0,"db_name":"kittens"}
```

The remote database may have a bit more information:

```js
{"db_name":"kittens","doc_count":0,"doc_del_count":0,"update_seq":0,"purge_seq":0,"compact_running":false,"disk_size":79,"data_size":0,"instance_start_time":"1410722558431975","disk_format_version":6,"committed_update_seq":0}
```

The important pieces of information, which you can depend upon in both PouchDB and CouchDB, are:

* `doc_count`: the number of undeleted documents in the database
* `update_seq`: the revision number for the entire database (will be discussed later)
* `db_name`: the name of the database

Debugging your local database
---------

When you create a local PouchDB, you can use the developer tools to see what the database looks like under the hood.

In Chrome, just choose *Overflow icon* &#9776; &#8594; *Tools* &#8594; *Developer Tools*. Then click the *Resources* tab, then *IndexedDB*, and you should see the following:

<img src="/static/img/dev_tools.png" alt="Chrome Developer Tools" style="width:100%"/>

This is the raw IndexedDB representation of your PouchDB, so it may be a little fine-grained compared to what PouchDB shows. However, it's great for quick debugging.

In Safari <8, the `kittens` database will be under *WebSQL* instead of *IndexedDB*.

Differences between the local and remote databases
-------

When you create a local PouchDB database, it uses whatever underlying datastore is available - IndexedDB in most browsers, Web SQL Database in older browsers, and LevelDB in Node.js.

When you create a remote PouchDB database, it communicates directly with the remote database &ndash; CouchDB, Cloudant, Couchbase, etc.

The goal of PouchDB is to allow you to seamlessly communicate with one or the other. You should not notice many differences between the two, except that of course the local one is much faster!

Next
-------

Now that you've created some databases, let's [put some documents in 'em](guides-documents.html)!