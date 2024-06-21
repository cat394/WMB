---
layout: center
---

# How to get started?

---

# Flow of Saving data

1. Open database

2. Get database

3. Create an <span class="leading-none" v-mark="{ at: 1, color: 'red' }">object store</span>

4. Create a <span class="leading-none" v-mark="{ at: 2, color: 'red' }">transaction</span>

5. Access an object store

6. Request to add data

7. (optional) Add event handler

---

# What is object store?

Object store is a storage for saving data in the form of key-value pairs in a database.

<div class="h-10"></div>

<div 
  v-click 
  v-motion
  :initial="{ x: -500, opacity: 0 }"
  :enter="{
    x: 0,
    opacity: 1,
    transition: {
      duration: 1000,
      type: 'keyframes',
      ease: 'ease-in-out'
    }
  }"
>
  <chart-container 
    title="Database A" 
    title-size="large"
    container-class="py-10 pl-20 pr-10 border-red text-lg"
  >
    <div class="grid grid-cols-2 gap-15">
      <chart-container
        title="Object Store A"
        title-position="center"
        container-class="p-7 border-blue"
      >
        <chart-store :keys="keys1"></chart-store>
      </chart-container>
      <chart-container
        title="Object Store B" 
        title-position="center" 
        container-class="p-7 border-blue"
      >
        <chart-store :keys="keys2"></chart-store>
      </chart-container>
    </div>
  </chart-container>
</div>

<script setup>
  const keys1 = ['1', '2', '3'];
  const keys2 = ['A', 'B', 'C'];
</script>

---
clicks: 6
---

# What is transaction?

Transaction is a collection of read and write operations against a set of database.

<div class="my-10">
  <v-click>
    <div class="grid grid-cols-2 gap-10">
      <chart-container
        class="relative"
        title="Transaction" 
        title-size="large" 
        :container-class="['p-7', 'w-fit', {'border-red-700': $clicks >= 5 }]"
      >
        <div class="flex gap-3">
          <div 
            :class="['pretty-btn', 'slide-in-from-left', { 'blink': $clicks === 3 }, { 'bg-neutral-700 text-gray': $clicks >= 5 }]"
            style="transition-delay: 1s;"
            @animationend="() => endOperation(1)"
            @transitionend="() => invalidPreOperation()"
          >Operation 1</div>
          <div
            :class="['pretty-btn', 'slide-in-from-left', { 'blink': $clicks === 4 }]"
            style="--btn-bg-color: #96ae3e; transition-delay: 1.5s;"
            @animationend="() => endOperation(2)"
          >Operation 2</div>
          <div
            :class="['pretty-btn', 'slide-in-from-left']"
            style="--btn-bg-color: #b751af; transition-delay: 2s;"
          >Operation 3</div>
        </div>
        <material-symbols-light-brightness-alert-outline
          v-if="$clicks >= 5"
          class="w-[2rem] h-[2rem] absolute top-[-20px] right-[-1rem] text-red-700 font-bold bg-[var(--dark-bg)]"
        />
      </chart-container>
      <div class="relative w-fit">
        <clarity-storage-line
          :class="['w-[80px]', 'h-[80px]', 'transition-colors', 'duration-700', {'text-blue': isFirstOperationEnd && $clicks < 5}, {'text-red-700': isErrorOperationEnd && !isRollback}, {'text-current': isRollback}]" 
        />
        <material-symbols-light-brightness-alert-outline
          v-if="isErrorOperationEnd && $clicks < 6"
          class="w-[2rem] h-[2rem] absolute top-[-25px] right-[-16px] text-red-700"
        />
        <div class="absolute top-[-25px] right-[-5rem] flex items-center">
           <material-symbols-light-autorenew
            v-if="$clicks >= 6"
            class="rotate w-[2rem] h-[2rem]"
            style="animation-duration: 1.2s;"
            @animationend="() => rollback()"
           />
           <span v-if="$clicks >= 6">Roll back</span>
        </div>
      </div>
    </div>
  </v-click>
</div>

<v-click>

## Why are transactions necessary?

- Ensures data integrity

- Prevents data corruption

</v-click>

<style>
  li::marker {
    content: '✅';
  }
</style>

<script setup>
import { ref } from 'vue';
const isFirstOperationEnd = ref(false);
const isErrorOperationEnd = ref(false);
const isPreOperationInvalid = ref(false);
const isRollback = ref(false);

function endOperation(operationNumber) {
  switch (operationNumber) {
    case 1:
      isFirstOperationEnd.value = true;
      break;
    case 2:
      isErrorOperationEnd.value = true;
      break;
  }
}

function invalidPreOperation() {
  isPreOperationInvalid.value = true;
}

function rollback() {
  isRollback.value = true;
}
</script>

---

# Summary

- Object store is a repository of data in indexedDB.<br />This stores data in the form of records composed of keys and values.

- Transaction is a set of operations on a database.<br />This ensures the integrity of the database.
