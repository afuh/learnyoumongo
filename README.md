# learnyoumongo

https://github.com/evanlucas/learnyoumongo

### CONNECT (Exercise 2 of 9)
Start mongod on port `27017` with data as the `dbpath`
```shell
mongod --port 27017 --dbpath=./data
```
### FIND (Exercise 3 of 9)
Here we will learn how to search for documents.

In this exercise the database name is `learnyoumongo`.
So, the url would be something like: `mongodb://localhost:27017/learnyoumongo`

Use the `parrots` collection to find all documents where `age`
is greater than the first argument passed to your script.
```javascript
const mongo = require('mongodb').MongoClient

const url = "mongodb://localhost:27017/learnyoumongo"
const age = process.argv[2];

mongo.connect(url, (err, db) => {
  if (err) console.error(err);
  const parrots = db.collection('parrots');

  parrots.find().toArray((err, docs) => {
    if (err) console.error(err);

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

const url = "mongodb://localhost:27017/learnyoumongo"
const age = process.argv[2];

mongo.connect(url, (err, db) => {
  if (err) console.error(err);
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
    if (err) console.error(err);
    console.log(docs)
    db.close();
  })
})
```
