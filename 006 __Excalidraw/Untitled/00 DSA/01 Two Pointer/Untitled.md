### Step 1: Sort the array
```js
arr.sort((a, b) => a - b);
```

### Step 2: Fix one number
```js
for (let i = 0; i < arr.length - 2; i++)
```

### Step 3: Use Two Pointers
```js
let left = i + 1;
let right = arr.length - 1;
```

### Step 4: Calculate Sum
```js
const sum = arr[i] + arr[left] + arr[right];
```
