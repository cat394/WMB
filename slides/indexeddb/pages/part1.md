# What is IndexedDB?

IndexedDB is a JavaScript API provided by web browsers for managing a NoSQL database of objects.

<div class="grid h-full grid-cols-[repeat(3,1fr)] items-center justify-items-center gap-8">
  <feature-card
    title="Powerful"
    description="IndexedDB features powerful and flexible queries for searching data."
  >
      <clarity-storage-line class="text-red" />
  </feature-card>
  <feature-card
    title="Simple" 
    description="Object-based database familiar to JavaScript developers."
  >
      <material-symbols-light-data-object class="text-blue" />
  </feature-card>
  <feature-card
    title="Efficient"
    description="Asynchronous APIs happily accommodate large amounts of data."
  >
      <material-symbols-light-sync-disabled class="text-green" />
  </feature-card>
</div>

---

# IndexedDB vs LocalStorage

| IndexedDB                           |                     | localStorage      |
| ----------------------------------- | ------------------- | ----------------- |
| Large                               | Data size           | Small             |
| Most JavaScript values              | Storable Data Types | Strings only      |
| Async                               | API                 | Sync              |
| Yes                                 | Search / Query      | No                |
| Store large amounts of complex data | Use cases           | Store simple data |

<style>
  th, td {
    text-align: center;
  }
  th:first-child, th:last-child {
    color: transparent;
    filter: brightness(1.5);
    font-weight: bold;
  }
  th:first-child {
    background-image: linear-gradient(315deg, #ffff45 0%, #ff5858 74%);
    background-clip: text;
  }
  th:last-child {
    background-image: linear-gradient(-135deg, #92FFC0 10%, #002661 100%);
    background-clip: text;
  }
  tr {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
  }
</style>

---

# Summary

- IndexedDB is an ultra-high-performance database that runs in the browser.

- IndexedDB is easy to use localStorage if you just want to handle simple data.

- IndexedDB is the best for storing data of web apps when they are offline.
