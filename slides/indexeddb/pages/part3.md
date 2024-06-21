---
layout: two-cols
---

# Saving data

1. Open database<span v-if="$clicks === 3" class="line spread"></span>

2. Get database<span v-if="$clicks === 4 || $clicks === 5" class="line spread"></span>

3. Create an object store<span v-if="$clicks > 5 && $clicks < 9" class="line spread"></span>

4. Start traction<span v-if="$clicks === 9" class="line spread"></span>

5. Access an object store<span v-if="$clicks === 10" class="line spread"></span>

6. Request to add data<span v-if="$clicks === 11" class="line spread"></span>

7. Add event handler<span v-if="$clicks === 12" class="line spread"></span>

::right::

<v-click>

````md magic-move {at: 2}
<!-- Prepare initial data -->

```js
const initialData = {
	id: 1,
	todo: 'Learn indexedDB',
	complete: false,
	createdAt: Date.now(),
};
```

<!-- Define variable to store database objects -->

```js
const initialData = {
	id: 1,
	todo: 'Learn indexedDB',
	complete: false,
	createdAt: Date.now(),
};

let db;
```

<!-- Open database -->

```js {3}
let db;

const openRequest = indexedDB.open('todoApp', 1);
```

<!-- add handler when success or error -->

```js {5-7,8-10}
let db;

const openRequest = indexedDB.open('todoApp', 1);

openRequest.onsuccess = (event) => {
	// Successfully opened database
};

openRequest.onerror = (event) => {
	// Failed to open database
};
```

<!-- Listen onupgradeneeded event -->

```js {3-6}
const openRequest = indexedDB.open('todoApp', 1);

openRequest.onupgradeneeded = (event) => {
	// Execute when creating a new database
	// or changing database version
};
```

<!-- Get database -->

```js {3-5}
const openRequest = indexedDB.open('todoApp', 1);

openRequest.onupgradeneeded = (event) => {
	db = event.target.result;
};
```

<!-- Create an object store -->

```js {4}
openRequest.onupgradeneeded = (event) => {
	db = event.target.result;

	db.createObjectStore('todos', { keyPath: 'id' });
};
```

<!-- (optional) set key by autoIncrement -->

```js {4-8}
openRequest.onupgradeneeded = (event) => {
	db = event.target.result;

	db.createObjectStore('todos', { keyPath: 'id' });

	// or

	db.createObjectStore('todos', { autoIncrement: true });
};
```

<!-- Back prev code -->

```js{4}
openRequest.onupgradeneeded = (event) => {
  db = event.target.result;

  db.createObjectStore('todos', { keyPath: 'id' });
}
```

<!-- Create an transaction -->

```js {6}
openRequest.onupgradeneeded = (event) => {
	db = event.target.result;

	db.createObjectStore('todos', { keyPath: 'id' });

	const transaction = db.transaction(['todos'], 'readwrite');
};
```

<!-- Access an object store -->

```js {8}
openRequest.onupgradeneeded = (event) => {
	db = event.target.result;

	db.createObjectStore('todos', { keyPath: 'id' });

	const transaction = db.transaction(['todos'], 'readwrite');

	const objectStore = transaction.objectStore('todos');
};
```

<!-- Request to add data -->

```js {10}
openRequest.onupgradeneeded = (event) => {
	db = event.target.result;

	db.createObjectStore('todos', { keyPath: 'id' });

	const transaction = db.transaction(['todos'], 'readwrite');

	const objectStore = transaction.objectStore('todos');

	const addRequest = objectStore.add(initialData);
};
```

<!-- Add event handler -->

```js {12-14,16-18}
openRequest.onupgradeneeded = (event) => {
	db = event.target.result;

	db.createObjectStore('todos', { keyPath: 'id' });

	const transaction = db.transaction(['todos'], 'readwrite');

	const objectStore = transaction.objectStore('todos');

	const addRequest = objectStore.add(initialData);

	addRequst.onsuccess = (event) => {
		// Successfully added data!
	};

	addRequest.onerror = (event) => {
		// Failed to add data...
	};
};
```
````

</v-click>

<style>
  li {
    position: relative;
    width: fit-content;
  }
</style>

---

# Summary

- Access the database using indexedDB.open().

- If you want to perform any operations during the database initialization, add a handler to the onupgradeneeded event.

- The onsuccess event occurs when each asynchronous request succeeds, and the onerror event occurs when it fails.

- To add data, create a transaction and perform write operations on the object store within that transaction.

- By simply passing a callback function to db.onerror, you can handle errors for each request made to the database.
