Installation
------------
`websql.Source` is installed into your project's lib directory, much like
any other Enyo plugin, for example `layout` or `onyx`. [See this post in Enyo Wiki for details](https://github.com/enyojs/enyo/wiki/Managing-Your-Project).

Generally, you want to add `websql.Source` plugin as a git submodule of your
project, placed under `lib/indexeddb` directory.

One way to do that is to run the following at your project's root directory:

    git submodule add https://github.com/vtomilin/EnyoWebSQLSource.git lib/websql


Then you should add `websql.Source` dependency to your top-level `package.js`
file:

```javascript
enyo.depends(
    '$lib/layout',
    '$lib/onyx',
    '$lib/websql',
    // Other dependencies...
);
```

Usage
-----
This code is to be run when application instance is initialized.
```javascript
try {
    var webSqlSrc = new websql.Source({
        dbName: 'food',
        dbSchema: MyApp.Resources.schema,
        dbSchemaDidUpdate: function(e) {
            if(e) {
                enyo.error('Failed to create database schema:', e);
                
                return;
            }
            
            enyo.store.addSources({sql: webSqlSrc});
        }
    });
} catch(e) {
    enyo.error(e && e.toString());
}
```

This will create(or open if it already exists) a database 'food'
using SQL statements, provided in MyApp.Resources.schema string.

In this example, MyApp.Resources is a singleton, containing application
resources, such as database schema SQL statements.

Once schema is created/updated, in this particular example, a special
global 'onWebSQLDbReady' signal is sent such that the other parts of
the app could make use of the data in the database.

You can now save and retrieve your models using WebSQLSource source,
registered under 'sql' name in this particular example. Further, in
the following example, involving retrieving models from the database,
it is assumed there is a table 'diet' in the database. The 'table'
property indicates the table, which Collection is pupulated from.
Note that you could use 'url' property instead for that same purpose
as you subclass Collection and Model as well. The 'url' property
could be your usual url, i.e. 'http://myserver.com/data' as long
as 'data' at the end of the url is the database table where your
'data' is.

```javascript
var data = new Collection();
data.fetch({
    table: 'diet',
    source: 'sql'
});
```

this should work also:

```javascript
var data = new Collection();
data.fetch({
    url: 'diet',
    source: 'sql'
});
```

so as this

```javascript
enyo.kind({
    name: 'MyDiet',
    kind: 'enyo.Collection',
    url: 'http://w8management.com/diet'
});

var diets = new MyDiet();
diets.fetch({source: 'sql'});
```

Other Enyo Model/Collection CRUD methods work similarly.


[See WebSQL spec for details](http://dev.w3.org/html5/webdatabase/#database)