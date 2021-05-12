## Aggregate in mongoDB

Aggregate is a powerful method that enables advanced and complex operations on records in database.

### Why?

Some use cases I run into:

- Filter after mapping fields
- Complex filters (eg. number of keys in object)
- Combine data from other collections (using [`$lookup`](/mongodb/combine-data-from-two-collections.md))

### How?

[Aggregate function](https://docs.mongodb.com/manual/aggregation/) accepts an array of operations that will be run in sequence. For example, to find some elements, count number of keys those element have and then filter out those that have more than one key, following code could be run:

```js
db.collection.aggregate([
  // filter only certain type
  { $match: { type: { $in: ["typeA", "typeB"] } } },

  // count number of guids present in property `elements.byGuid`
  {
    $addFields: {
      numberOfKeys: {
        $size: { $objectToArray: "$elements.byGuid" },
      },
    },
  },

  // filter again only those that have more than one key
  { $match: { numberOfKeys: { $gt: 1 } } },
]);
```
