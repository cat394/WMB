---
layout: two-cols
---

# CRUD

## Setup

- Database

- Transaction

- Object store

::right::

<v-click>

````md magic-move
```js {all|4|7-11|13-23|25|all}
let db;

async function openDB(dbName, version, handler) {
	const request = indexedDB.open(dbName, version);

	return new Promise((resolve, reject) => {
		request.onupgradeneeded = (event) => {
			db = event.target.result;

			handler.onupgradeneeded(db);
		};

		request.onsuccess = (event) => {
			db = event.target.result;

			db.onerror = (event) => {
				console.log('DATABASE_ERROR', event.target.error);
			};

			handler.onsuccess(db);

			resolve('Database opened successfully');
		};

		request.onerror = () => reject('Failed to open database');
	});
}
```

<!-- Define handler and open Database -->

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

<!-- Add helper functions -->

```js {3-5|7-9}
await init();

function startTransaction(db, storeNames, mode = 'readonly') {
	return db.transaction(storeNames, mode);
}

function getObjectStore(transaction, storeName) {
	return transaction.objectStore(storeName);
}
```

```js {5-15}
function getObjectStore(transaction, storeName) {
	return transaction.objectStore(storeName);
}

async function requestResult(request) {
	return new Promise((resolve, reject) => {
		request.onsuccess = (event) => {
			resolve(event.target.result);
		};

		request.onerror = (event) => {
			reject(event.target.error);
		};
	});
}
```
````

</v-click>

---

## layout: center

# Let's operate the CRUD in the database!

---

# Create

````md magic-move
<!-- Define function -->

```js
async function addData(store, data) {
	const request = store.add(data);

	return await requestResult(request);
}
```

<!-- Example -->

```js
async function addData(store, data) {
	// ...
}

const data = {
	id: 2,
	done: false,
	description: 'Learn Web Components',
};

const transaction = startTransaction(db, ['todos'], 'readwrite');

const objectStore = getObjectStore(transaction, 'todos');

try {
	await addData(objectStore, data);
} catch (error) {
	console.error(error);
}
```
````

---

# Read

````md magic-move
<!-- Define function -->

```js {1-5|7-11|all}
async function getData(store, key) {
	const request = store.get(key);

	return await requestResult(request);
}

async function getAllData(store, count) {
	const request = store.getAll(count);

	return await requestResult(request);
}
```

<!-- Example -->

```js
async function getData(store, key) {
	// ...
}

async function getAllData(store, count) {
	// ...
}

const transaction = startTransaction(db, ['todos']);

const objectStore = getObjectStore(transaction, 'todos');

const key = 1;

try {
	await getData(objectStore, key); // Example: { id: 1, done: false, description: 'Learn indexedDB' }

	await getAllData(objectStore); // Example: [{ id: 1, done: false, description: 'Learn indexedDB' }, ...]
} catch (error) {
	console.error(error);
}
```
````

---

# Update

````md magic-move
<!-- Define function -->

```js
async function updateData(store, key, newData) {
	const currentData = await getData(store, key);

	if (!currentData) {
		throw new Error('Data not found', `Key: ${key}`);
	}

	const merged = { ...currentData, ...newData };

	const request = store.put(merged);

	return await requestResult(request);
}
```

<!-- Example -->

```js
async function updateData(store, key, newData) {
	// ...
}

const transaction = startTransaction(db, ['todos'], 'readwrite');

const objectStore = getObjectStore(transaction, 'todos');

const key = 1;

const newData = { todo: 'Learn JavaScript' };

try {
	await updateData(objectStore, key, newData);
} catch (error) {
	console.error(error);
}
```
````

---

# Delete

````md magic-move
<!-- Define function -->

```js
async function deleteData(store, key) {
	const request = store.delete(key);

	return await requestResult(request);
}
```

<!-- Example -->

```js
async function deleteData(store, key) {
	// ...
}

const transaction = startTransaction(db, ['todos'], 'readwrite');

const objectStore = getObjectStore(transaction, 'todos');

const key = 1;

try {
	await deleteData(objectStore, key);
} catch (error) {
	console.error(error);
}
```
````

---
layout: center
---

# Mastered CRUD operations!

---

# Summary

- IndexedDB is a powerful, flexible, and efficient NoSQL database API.

- IndexedDB is suitable for building offline-first web applications.

- IndexedDB is more complex than localStorage but provides more features.
