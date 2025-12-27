# Middleware(pre/post hooks)
Mongoose has 4 types of middleware: document middleware, model middleware, aggregate middleware, and query middleware.

1.) Document middleware
Hooks:
validate()              (checks schema rules and validates document data, runs before validation)
save()                  (writes the document to MongoDB, runs before or after saving)
remove()                (deletes the document using doc.remove(), runs before or after deletion)
deleteOne()             (deletes this document instance, runs before or after deletion)
init()                  (creates a document object from MongoDB data, runs when document is loaded)

2.) Query middleware
Hooks:
find()                  (retrieves multiple documents matching a query, runs before or after execution)
findOne()               (retrieves a single matching document, runs before or after execution)
findOneAndUpdate()      (finds a document and updates it in one step, runs before or after execution)
findOneAndDelete()      (finds a document and deletes it in one step, runs before or after execution)
updateOne()             (updates one matching document, runs before or after execution)
updateMany()            (updates multiple matching documents, runs before or after execution)
deleteOne()             (deletes one matching document via query, runs before or after execution)
deleteMany()            (deletes multiple matching documents via query, runs before or after execution)
count()                 (counts documents using a query filter, runs before or after execution)
countDocuments()        (counts documents matching a filter accurately, runs before or after execution)
estimatedDocumentCount()(estimates total documents in collection, runs before or after execution)

3.) Model middleware
Hooks:
insertMany()            (bulk-inserts multiple documents directly, runs before or after insertion)

4.) Aggregate middleware
Hooks:
aggregate()             (executes an aggregation pipeline, runs before or after execution)

## Middleware phases
We have pre hook, which is when the middleware runs before the running the function and post hook which runs after running the function.
pre middleware can block or modify the document while post is read-only.

Ex for pre:-
schema.pre('save', function () {
   this.updatedAt = Date.now(); 
});
<!-- this is the current document,updatedAt is assumed to be defined at schema definition -->

schema.pre('find', function () {
  this.where({ isDeleted: false });
});
<!-- this is the query object,isDeleted is assumed to be in the schema definition -->

schema.pre('insertMany', function (next, docs) {
  docs.forEach(d => d.createdAt = Date.now());
  next();
});

schema.pre('insertMany', function (next, docs) {
  docs.forEach(d => d.createdAt = Date.now());
  next();
});

<!-- docs is array of objects to be inserted -->

schema.pre('aggregate', function () {
  this.pipeline().unshift({ $match: { isDeleted: false } });
});

Ex for post:-
schema.post('save', function (doc) {
  console.log('Saved:', doc._id);
});

schema.post('find', function (docs) {
  console.log('Found:', docs.length);
});

schema.post('insertMany', function (docs) {
  console.log('Inserted:', docs.length);
});

schema.post('aggregate', function (result) {
  console.log('Aggregation result ready');
});


