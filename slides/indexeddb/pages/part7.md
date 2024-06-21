# Range and Direction

## Cursor range

To access with a cursor only those records that meet certain conditions from the large amount of data stored in the object store, create an `IDBKeyRange` object.

## Cursor direction

The `openCursor()` method can specify the circulation direction of the cursor in the second argument.

The index may have a record with a duplicate key, and it can also specify how the record with the duplicate key is processed.

---

## layout: two-cols

# Cursor data with range

1. Create an index

2. Create an `IDBKeyRange` object

3. Use `openCursor()` method with the `IDBKeyRange` object

::right::

````md magic-move
```js
await init();

const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');
```

<!-- Access date index -->

```js{7}
await init();

const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

const dateIndex = objectStore.index('date');
```

<!-- Define start and end -->

```js{9-10}
await init();

const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

const dateIndex = objectStore.index('date');

const startDate = new Date('2024-06-01').getTime();
const endDate = new Date('2024-07-01').getTime();
```

<!-- Create range -->

```js{6-7}
const dateIndex = objectStore.index('date');

const startDate = new Date('2024-06-01').getTime();
const endDate = new Date('2024-07-01').getTime();

// 2024-06-01 <= DATE < 2024-07-01
const range = IDBKeyRange.bound(startDate, endDate, false, true);
```

<!-- Use range -->

```js{9-20}
const dateIndex = objectStore.index('date');

const startDate = new Date('2020-06-01').getTime();
const endDate = new Date('2020-07-01').getTime();

// 2020-06-01 <= DATE < 2020-07-01
const range = IDBKeyRange.bound(startDate, endDate, false, true);

const cursorRequest = dateIndex.openCursor(range);

cursorRequest.onsuccess = (event) => {
  const cursor = event.target.result;

  if (cursor) {
    console.log(cursor.value); // Log todo with date in June
    cursor.continue();
  } else {
    console.log('No more todos');
  }
};
```
````

---

# Cursor direction

By default, the cursor accesses the dataset in ascending key order. If you want to change this behavior, set the second argument to the `openCursor()` method.

````md magic-move
<!-- IDBObjectStore.openCursor() -->

```js
const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

// The cursor cycles through the ids in descending order.
const cursorRequest = objectStore.openCursor(null, 'prev');

cursorRequest.onsuccess = (event) => {
  // ...
};
```

```js
const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

// This date index may contain duplicate values ​​set as keys.
const someIndex = objectStore.index('date');

// If there are multiple records with the same key value,
// only the first of those records visited is returned.
const cursorRequest = dateIndex.openCursor(null, 'nextunique');

cursorRequest.onsuccess = (event) => {
  // ...
};
```

```js{7-8}
const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

const dateIndex = objectStore.index('date');

// Descending order of date
const cursorRequest = dateIndex.openCursor(null, 'prev');

cursorRequest.onsuccess = (event) => {
  // ...
};
```
````

---

# Summary

- `IDBKeyRange` can be used to narrow the results of a request to a specific range.

- The second argument of the `openCursor()` method specifies the cursor direction.
