# MONGOOSE CRUD

## Create
Single
```js
await Users.create({name:"Alex",age:22})
```
multiple: 
```js
await Users.insertMany([{name:"Srujan",age:23},{name:'Rob',age:14}]) (should pass an array of objects)
```

## Read

```js
await Users.find({}) //Find all the documents
await Users.find({name:'Srujan'}) //Only one document
await Users.findById(Id) // (need to pass an ID)
```

## Update
```js
- await Users.updateOne(
    {name:"Srujan"},
    {$set:{age:24}}
)
```
Return the document after updating:
```js
await Users.findOneAndUpdate(
    {Name:"Rob"},
    {$set:{age:15}},
    {new:true}
)
```
Increment many documents:
```js
await Users.updateMany(
    {age:{$gt:20}},
    {$inc:{age:1}}
)
```
# Delete
```js
await Users.deleteOne({name:"Srujan"})
await Users.deleteMany({age:{$lt:18}})
await Users.findByIdAndDelete(Id) //Delete by id
```
# save()
```js
const user = new User({name:"Roshi",age:32})//This is still a js object and not written in db.
```
- call save() when you create or modify document in memory 
ex of what's not in memory:
```js
const user = await User.create({name:"Roshi",age:32})
```
- Calling save() will run Schema validations and pre/post save() middleware.

