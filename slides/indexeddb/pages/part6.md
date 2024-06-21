# Create index

To create an index on an object store, use the `IDBObjectStore.createIndex()`.

````md magic-move

<!-- Initial code -->

```js
async function openDB(dbName, version, handler) {
	// ...
}

const handler = {
	onupgradeneeded: (db) => {
		db.createObjectStore('todos', { keyPath: 'id' });
	},
};

async function init() {
	try {
		await openDB('todoApp', 1, handler);
	} catch (error) {
		console.error(error);
	}
}

await init();
```

<!-- Create index -->

```js{all|5-6}
const handler = {
  onupgradeneeded: (db) => {
    const objectStore = db.createObjectStore('todos', { keyPath: 'id' });

    // Todos are sorted by date ascending
    objectStore.createIndex('date', 'createdAt');
  }
}

async function init() {
  try {
    await openDB('todoApp', 2, handler); // Update version!!
  } catch (error) {
    console.error(error);
  }
}
```

<!-- Unique index -->

```js{8-9}
const handler = {
  onupgradeneeded: (db) => {
    const objectStore = db.createObjectStore('todos', { keyPath: 'id' });

		// Todos are sorted by date ascending
    objectStore.createIndex('date', 'createdAt');

    // Unique index
    objectStore.createIndex('date', 'createdAt', { unique: true });
  }
}

async function init() {
  try {
    await openDB('todoApp', 2, handler); // Update version!!
  } catch (error) {
    console.error(error);
  }
}
```

<!-- Set unique option -->

```js{5-6|all}
const handler = {
  onupgradeneeded: (db) => {
    const objectStore = db.createObjectStore('todos', { keyPath: 'id' });

    // Unique index
    objectStore.createIndex('date', 'createdAt', { unique: true });
  }
}

async function init() {
  try {
    await openDB('todoApp', 2, handler); // Update version!!
  } catch (error) {
    console.error(error);
  }
}
```


<!-- Prevents re-creation of an object store with the same name -->

```js{all|7-12|14-17|all}
const handler = {
	onupgradeneeded: (db) => {
		const storeName = 'todos';

		let objectStore;

		// Check if object store exists
		if (!(db.objectStoreNames.contains(storeName))) {
			objectStore = db.createObjectStore(storeName, { keyPath: 'id' });			
		} else {			
			objectStore = db.transaction.objectStore(storeName);
		}

		// Create date index
		if (!objectStore.indexNames.contains('date')) {
			objectStore.createIndex('date', 'createdAt');
		}
	},
};
```
````

---

# Using an index

To get the registered indexes, use the `IDBObjectStore.index()`.

````md magic-move
```js{all|5-6|8-9|11-12|14-15}
await init();

// ...Other functions

// 1. Start a transaction
const transaction = startTransaction(db, ['todos']); // = db.transaction(['todos']);

// 2. Get object store
const objectStore = getObjectStore(transaction, 'todos'); // = transaction.objectStore('todos');

// 3. Get index
const dateIndex = objectStore.index('date');

// 4. Get todos in ascending date order
const todos = getAllData(dateIndex); // = dateIndex.getAll();
```
````

---

# Recap

1. Create an index with `ObjectStore.createIndex()` in upgradeneeded event.

2. Upgrade database version.

3. Access the index with `ObjectStore.index()`.

---

# Using an cursor

To access the data stored in the object store piece by piece, use the `IDBObjectStore.openCursor()` or `IDBIndex.openCursor()`.

````md magic-move

<!-- Basic usage -->

```js{all|5|7-17|all}
const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

const cursorReqeust = objectStore.openCursor();

cursorReqeust.onsuccess = (event) => {
	const cursor = event.target.result;

	if (cursor) {
		console.log(cursor.value); // Log todo
		cursor.continue(); // Move to the next record
	} else {
		console.log('No more todos');
	}
};
```

<!-- Cursor index -->

```js{5-7|all}
const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

const dateIndex = objectStore.index('date');

const cursorReqeust = dateIndex.openCursor();

cursorReqeust.onsuccess = (event) => {
	const cursor = event.target.result;

	if (cursor) {
		console.log(cursor.value); // Print todo sorted by date
		cursor.continue(); // Move to the next todo
	} else {
		console.log('No more todos');
	}
};
```
````

---

# Advanced usage

Set a flag to abort cursor operations midway. 

````md magic-move
```js{5-7|9-20}
const dateIndex = objectStore.index('date');

const cursorRequest = dateIndex.openCursor();

let count = 0;

const limit = 10;

cursorRequest.onsuccess = (event) => {
	const cursor = event.target.result;

	// Get the top 10 todos in order of oldest todos by date
	if (cursor) {
		console.log(cursor.value);
		count++;

		// Do not call the continue() if count is limit number or more.
		count < limit && cursor.continue();
	} else {
		console.log('No more todos');
	}
};

```
````

---

# Summary

## Index

- Create an index using `IDBObjectStore.createIndex()`.

- Get the index using `IDBObjectStore.index()`.

## Cursor

- Access the data using `IDBObjectStore.openCursor()` or `IDBIndex.openCursor()`.

- Use the `cursor.continue()` method to move to the next data.

