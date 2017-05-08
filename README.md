# learnyoumongo

https://github.com/evanlucas/learnyoumongo

### CONNECT (Exercise 2 of 9)
Start mongod on port `27017` with data as the `dbpath`
```shell
$ mongod --port 27017 --dbpath=./data
```
### FIND (Exercise 3 of 9)
Here we will learn how to search for documents.

In this exercise the database name is `learnyoumongo`.
So, the url would be something like: `mongodb://localhost:27017/learnyoumongo`

Use the `parrots` collection to find all documents where `age`
is greater than the first argument passed to your script.
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = "mongodb://localhost:27017/learnyoumongo"
const age = process.argv[2];

mongo.connect(url, (err, db) => {
  handleError(err);
  const parrots = db.collection('parrots');

  parrots.find().toArray((err, docs) => {
    handleError(err);

    const fil = docs.filter(list => list.age > age);
    console.log(fil);
    db.close();
  })
})
```
### FIND PROJECT (Exercise 4 of 9)
Here we will learn how to search for documents but only fetch the fields we need. Also known as **projection** in MongoDB
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = "mongodb://localhost:27017/learnyoumongo"
const age = process.argv[2];

mongo.connect(url, (err, db) => {
  handleError(err);
  const parrots = db.collection('parrots');

  parrots.find({
    age: {
      $gt: +age
    }
  }, {
    name: 1
  , age: 1
  , _id: 0
  }).toArray((err, docs) => {
    handleError(err);
    console.log(docs)
    db.close();
  })
})
```
### INSERT (Exercise 5 of 9)
Connect to MongoDB on port `27017`.
You should connect to the database named `learnyoumongo` and insert
a document into the `docs` collection.
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = "mongodb://localhost:27017/learnyoumongo"

const firstName = process.argv[2];
const lastName = process.argv[3];
const fullName = {firstName, lastName}

mongo.connect(url, (err, db) => {
  handleError(err);
  const collection = db.collection('docs')

  collection.insert(fullName, (err, data) => {
    handleError(err);
    console.log(JSON.stringify(fullName));
    db.close()
  })
})
```
### UPDATE (Exercise 6 of 9)
Here we are going to update a document in the `users` collection.
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = `mongodb://localhost:27017/${process.argv[2]}`

mongo.connect(url, (err, db) => {
  handleError(err);
  const collection = db.collection('users');

  collection.update({
    username: "tinatime"
  }, {
    $set: {
      age: 40
    }
  }, err => {
    handleError(err);
    db.close();
    }
  )  
})
```
### REMOVE (Exercise 7 of 9)
This lesson involves removing a document with the given `_id`.
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = `mongodb://localhost:27017/${process.argv[2]}`

mongo.connect(url, (err, db) => {
  handleError(err);
  const collection = db.collection(process.argv[3]);
  const _id = process.argv[4];

  collection.remove({ _id }, err => {
    handleError(err);
    db.close();
  })
})
```
### COUNT (Exercise 8 of 9)
Here we will learn how to count the number of documents that meet certain criteria.
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = `mongodb://localhost:27017/learnyoumongo`

mongo.connect(url, (err, db) => {
  handleError(err);
  const collection = db.collection('parrots');
  const age = process.argv[2];

  collection.count({
    age: {
      $gt: +age
    }
  }, (err, count) => {
    handleError(err);
    console.log(count);
    db.close();
  })
})
```
### AGGREGATE (Exercise 9 of 9)
Next up is aggregation. Aggregation allows one to do things like calculate the sum of a field of multiple documents or the average of a field of documents meeting particular criteria.
```javascript
const mongo = require('mongodb').MongoClient
const handleError = (err) => err && console.error(err);

const url = `mongodb://localhost:27017/learnyoumongo`
const size = process.argv[2]

mongo.connect(url, (err, db) => {
  handleError(err);
  const prices = db.collection('prices')
  prices.aggregate([
    { $match: {
      size: size
    }}
  , { $group: {
      _id: 'average'
    , average: {
        $avg: '$price'
      }
    }}
  ]).toArray((err, results) => {
    handleError(err);

    console.log(Number(results[0].average).toFixed(2))
    db.close()
  })
})
```
