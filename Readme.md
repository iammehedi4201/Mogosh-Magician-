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

## Another way for Field Filtering using `.project()`

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
  .find({ gender: "Male", age: { $gte: 18, $lte: 30 } }, { age: 1, gender: 1 })
  .sort({ age: 1 });
```

## Using `$in` operator

- $in array modde je value gulo dewa hobe oigulo mille oi value documents gulo dibe.
- jodi eka mile tahole ektai dibe

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
      interests: "Coking", // interest is `array of string `so we check that the value is in `interest` field  or not by putting it's value.we don't need any map or loop
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
      interests: { $in: ["Coking", "Reading"] },
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
      { interests: { $in: ["Coking", "Reading"] } }, //condition 4
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
    age: { $not: { $in: [1, 3, 4, 7] } }, //egulo bade baki sob dibe
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

## (`$not`,`$nor`,`#nin`) E operators gulo moddde amra je value gulo dibe oi gulo bade baki sob dibe value dibe

## Using `$exits` operators

- The $exists operator in MongoDB is used to check whether a specified field exists in a document. It returns documents where the field does or does not exist, based on its value which can be true or false.

- Note : `If i order $exits operator to find the filed with null , empty string , undefine value then $exits will not find it for me  `

```ts
db.test.find({ age: { $exists: true } }); // `age` field exits kore emon document gulo deo

//---------------

db.test.find({ age: { $exists: false } }); //`age` field exits kore nah emon document gulo deo
```

## Using `$type` operators

- The $type operator in MongoDB is used to select documents where a field's value is a certain type. The value can be a BSON type number or the string alias.

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

- The $size operator in MongoDB is used to select documents where an array field has a certain number of elements. It matches any array with the number of elements specified by the $size operator.

For example, if you have a collection of students and you want to find all students who have exactly 3 interests, you would use the $size operator.

- `$size` only used in `Array`

```ts
db.test.find({ interests: { $size: 2 } }, { interests: 1, email: 1 }); // ekhane interests array te 2 element thakle oita dibe

//---------------

db.test.find({ interests: { $size: 0 } }, { interests: 1, email: 1 }); // array empty thekle oita dibe
```

## Using `$all` operator

-The $all operator in MongoDB is used to select documents where an array field contains all the specified elements. It's like saying "I want all of these".

For example, let's say you have a collection of students, and each student document has an interests array. If you want to find all students who are interested in both "Cooking" and "Reading", you would use the $all operator.

```ts
db.test
  .find({ interests: { $all: ["Travelling", "Gaming", "Reading"] } })
  .projection({ interests: 1 });
```

## Using `$elemMatch` operator

- The $elemMatch operator in MongoDB is used to match documents where an array field contains at least one element that meets all the specified criteria. It's like saying "I want at least one match of all these conditions in the array".

- it's mostly use in `Array of objects `

```ts
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
```

## Exception "

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
