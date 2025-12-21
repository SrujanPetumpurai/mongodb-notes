# MONGOOSE CRUD

## Create
- await Users.create({name:"Alex",age:22})
multiple: 
-await Users.insertMany([{name:"Srujan",age:23},{name:'Rob',age:14}]) (should pass an array of objects)

## Read
- await Users.find({})
 Only one document via a property 
- await Users.find({name:'Srujan'})
 Find one document via ID
- await Users.findById(Id) (need to pass an ID)

## Update
- await Users.updateOne(
    {name:"Srujan"},
    {$set:{age:24}}
)
Return the document after updating:
-await Users.findOneAndUpdate(
    {Name:"Rob"},
    {$set:{age:15}},
    {new:true}
)
Increment many documents:
- await Users.updateMany(
    {age:{$gt:20}},
    {$inc:{age:1}}
)

# Delete
-await Users.deleteOne({name:"Srujan"})
 Delete by Id:
- await Users.findByIdAndDelete(Id)
 Delete many:
- await Users.deleteMany(
    {age:{$lt:18}}
)

# save()
- call save() when you create or modify document in memory
ex: const user = new User({name:"Roshi",age:32}) //This is still a js object and not written in db.
ex of what's not in memory:
const user = await User.create({name:"Roshi",age:32})

- Calling save() will run Schema validations and pre/post save() middle ware.

