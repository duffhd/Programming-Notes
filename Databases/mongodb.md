# MongoDB Cheatsheet

## Database

```js
// List databases
show databases
show dbs

// Enter a database
use database_example

// Show current database
db

// Show the collections inside database_example
show collections

// Delete a collection
db.collection_name.drop()
```

## Find

```js
// 'db.' automatically references the database you entered with 'use'

// Find everything under a specific collection that satisfies this query
db.collection_name.find() # Returns everything
db.collection_name.find({"name": "alex"})

// Count the number of documents returned by find
db.collection_name.find({"name": "alex"}).count()

// Output the query result in a more human redable style
db.collection_name.find({"name": "alex"}).pretty()
```

## Count

```js
// Count all elements from the collection       
db.collection_name.estimatedDocumentCount()  // estimation based on collection metadata
db.collection_name.countDocuments() // accurate count

// Count elements from a query
db.collection_name.count({"name": "Alex"})
db.collection_name.countDocuments({"name": "Alex"})
```

## Insert

```js
// Insert values
db.collection_name.insertOne({"name": "paul"}) // Single document
db.collection_name.insertMany([{"name": "leto"}, {"name": "alisson"}]) // Array

// If ordered is true, if an error occurs in one document, the insertion will be canceled
// and other documents that are after the current will not be evaluated and inserted.
// If ordered is false, the insertion will not cancel and add every document that is correct.
db.collection_name.insertMany([{"name": "leto"}, {"name": "alisson"}], {"ordered": false})
```

## Delete

```js
db.collection_name.deleteMany({}) // Deletes all documents from the collection
db.collection_name.deleteMany({"name": "Alex"}) // Deletes all documents returned by this query

// Delete only one documented contained in this query
db.collection_name.deleteOne({"name": "Alex"})

// Returns the document and delete it
db.collection_name.findOneAndDelete({"name": "Max"}) 
```

## Update

```js
// Change a value with $set
db.collection_name.updateOne({"_id": 1}, {$set: {"name": "Alice"}})

// Increment a value with $inc
db.collection_name.updateOne({"_id": 1}, {$inc: {"age": 10"}})
```

## Query operators

$eq  = equal to

$ne  = not equal to

$gt  = greater then

$lt  = lesser then

$gte = greater then or equal to

$lte = lesser then or equal to

```js
// Issue a query with an operator
db.collection_name.find({"length": {$gt: 100}})
db.collection_name.find({"grade": {$gt: 60, $lt: 100}})
```

## Logic Operators

$and matches all given clauses

$or  matches at least one clause

$nor fails to match given clauses

$not negates the query

```js
// $and is mot of the times implicit
db.collection_name.find({"major": "CS", "grade": "100"})

// $or, $nor and $and are all used with arrays []
// Returns every document which contains city CLEVELAND or ALPINE
db.collection_name.find({$or: [ {"city": "CLEVELAND"}, {"city": "ALPINE"} ]})

// Return every city that is not CLEVELAND or ALPINE
db.collection_name.find({$nor: [ {"city": "CLEVELAND"}, {"city": "ALPINE"} ]})

// Example nested $or query
db.collection_name.find({$or: [
    {$or: [{"category_code": "social"}, {"category_code": "web"}], "founded_year": 2004}, 
    {$or: [{"category_code": "social"}, {"category_code": "web"}], "founded_month": 10}
]}).count()
```

## Expr

$expr operator works by comparing values from a given operator

```js
// The use of $ on the document fields return the value of each key
// Here we are comparing that birth_city equals current_living inside each document
// The query will return every document that matches the said condition
db.collection_name.find({$expr: {$eq: ["$birth_city", "$current_living"]}}).count()
```

## Array operations

```js
// When querying a list you can search for a maximum size and number of certain values
db.collection_name.find({"cities": {$size: 20, $all: ["Montreal", "Vancourver"]}})

// To queery an object field that is insde of an array you can use $elemMatch
// Here "offices" is an array with objects that describes the city in which a company is located
// This query will only search for documents that contain "Seattle" in one of the objects of the "offices" array
db.collection_name.find({"offices": {$elemMatch: {"city": "Seattle"}}}).count()
```

## Sub-documents

Sub-documents are custom objects within documents. To query each field we can use dot notation

```js
// Here we have a custom object named "milestones" which has a field "stoned_year"
// we can access it with "milestones.stone_year" and do query operations as with any field
db.collection_name.find({"milestones.stoned_year": {$gt: 2012}}).count()

// You can access an array by its index inside a sub-document
// Here we query the 0 index of "coordinates" array
 db.collection_name.find({"start station location.coordinates.0": {$lt: -74}})
```

## Projection

Projection is used when you want to filter some of the fields from the document. 
After a query is done only the fields you specified will be shown on screen.

```js
// {"items": 1} is the second parameter of find()
// 1 means that only the field "items" will be displayed (alongside with the _id which is default)
db.collection_name.find({"players": {$gt: 4}}, {"items": 1})

// Remove the default _id
db.collection_name.find({"players": {$gt: 4}}, {"items": 1, "_id": 0})

// This will show every other field and "items" will be ommitted
db.collection_name.find({"players": {$gt: 1}}, {"items": 0})
```
