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

## 2dsphere index
It is a geospatial index that tells mongodb, these coordinates are points on Earth's surface(sphere).
2dSphere index can you used to calculate:-
i)Nearest location
ii)maxDistance
ii)geoWithin

In the Schema defenition the field you want to put 2dsphere index on should have GeoJson;
ex:-
const placeSchema = new mongoose.Schema({
    location:{
        type:{
            type:'String',
            enum:['Point'],
            required:true
        },
        coordinates:{
            type:[Number],
            required:true
        }
    }
})

placeSchema.index({location:'2dsphere'})

//Find nearest points:-
await Place.find({
    location:{
        $near:{
            $geometry:{type:'Point',coordinates:[45.9284874,78.3828473]},
            $maxDistance:5000
        }
    }
});
//Returns the document  

//You would have a collection with all documents in geoJson format,
//All points inside the circle:-
await Place.find({
  location: {
    $geoWithin: {
      $centerSphere: [[83.2185, 17.6868],5/6378.1] // 6378.1 is earths radius in km. radians = distance/earth's radius.
      // We will get a 5km radius circle.
    }
  }
});

//Inside a polygon
await Place.find({
  location: {
    $geoWithin: {
      $polygon: [
        [83.21,17.68],
        [83.22,17.68],
        [83.22,17.69],
        [83.21,17.69],
        [83.21,17.68]
      ]
    }
  }
});
//Return the document

//Checks overlap with a geometry
await Place.find({
  location: {
    $geoIntersects: {
      $geometry: {
        type: 'Polygon',
        coordinates: [[
          [83.21,17.68],
          [83.22,17.68],
          [83.22,17.69],
          [83.21,17.69],
          [83.21,17.68]
        ]]
      }
    }
  }
});

//Returns the Document




