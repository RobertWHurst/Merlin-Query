Monarch Query
=============

Monarch Query is a query language developed for
the (soon to be released) Monarch ORM.

Designed as a standalone package to allow other
libraries to share a common query language. It
can be used to filter arrays, and check for
matches.

Use it with node or in the browser using
browserify.

```shell
npm install monarch-query
```

[![NPM version](https://badge.fury.io/js/monarch-query.png)](http://badge.fury.io/js/monarch-query)

Example
-------
```javascript
var query = require('monarch-query');

// create a query for fresh apples.
var q = query({
  kind: 'apple',
  sweetness: { $gt: 60, $lt: 90, $sort: [{ kind: 'asc' }] }
});

// our array of fruits.
var fruits = [
  { kind: 'apple', sweetness: 74 },
  { kind: 'bannana', sweetness: 54 },
  { kind: 'mango', sweetness: 81 },
  { kind: 'apple', sweetness: 99 },
  { kind: 'mango', sweetness: 40 },
  { kind: 'apple', sweetness: 44 },
  { kind: 'apple', sweetness: 61 },
  { kind: 'bannana', sweetness: 97 }
];

// filter the fresh apples from the lot.
var freshApples = q.filter(fruits);
freshApples => [
  { kind: 'apple', sweetness: 61 },
  { kind: 'apple', sweetness: 74 }
];

// our a random fruit.
var randomFruit = { kind: 'apple', sweetness: 74 };

// check to see if the fruit matches the query.
var isMatch = q.match(randomFruit);
isMatch => true
```

Possible Query Properties
-------------------------
Below is an example of the possible fields in
a query object.

```javascript
{
  field: 'value',
  field: /pattern/,
  field: { $in: ['value1', 'value2'] },
  field: { $notIn: ['notvalue1', 'notvalue2'] },
  field: { $not: 'notvalue' },
  field: { $gt: 1 },
  field: { $gt: 'a' },
  field: { $ln: 1 },
  field: { $ln: 'a' },
  field: { $exists: true },
  $fields: ['field1', 'field2'],
  $sort: [{ field1: 'asc' }, { field2: 'desc' }],
  $offset: 0,
  $limit: 10
}
```

Query
-----
```javascript
query(Object query) => Query q
```
Creates a new query. The query can be used to
filter an array of objects, or deterime if a
single item is a match.

The query itself supports a collection of
operators, as well as regex matching.

Note that operators can be used together.

#### Direct Match
```javascript
{ field: 'val' }
```
Will match any object containing the property
`field` with the value `'val'`.

#### Regex
```javascript
{ field: /pattern/ }
```
Will match any object containing the property
`field` with a value matching the `/pattern/`.

#### Exists
```javascript
{ field: { $exists: true } }
```
Will match any object containing the property
`field` regardless of the value.

#### Less Than
```javascript
{ field: { $lt: 1 } }
```
Will match any object containing the property
`field` with a value less than 1. Less than works
with strings and numbers.

#### Greater Than
```javascript
{ field: { $gt: 1 } }
```
Will match any object containing the property
`field` with a value greater than 1. Greater than
works with strings and numbers.

#### Not
```javascript
{ field: { $not: 'val' } }
```
Will match any object containing the property
`field` with a value not equal to `'val'`.

#### In
```javascript
{ field: { $in: ['val1', 'val2'] } }
```
Will match any object containing the property
`field` with a value matching any within the
array. In this case `'val1'` and `'val2'`.

#### Not In
```javascript
{ field: { $notIn: ['val1', 'val2'] } }
```
Will match any object containing the property
`field` with a value not matching any within the
array. In this case anything not equal to 
`'val1'` and `'val2'`.

#### Offset
```javascript
{ $offset: 10 }
```
Causes `.filter` to skip the first `10` matches.

#### Limit
```javascript
{ $limit: 10 }
```
Causes `.filter` to skip the limit the results
to `10` matches. Limit will only be applied after
offset. Thus if the offset is `10` then matches 
`11` through `20` will be included in the results.

#### Fields
```javascript
{ $fields: ['field1', 'field2'] }
```
Results will only contain the fields given within
the fields array. In this example, the results will
only contain the fields `'field1'` and `'field2'`.

#### Sort
```javascript
{ $sort: [{ field1: 'asc' }, { field2: 'desc' }] }
```
Results will be sorted by the fields given within
each sort object. A sort object may only have one
key. The key is the field path you wish to sort by
and the value is ether `'asc'` for assending or
`'desc'` for decending. Sorting is done in order
of the sort array.  In this example, the results
will be sorted first by `field1` in ascending order,
then by `'field2'` in decending order.

Match
-----
```javascript
q.match(Object data) => Boolean isMatch;
```
Checks an object against the query. For
controlling the results see `$offset`, `$limit`,
`$fields`, and `$sort` above.

Filter
------
```javascript
q.filter(Array dataSet) => Array matches;
```
Checks each item in a data set against the query.
Returns an array containing the matching items.
