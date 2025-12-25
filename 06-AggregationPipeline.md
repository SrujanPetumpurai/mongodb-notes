# Aggregation & Pipeline
Aggregation:-The whole process of process data in different stages.
Pipeline:Contains the different stages.

//find()-> Is for fetching documents
//aggregate()-> Is to build new documents from old ones

Syntax to use aggregation:-
Users.aggregate(pipeline,options)
         pipeline-Array<Objects>
         options-Object(OPTIONAL)

pipeline structure:
 [
    {$stageName:{}},
    {$stageName:{}}
 ]

## Core aggregation stages:-

1.)Filtering/limiting:
  $match,
  $limit,
  $skip,
  $sort
2.)Grouping/analytics:
  $group,
  $count
3.)Reshaping documents:
  $project,
  $addFields,
  $unset,
  $replaceRoot,
4.)Joins:
  $lookup,
  $unwind
5.)Advanced:
  $facet,
  $bucket,
  $bucketAuto

## Query operations inside stages

1.)Inside $match:
$eq, $ne       //(equal to, not equal to)
$gt, $gte        
$lt, $lte
$in, $nin
$and, $or
$regex

ex:Users.aggregate([{$match:{age:{$gte:18}}}])

2.)Inside $group:
$sum
$avg
$min
$max
$first
$last
$push
$addToSet
$count
ex:Users.aggregate([
    {$group:{
        _id:'$userId',
        total:{$sum:'$amount'}
    }}
])

3.)Inside project/addFields
$concat
$toString
$ifNull
$cond
$multiply
$divide
$substr

Users.aggregate([
    {
  $project: {
    fullName: { $concat: ["$first", " ", "$last"] }
  }
}
])

4.)lookup operations:
Orders.aggregate([
    {
  $lookup: {
    from: "users",
    localField: "userId",
    foreignField: "_id",
    as: "user"
  }
}])  
//It is saying add user document from the users collection where the foreingField:_id matches with the localField userId from Orders collections as user
//This adds a field user to the documents returned temporarily in memory for that moment.

## Options
Common options:
allowDiskUse: true → large datasets
maxTimeMS → timeout
collation → locale-aware sorting

ex:-
Model.aggregate(pipeline, {
  allowDiskUse: true,
  maxTimeMS: 1000
})

//Most of the time they are skipped