# dasconcerto

An enterprise-commerce management system created using Loopback and React. 

## Steps

-----------------------------------------------------------------------
Step 1) Install loopback-cli

==> npm install -g strongloop (optional)
==> npm install -g loopback-cli

-----------------------------------------------------------------------
Step 2) Install connector-mysql or connector-postgresql


==> npm install loopback-connector-mysql --save

or

npm install loopback-connector-postgresql --save

--------------------------------------------------------------------
Step 3) Create the app and specify mysql database for datasource

==> lb

==> lb datasource dasconcertodb --connector mysql

or 

==> lb datasource dasconcertodb --connector postgresql

Ensure that server/datasources.json looks as follows 

for mysql
~~~
{
  "db": {
    "host": "localhost",
    "port": 0,
    "url": "",
    "database": "dasconcertodb",
    "password": "qwerty",
    "name": "db",
    "user": "admin",
    "connector": "mysql"
  }
}
~~~

or for postgresql

~~~
"mydb": {
  "name": "db",
  "connector": "postgresql"
  "host": "localhost",
  "port": 5432,
  "url": "postgres://admin:admin@localhost:5432/dasconcertodb?ssl=false",
  "database": "db1",
  "password": "admin",
  "user": "admin",
  "ssl": false
}
~~~

-----------------------------------------------------------------------

Step 4) Run lb model to create model class for TodoItem 


==> lb model

? Enter the model name: TodoItem
? Select the datasource to attach TodoItem to: db (mysql)
? Select model's base class PersistedModel
? Expose TodoItem via the REST API? Yes
? Custom plural form (used to build REST URL): 
? Common model or server only? common
Let's add some TodoItem properties now.

-----------------------------------------------------------------------

Step 5) Add autoupdate.js migration script under server/boot/autoupdate.js

~~~
module.exports = function(app) {
    var path = require('path');
    var models = require(path.resolve(__dirname, '../model-config.json'));
    var datasources = require(path.resolve(__dirname, '../datasources.json'));

    function autoUpdateAll(){
        Object.keys(models).forEach(function(key) {
            if (typeof models[key].dataSource != 'undefined') {
                if (typeof datasources[models[key].dataSource] != 'undefined') {
                    app.dataSources[models[key].dataSource].autoupdate(key, function (err) {
                        if (err) throw err;
                        console.log('Model ' + key + ' updated');
                    });
                }
            }
        });
    }

    function autoMigrateAll(){
        Object.keys(models).forEach(function(key) {
            if (typeof models[key].dataSource != 'undefined') {
                if (typeof datasources[models[key].dataSource] != 'undefined') {
                    app.dataSources[models[key].dataSource].automigrate(key, function (err) {
                        if (err) throw err;
                        console.log('Model ' + key + ' migrated');
                    });
                }
            }
        });
    }
    //TODO: change to autoUpdateAll when ready for CI deployment to production
    autoMigrateAll();
    //autoUpdateAll();

};
~~~



------------------------------------------------------------------------
Step 6) Start up the server as follows : 


==> node .

------------------------------------------------------------------------
Step 7) Open explorer in browser to view and interact with the API 




------------------------------------------------------------------------
Step 8) Send a curl command to POST to the REST API 


curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \
   "title": "Title", \
   "description": "Desc" \
 }'

------------------------------------------------------------------------
Step 9) Verify that DB has the record for TodoItem

For mysql : 
==> mysql -u admin -h localhost -p


or for postgres : 

==> psql postgres -U admin
psql (9.3.5, server 9.6.6)
WARNING: psql major version 9.3, server major version 9.6.
         Some psql features might not work.
Type "help" for help.

postgres=> \c dasconcertodb;
psql (9.3.5, server 9.6.6)
WARNING: psql major version 9.3, server major version 9.6.
         Some psql features might not work.
You are now connected to database "dasconcertodb" as user "admin".
dasconcertodb=> \dt
          List of relations
 Schema |    Name     | Type  | Owner 
--------+-------------+-------+-------
 public | accesstoken | table | admin
 public | acl         | table | admin
 public | role        | table | admin
 public | rolemapping | table | admin
 public | todoitem    | table | admin
 public | user        | table | admin
(6 rows)

dasconcertodb=> SELECT * FROM todoitem;
 id | title | description 
----+-------+-------------
  1 | todo1 | todo1
  2 | todo2 | todo2
(2 rows)

dasconcertodb=> 


------------------------------------------------------------------------
Step 10) Create a client_src folder and start app in that folder


==> mkdir client_src
==> cd client_src

------------------------------------------------------------------------
Step 11) Install create-react-app 
==> sudo npm install -g create-react-app
==> create-react-app .



------------------------------------------------------------------------
Step 12) Cleanup



------------------------------------------------------------------------
Step 13) Install router 


==> npm install --save react-router react-router-dom


------------------------------------------------------------------------


------------------------------------------------------------------------
## Optional

To have launchd start mongodb now and restart at login:
  brew services start mongodb

Or, if you don't want/need a background service you can just run:
  mongod --config /usr/local/etc/mongod.conf


## Useful Links

https://www.raymondcamdencom/2016/04/27/loopback-strongloop-and-api-connect-how-in-the-heck-do-they-relate/


https://github.com/strongloop/loopback-connector-postgresql

https://www.codementor.io/engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb



# Postgres commands

==> psql postgres -U admin
psql (9.3.5, server 9.6.6)
WARNING: psql major version 9.3, server major version 9.6.
         Some psql features might not work.
Type "help" for help.

postgres=> \c dasconcertodb;
psql (9.3.5, server 9.6.6)
WARNING: psql major version 9.3, server major version 9.6.
         Some psql features might not work.
You are now connected to database "dasconcertodb" as user "admin".
dasconcertodb=> \dt
          List of relations
 Schema |    Name     | Type  | Owner 
--------+-------------+-------+-------
 public | accesstoken | table | admin
 public | acl         | table | admin
 public | role        | table | admin
 public | rolemapping | table | admin
 public | todoitem    | table | admin
 public | user        | table | admin
(6 rows)

dasconcertodb=> SELECT * FROM todoitem;
 id | title | description 
----+-------+-------------
  1 | todo1 | todo1
  2 | todo2 | todo2
(2 rows)

dasconcertodb=> 




# Version Management

==> node -v
v7.7.2
~/repos/ad/githubrepos/dasconcerto (master) 
==> npm -v
4.1.2
~/repos/ad/githubrepos/dasconcerto (master) 
==> git --version
git version 2.16.1