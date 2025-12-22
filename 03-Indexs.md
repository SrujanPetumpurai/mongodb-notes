# Indexes: single, compound, text, 2dsphere
:Index is a extra data structure created by mongodb to make reading faster but downsides are:-
More memory
Slower writing
## Single-Field index
Index on one field only

//At Schema definition
userSchema.index({age:1})  //This is how you single index

## Compound Index
Index on multiple fields in a fixed order

userSchema.index({age:1,isActive:1})
//First the data structure is stored by age and then by isActive
//Ex data:- age:20,isActive:true
//          age:20,isActive:false,
//          age:21,isActive:true
:-When querying,
Users.find({age:20}) //Works fine
Users.find({age:25,isActive:false}) //Works
Users.find({isActive:true}) //Doesn't not work, because mongodb can't jump in isActive

## Text Index
You can text index, only on one field in a collection.
It breaks the text into tokens.Removes stop words(is,the,and).
Gives score to the document depending on the text.

- userSchema.index({bio:'text'}) //Covers only one field
- userSchema.index({bio:'text',title:'text'})  //Covers two fields in a collection but is still only one text index.
- userSchema.index({bio:'text',title:'text'},{weight:{title:10,bio:1}})

//How to search using text index
- await Users.find({$text:{$search:'Youtuber and actor'}}}) //Looks up token youtube, looks up token actor and ranks the document by textScore 

//If you want to sort the results then do this:
await Users.find({$text:{$search:'Youtuber and actor'}}).sort({score:{$meta:'textScore'}})
//Mongodb sort function, sort({field:1}) //normal format
//Can roughly take these, sort:-{
    field:(1|-1|metaexpression)
}

