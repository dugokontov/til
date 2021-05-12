## Combine data from two collections

To combine data from two or more collections use [`$lookup`](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/) aggregation.

### How?

Let's say we want to augment data from one collection with data from the other.

```js
db.collection1.aggregate([
  // union data from other collection
  {
    $lookup: {
      // name of the second collection
      from: "collection2",
      // variables that can be used in pipeline.
      // let's say we will create a variable that will hold
      // id in collection 2. but that ID is constructed using
      // string concatenation of string literal and property `id`
      let: { collection_2_id: { $concat: ["collection_1_", "$id"] } },
      // pipeline is an array of steps that will take place for each element in collection1
      pipeline: [
        // here we find matching element in collection2 using
        // defined variable `collection_2_id
        { $match: { $expr: { $eq: ["$id", "$$collection_2_id"] } } },
        // we map object from collection 2 into something
        // that is useful for us
        {
          $project: {
            info: "$propertyOfInterest",
            // we are not interested in build in id, so we say
            // don't map it
            _id: 0,
          },
        },
      ],
      // name of the property where data from collection 2 will be stored
      as: "newPropertyInDocument",
    },
  },
  // records created using $lookup are always stored as an array.
  // $unwind will create a document for each element of the array.
  // If we are sure that only one document will be returned from
  // collection 2 that this will simply convert array to object
  { $unwind: "$newPropertyInDocument" },
  // now we can use this property in further aggregation methods
  { $match: { "newPropertyInDocument.info": { $exists: true } } },
]);
```

### Reference
 - https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/
