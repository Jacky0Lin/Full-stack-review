[TOC]

## Quick Link

Mongoose: https://mongoosejs.com/docs/models.html

Mysql2: https://www.npmjs.com/package/mysql2

React: https://reactjs.org/docs/getting-started.html

Express: https://expressjs.com/en/4x/api.html

## Module export

### In Node.js 

Use  [CommonJS (CJS)](https://en.wikipedia.org/wiki/CommonJS)  module formats

```js
module.exports = {your_modules}; 
// or exports = {your_modules}

const module_YouNeed = required('path OR lib')
```

### In React

use [ES Module (ESM)](https://www.sitepoint.com/understanding-es6-modules/) format

```js
import module_YouNeed from 'path OR lib';

export default your_module;
```



## Terminal (for testing)

### Mongoose

```shell
$mongosh OR $mongo     // two appoarch to run
$show databases
$use dbname
$show tables
$db.dbname.find({})		 //  find all
$db.dbname.remove({})  // remove all
```

==Search==: `db.collection.find()`

Remove: 

### Mysql

```shell
$mysql -u root < fileName.sql`  -- execute .sql file in vscode
$mysql -u root;
$mysql> show databases;
$mysql> use dbname;
$mysql> show tables;
$mysql> describe tbname;
$mysql> select * from tbname;
$mysql> drop database dbname;
```

## Database

### Mysql2 & query

SETUP - 1 :
Execute SQL in vscode to create database & table   :`$mysql -u root < fileName.sql`

```js
const mysql = require("mysql2");

const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: '',
  database: dbname,
});

// optional
// const db = Promise.promisifyAll(connection, { multiArgs: true });

// simple query
connection.query(YOUR_SQL, (err, results) => {})

// with placeholder
connection.query(
	`SELECT * FROM tb_Name WHERE name = ? AND age = ?`,
  ['Page', 45],
  function(err, results) {
    console.log(results);
  }
)
```

SETUP - 2:

Create Schema in server

```js
const mysql = require("mysql2");
const Promise = require("bluebird");

// Configure process.env variables in ../.env
const connection = mysql.createConnection({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME,
});

const db = Promise.promisifyAll(connection, { multiArgs: true });

db.connectAsync()
  .then(() => console.log(`Connected to MySQL as id: ${db.threadId}`))
  .then(() =>
    // Expand this table definition as needed:
    db.queryAsync(
      "CREATE TABLE IF NOT EXISTS responses (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY)"
    )
  )
  .catch((err) => console.log(err));

module.exports = db;
```



#### Mysql create table

```sql
CREATE DATABASE dbname;
USE dbname;
CREATE TABLE tbName (
  id int not null auto_increment,
  primary key (id),
  text varchar(255),
  -- FOREIGN KEY (your_id) REFERENCES foreignTb(foreign_ID)
);
```

#### Mysql Insert

```sql
INSERT INTO tbName (col1, col2, col3) VALUES(val1, val2, val3);
```

#### Mysql update

```sql
UPDATE tableName SET anAttribute = 'Something' WHERE anOtherAttribute = 'SomethingElse'
```

#### Mysql delete

```sql
DELETE FROM table_name
WHERE some_column = some_value 
```



### MongoDB

Setup Link: https://mongoosejs.com/docs/models.html

#### Mongoose setup & query

```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/[dbname]');
let repoSchema = mongoose.Schema({
  id: Number,
});

// .model() function makes a copy of schema
// and compiles a model for you.
let model_Name = mongoose.model('model_Name', repoSchema);

model_Name.create(doc, ) // inerst a doc into mongoDB
/**
* Any Query here..
* model_Name.QUERY_FUNC()
**/
module.exports.save = save;
```

For more Query: https://mongoosejs.com/docs/queries.html

#### Insert

##### Insert one doc - save()

```js
let model_Name = mongoose.model('model_Name', repoSchema);

// Insert a Doc
const model_instance = new model_Name(doc);

model_instance.save(function (err) {
  if (err) return handleError(err);
  // saved!
});
```

> The `.save()` is an instance method of the model, while the `.create()` is called directly from the `Model` as a method call, being static in nature, and takes the object as a first parameter.
>
> Stack overflow: https://stackoverflow.com/questions/38290684/mongoose-save-vs-insert-vs-create



##### Insert one doc - create()

```js
let model_Name = mongoose.model('model_Name', repoSchema);

model_Name.create(doc, function (err, small) {
  if (err) return handleError(err);
  // saved!
});
```

##### Insert many doc - insertMany()

insertMany is a method from **MongoDB**, not **Mongoose**

```js
let model_Name = mongoose.model('model_Name', repoSchema);

model_Name.insertMany([doc1, doc2, doc3....], function(err) {

});
```



#### Querying (find)

```js
// Find All
MyModel.find({});

// Find based on Condition
// find name john and age at least 18
MyModel.find({ name: 'john', age: { $gte: 18 }}, function (err, docs) {});
```



#### Delete

```js
// delete one
MyModel.deleteOne(doc, (err,...) => {} );
// delete many
MyModel.deleteMany(doc, (err,...) => {} );

// deprecated function
//MyModel.remove(doc, (err, results) => {} )
```



#### update (To be added....)





## Server

Ajax 

Contenttype:

body-parser:

Express.json()

axios

## Ajax (jQuery)

```js
// dataType: type of data from server
// contentType: type of data sending to server
import $ from 'jquery';

$.ajax({
        type: "POST",
        url: "/api/saveProfile",
        data: JSON.stringify(aData),
        contentType: "application/json",
            success: function(){
               alert("Request was successful");
            }
        });
```

## Express

```js
const express = require('express');
const path = require('path');

// 4.3
app.use(express.json());

//
var bodyParser = require('body-parser');
var urlencodedParser = bodyParser.urlencoded({ extended: true });
app.use(urlencodedParser);
app.use(bodyParser.json());

```












