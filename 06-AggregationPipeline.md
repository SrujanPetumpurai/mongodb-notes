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
$match (filter documents by condition),
$limit (restrict number of documents),
$skip (skip N documents),
$sort (order documents)

2.)Grouping/analytics:
$group (group documents and compute values),
$count (count documents)

3.)Reshaping documents:
$project (include/exclude/transform fields),
$addFields (add or compute new fields),
$unset (remove fields),
$replaceRoot (replace entire document)

4.)Joins:
$lookup (join another collection),
$unwind (flatten array field)

5.)Advanced:
$facet (run multiple pipelines in parallel),
$bucket (group into fixed ranges),
$bucketAuto (group into auto-calculated ranges)

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