# Query Operations

## Comparision
    $gt //greater than
    $lt //Leser than
    $gte //greater then or equal to 
    $lte // lesser than or equal to 
    $nt // not equal to

## Array operation

{role:{$in:['sunflower','Rose','violet']}} //TO check if role is in the given array
{role:{$nin:['Mango','Watermelon','Papaya']}} // TO check that role is not in the given array

## logical

Explicit way of using and:-

    await Users.find({                  // And operator makes sure all the conditions inside the array are met
        $and:[                         //Should Pass a array of conditions inside and operator
        {age:{gt:18}},
        {age:{lt:60}}                 //Checking that age is greater than 18 and less than 60
        ]
    });

Implicit way of using and:-
    await Users.find({
        age:{$gt:18,$lt:60}
    });

If The condition field are different:-
    await Users.find({
        age:{$gt:18},
        isActive:true
    });

## To check if a specified field exits 

await Users.find({
    phone:{$exists:true}
})

##  Suppose there is a property that holds a array of elemnts

await Users.find({
   tags:{$all:['Srujan','Rob','Ned']} //It will return the document whos tags contain all these elements inside it's array
})

await Users.find({
    score:{$size:3}             //Return the document whose score is a array of only 3 elements
})

## Regex 

await Users.find({            // ^ means at the start, $ means at the end
    name:{$regex:/^sr/}      //Name that starts with sr and case Insensitive. (^_^)
})

await Users.find({
    name:{$regex:/sr$/i}    //Name that ends with sr and is casesensitive
})