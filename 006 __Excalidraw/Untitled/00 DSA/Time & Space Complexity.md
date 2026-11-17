Time Complexity -> 
- Is measures how the execution time grows as the input size increase

1. O(1) — Constant time
	- No matter of size of array it takes the same time
	- - Doesn’t use loops
	- Doesn’t depend on input size
	- Executes a fixed number of steps
```js
let len = arr.length;
//----------------------------
let value = arr[5];
//----------------------------
let first = arr[0];
let last = arr[arr.length - 1];
//----------------------------
let result = a + b;
let square = x * x;
```

---
2. O(n) — Linear Time
	- The time taken by code grows directly with the size of the input.
	
- If array has 5 elements → 5 steps
- If array has 100 elements → 100 steps
```js
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
//----------------------------
let sum = 0;
for (let i = 0; i < arr.length; i++) {
  sum += arr[i];
}
//----------------------------
function find(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return true;
  }
  return false;
}
```

Your code is **O(n)** if:

- It loops through the input once
- Work increases linearly with input size

| Input Size (n) | Steps in O(n) |
| -------------- | ------------- |
| 10             | 10 steps      |
| 100            | 100 steps     |
| 1000           | 1000 steps    |

---
3.  **O(n²) — Quadratic time**
	- **O(n²)** means the runtime grows **proportionally to the square of the input size**.

 👉 If input is 10 → ~100 operations  
👉 If input is 100 → ~10,000 operations

```js
for (let i = 0; i < n; i++) {
  for (let j = 0; j < n; j++) {
    console.log(i, j);
  }
}
//           nested loops → O(n²)

function bubbleSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
}
```

|n|Operations (approx)|
|---|---|
|10|100|
|100|10,000|
|1000|1,000,000|

---
4.  ⚡ O(log n) — Logarithmic Time (JavaScript)
- **O(log n)** means the algorithm reduces the problem size **by half (or a fraction) each step**.

### T(n)=T(n/2)+1

```js
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while ( left <= right ) {
    let mid = Math.floor((left + right) / 2);

    if if ( arr[mid] < target ) {
	    left = mid + 1;
    } else if ( arr[min] > target ) {
	    right = mid - 1;
    } else {
	    return min
    }
  }
}
```

n → n/2 → n/4 → n/8 → .... → 1

### 1. Finding element in sorted data

- Binary search (above)
- Works only on **sorted arrays**
### 2. Dividing a number repeatedly
```js
let n = 100;

while (n > 1) {
  n = Math.floor(n / 2);
  console.log(n);
}
```

---
---
Space Complexity
- How much extra memory code uses as input size grows
