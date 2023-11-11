# <p style="color: blue">Be A Mongo Magician: <p>

## For Insert one data

```ts
db.user.insertOne({ name: "Mehedi Hasan", age: 25, Phone: "0177577038" });
```

## For Insert Many Data

```ts
db.test.insertMany([{}, {}, {}]);
```

## For find every data

```ts
db.test.find({});
```

## For find one data

```ts
db.test.findOne({});
```

## For Field Filtering use this query

- (1) mean true that's mean it will take this field

```ts
db.test.find({ gender: "Male" }, { name: 1, phone: 1 }); //test mean document collection
```

## Another way for Field Filtering using `.project() | .projection()`

- `note`: .project only work with `.find()`

```ts
db.test.find({ gender: "Male" }).project({ name: 1, email: 1 });
```

## Using `$eq`(equal to ) operator

- The `$eq` operator matches documents where the value of a field equals the specified value.

```ts
{ <field>: { $eq: <value> } }
db.test.find({ phone: { $eq: "(670) 1831100" } });
```

## Using `$ne` (not equal) operator

- The `$ne`selects the documents where the value of the specified field is not equal to the specified value.
- ekhane je field ta dibe ta bade onno sob amake dibe

```ts
{ <field>: { $ne: <value> } }
db.test.find({ phone: { $ne: "(670) 1831100" } })

```

## Using `$gt` (greater than) operator

```ts
{ <field>: { $gt: <value> } }
db.test.find({age:{$gt: 18}})
```

## Using `$gte` (greater than equal to) operator

```ts
{ <field>: { $gte: <value> } }
db.test.find({age:{$gte: 18}})
```

## using `.sort()` operator

- we can make number ascending to descending and descending to ascending using `sort()`. `1` mean `ascending to descending ` and `-1` mean `descending to ascending `

```ts
.sort({ sortField: -1 });

db.test.find({ age: { $gte: 50 } }).sort({ age: 1 });
```

## Using `$lt` (less than) operator

```ts
{ <field>: { $lt: <value> } }
db.test.find({age:{$lt: 18}})
```

## Using `$lte` (less than equal to) operator

```ts
{ <field>: { $lte: <value> } }
db.test.find({age:{$lte: 18}})
```

## Using `implicit and`

- we can use multiple condition use `,` this called `implicit and`

```ts
// { $gte: 18, $lte: 30 } in using `,` we using multiple condition

db.test
  .find(
    {
      gender: "Male", //condition 1
      age: { $gte: 18, $lte: 30 }, //condition 2
    },
    { age: 1, gender: 1 }
  )
  .sort({ age: 1 });
```

## Using `$in` operator

- $in array modde je value gulo dewa hobe oigulo mille oi value documents gulo dibe.
- jodi eka mile tahole ektai dibe
- $in mean holo eta othoba oita

```ts
//{ field: { $in: [<value1>, <value2>, ... <valueN> ] } }
db.test
  .find(
    { gender: "Female", age: { $in: [20, 22, 26, 28] } }, // it will give these value document
    { age: 1, gender: 1 }
  )
  .sort({ age: -1 });
```

## Using `$nin` operator

- $nin array modde je value gulo dewa hobe oigulo mille oi valuer documents gulo dibe nah.baki gulo dibe

```ts
//{ field: { $in: [<value1>, <value2>, ... <valueN> ] } }
db.test
  .find(
    { gender: "Female", age: { $nin: [20, 22, 26, 28] } }, // it will not give these value document
    { age: 1, gender: 1 }
  )
  .sort({ age: -1 });

//------------------------------

//"interests" : [ "Cooking", "Travelling", "Reading" ]

db.test
  .find(
    {
      gender: "Female",
      age: { $gte: 18, $lte: 30 },
      interests: "Cooking", // interest is `array of string `so we check that the value is in `interest` field  or not by putting it's value.we don't need any map or loop
    },
    { age: 1, gender: 1, interests: 1 }
  )
  .sort({ age: 1 });
//-----------------------------

db.test
  .find(
    {
      gender: "Female",
      age: { $gte: 18, $lte: 30 },
      interests: { $in: ["Cooking", "Reading"] },
    },
    { age: 1, gender: 1, interests: 1 }
  )
  .sort({ age: 1 });
```

## Using `Explicit $and` Operator

```ts
//sob condition match korte hobe
//{ $and: [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }
db.test
  .find({
    $and: [
      { gender: "Female" }, //condition 1
      { age: { $gte: 18 } }, //condition 2
      { age: { $lte: 30 } }, //condition 3
      { interests: { $in: ["Cooking", "Reading"] } }, //condition 4
    ],
  })
  .project({
    age: 1,
    gender: 1,
    interests: 1,
  })
  .sort({
    age: 1,
  });
```

- if the field the is array of object then we write this for applying condition :

```ts

"skills" : [
		{
			"name" : "C",
			"level" : "Intermidiate",
			"isLearning" : true
		},
		{
			"name" : "JAVASCRIPT",
			"level" : "Intermidiate",
			"isLearning" : true
		},
		{
			"name" : "C#",
			"level" : "Expert",
			"isLearning" : true
		}
	]

db.test.find({
    $and: [
        { "skills.name": "JAVASCRIPT" },
        { "skills.name": "C" }
    ]
}).projection({
    skills: 1
})


```

## Using `Explicit $or` Operator

```ts
// jekono ekta match korle hobe
//{ $or: [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }
db.test
  .find({
    $or: [{ interests: "Coking" }, { interests: "Traveling" }],
  })
  .project({
    interests: 1,
  });
```

## `Implicit $or`

```ts
db.test
  .find({
    "skills.name": { $in: ["c", "JAVA"] },
  })
  .projection({
    skills: 1,
  });
```

## Using `$nor` Operator

- The `$nor` operator in MongoDB is used to select documents where none of the conditions are true. It's like saying "give me the items where none of these conditions match". For example, if you have a collection of books and you want to find all books that are neither written by "Author A" nor "Author B", you would use the $nor operator.

```ts
//{ $nor: [ { <expression1> }, { <expression2> }, ...  { <expressionN> } ] }

db.test.find({
  $nor: [{ languages: "German" }, { languages: "Portuguese" }],
});
```

## Using `$not` Operator

-The `$not` operator in MongoDB is used to negate a condition, returning documents that do not match the condition. It's like saying "give me the items where this condition does not match".

For example, if you have a collection of books and you want to find all books that are not written in "English", you would use the $not operator.

```ts
//{ field: { $not: { <operator-expression> } } }

db.test
  .find({
    age: { $no t: { $in: [1, 3, 4, 7] } }, //egulo bade baki sob dibe
  })
  .project({
    age: 1,
  })
  .sort({
    age: 1,
  });

//--------------------
db.test
  .find({
    age: { $not: { $eq: 12 } }, // 12 bade baki sob dibe
  })
  .project({
    age: 1,
  })
  .sort({
    age: 1,
  });
```

## (`$not`,`$nor`,`$nin,$ne`) E operators gulo moddde amra je value gulo dibe oi gulo bade baki sob dibe value dibe

## Using `$exits` operators

- The `$exists` operator in MongoDB is used to check whether a specified field exists in a document. It returns documents where the field does or does not exist, based on its value which can be true or false.

- Note : `If i order $exits operator to find the filed with null , empty string , undefine value then $exits will not find it for me  `

```ts
db.test.find({ age: { $exists: true } }); // `age` field exits kore emon document gulo deo

//---------------

db.test.find({ age: { $exists: false } }); //`age` field exits kore nah emon document gulo deo
```

## Using `$type` operators

- The `$type` operator in MongoDB is used to select documents where a field's value is a certain type. The value can be a BSON type number or the string alias.

For example, if you have a collection of items and you want to find all items where the price field is a type of Number, you would use the $type operator.

```ts
//{ field: { $type: <BSON type> } }

db.test.find({ age: { $type: "number" } });

db.test.find({ interests: { $type: "array" } });

db.test.find({ gender: { $type: "null" } }).projection({ email: 1, gender: 1 });
```

## using `$type` operator i can check this data type :

1. `double`: Double precision floating-point numbers.
2. `string`: UTF-8 strings.
3. `object`: Embedded documents.
4. `array`: Arrays.
5. `binData`: Binary data.
6. `undefined`: Undefined values. (Deprecated)
7. `objectId`: ObjectId.
8. `bool`: Boolean values.
9. `date`: Date.
10. `null`: Null values.
11. `regex`: Regular expressions.
12. `dbPointer`: DBPointer values. (Deprecated)
13. `javascript`: JavaScript codes.
14. `symbol`: Symbol. (Deprecated)
15. `javascriptWithScope`: JavaScript code with scope.
16. `int`: 32-bit integer.
17. `timestamp`: Timestamp.
18. `long`: 64-bit integer.
19. `decimal`: Decimal128 values.
20. `minKey`: The query operator that selects documents where the value of the field is less than any other value, including null.
21. `maxKey`: The query operator that selects documents where the value of the field is more than any other value.

## Using `$size` operators

- The `$size` operator in MongoDB is used to select documents where an array field has a certain number of elements. It matches any array with the number of elements specified by the $size operator.

For example, if you have a collection of students and you want to find all students who have exactly 3 interests, you would use the `$size` operator.

- `$size` only used in `Array`

```ts
db.test.find({ interests: { $size: 2 } }, { interests: 1, email: 1 }); // ekhane interests array te 2 element thakle oita dibe

//---------------

db.test.find({ interests: { $size: 0 } }, { interests: 1, email: 1 }); // array empty thekle oita dibe
```

## Using `$all` operator

-The `$all` operator in MongoDB is used to select documents where an array field contains all the specified elements. It's like saying "I want all of these".

For example, let's say you have a collection of students, and each student document has an interests array. If you want to find all students who are interested in both "Cooking" and "Reading", you would use the $all operator.

- Basically it's used for `Array`

```ts
db.test
  .find({ interests: { $all: ["Travelling", "Gaming", "Reading"] } }) // tin tai interest jar oi document deo
  .projection({ interests: 1 });
```

## Using `$elemMatch` operator

- The `$elemMatch `operator in MongoDB is used to match documents where an array field contains at least one element that meets all the specified criteria. It's like saying "I want at least one match of all these conditions in the array".

- it's mostly use in `Array of objects `

```ts
//{ <field>: { $elemMatch: { <query1>, <query2>, ... } } }

db.test
  .find({
    skills: { $elemMatch: { name: "JAVASCRIPT", level: "Expert" } }, // ami cacchi oi document ta deo jeta array of object property e condition match kore
  })
  .projection({
    skills: 1,
  });

//--------------

db.test
  .find({
    education: {
      $elemMatch: {
        degree: "Master of Education",
        major: "Philosophy",
      },
    },
  })
  .projection({
    education: 1,
  });

//ekhane ami cacchi je oi manush document ta deo jar javascrip and java skill and expert level ar

db.test.find({
  $and: [
    {
      skills: {
        $elemMatch: {
          name: "JAVASCRIPT",
          level: "Expert",
        },
      },
    },
    {
      skills: {
        $elemMatch: {
          name: "JAVA",
          level: "Expert",
        },
      },
    },
  ],
});
```

## using `$set` operator for updating

- The` $set` operator in MongoDB is used to update the value of a field in a document. It's like saying "I want to set this field to this value".
- using `$set` operator we can add new property in object
- we can not assign same value to a property.if it's a new value then it will assign
- none premetive like (array) entirely jodi kono property change or repleace korle use korba nah hole dorkar nai

```ts
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $set: {
      age: 40,
    },
  }
);
//------------------

db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $set: {
      interests: ["Watching Anime ", "Reading", "Typeing "],
    },
  }
);

//Update in object

	"address" : {
		"street" : "1188 Lerdahl Point",
		"city" : "Dhaka",
		"country" : "Bangladesh",
		"area" : "Khilgoan"
	},

  db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000065") },
    {
        $set: {
            "address.country": "Bangladesh",
            "address.city": "Dhaka",
            "address.area": "Khilgoan"
        }
    }
)

```

## using `$addToSet` operator for updating

- The `$addToSet` operator in MongoDB is used to add a value to an array field in a document, but only if the value does not already exist in the array. It's like saying "Add this to the list, but only if it's not already there".

- using `$addToSet` we can add object to a array

```ts
//{ $addToSet: { <field1>: <value1>, ... } }

db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $addToSet: {
      interests: "Watching Tour Video",
    },
  }
);

//---------------------

db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $addToSet: {
      skills: {
        name: "REACT",
        level: "Expert",
        isLearning: false,
      },
    },
  }
);
```

## using `$each` operator for updating

- The $each operator in MongoDB is used when you want to add multiple values to an array field in a document. It's like saying "Add each of these to the list".it's also check if it exits or not . if exits it will not insert

```ts
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $addToSet: {
      interests: { $each: ["Showering", "Coding"] },
    },
  }
);
```

## using `$push` operator for updating

- The $push operator in MongoDB is used to add a value to an array field in a document, regardless of whether the value already exists in the array. It's like saying "Add this to the list".

```ts
//{ $push: { <field1>: <value1>, ... } }

db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $push: {
      interests: { $each: ["Showering", "Coding"] },
    },
  }
);
```

## using `$addToSet , $set , $push ` we can add new property to main document

## using `$unSet` operator for updating

-The `$unset` operator in MongoDB is used to remove a field from a document. It's like saying "Remove this from the list".

```ts
//{ $unset: { <field1>: "", ... } }
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $unset: {
      FavouriteCharater: "" | 1, // we can also `1` it's mean true
    },
  }
);
```

## using `$pop` operator for updating

- The `$pop` operator in MongoDB is used to remove the first or last element from an array. It's like saying "Remove the first/last item from the list".Pass $pop a value of -1 to remove the first element of an array and 1 to remove the last element in an array.

```ts
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $pop: {
      FavouriteAnime: -1,
    },
  }
);
```

## using `$pull` operator for updating

- The `$pull `operator in MongoDB is used to remove all instances of a value from an array. It's like saying "Remove this item from the list wherever it appears".

```ts
//{ $pull: { <field1>: <value|condition>, <field2>: <value|condition>, ... } }

db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $pull: {
      FavouriteAnime: "Eminance in Shadow",
    },
  }
);

//--------------------------------

//remove a object form array of object

	"skills" : [
		{
			"name" : "RUBY",
			"level" : "Ultra Expert",
			"isLearning" : true
		},
		{
			"name" : "KOTLIN",
			"level" : "Ultra Expert",
			"isLearning" : false
		}
	],

  db.test.updateOne(
    { email: "omirfin2@i2i.jp" },
    {
        $pull: {
            skills: {
               name :"Python"
            }
        }
    }
)

```

## using `$pullAll` operator for updating

- The $pullAll operator in MongoDB is used to remove all instances of multiple values from an array. It's like saying "Remove all of these items from the list wherever they appear".

```ts
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000065") },
  {
    $pullAll: {
      FavouriteAnime: [["uxufsdfklsf", "maiggaga", "Magoni"], "uxufsdfklsf"], //se fvrt anime theke e element gulo ber kore dibe
    },
  }
);
```

## using `$` positional Operator

- The $ operator in MongoDB is known as the positional operator. It is used to update the value of an item in an array that matches a certain condition. It's like saying "Update the value of the item in the list that meets this condition".

```ts
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000066"), "education.major": "History" },
  {
    $set: {
      "education.$.major": "Cse", // $ it will update first match value
    },
  }
);

//--------------------

db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000066"), "skills.level": "Expert" },
  {
    $set: {
      "skills.$[].level": "Ultra Expert", // $[] it will update all the match value
    },
  }
);
```

## using `$inc` positional Operator

- The $inc operator in MongoDB is used to increment the value of a field by a specified amount. It's like saying "Increase this value by this much".

```ts
db.test.updateOne(
  { _id: ObjectId("6406ad63fc13ae5a40000066") },
  {
    $inc: {
      age: 10,
    },
  }
);
```

## Exception

```ts
//"interests" : [ "Reading", "Gardening", "Cooking" ]
db.test.find({ "interests.2": "Cooking" }).projection({ interests: 1 }); //ekhane interest array last element `cooking` ola document ta deo
//-------------------
db.test
  .find({ interests: ["Cooking", "Reading", "Writing"] })
  .projection({ interests: 1 }); // ekhane exact  je array dilam exact oi mil array thakle oi document gulo dibe


//-------------

"skills" : [
		{
			"name" : "RUBY",
			"level" : "Expert",
			"isLearning" : true
		},
		{
			"name" : "KOTLIN",
			"level" : "Expert",
			"isLearning" : false
		}
	],

    db.test.find({
    skills: { //ekhane exact  je object  dilam exact oi mil object thakle oi document gulo dibe
        name: "RUBY",
        level: "Expert",
        isLearning: true
    }
})

```
