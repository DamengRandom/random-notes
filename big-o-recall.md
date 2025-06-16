Why Big O?

- To understand the performance of an algorithm
- To compare the performance of different algorithms
- To predict the performance of an algorithm
- To measure how much memory an algorithm uses


Big O Notation: How code slow as data grows

- O(1) - Constant Time
- O(n) - Linear Time
- O(n^2) - Quadratic Time
- O(log n) - Logarithmic Time
- O(n log n) - Log-Linear Time


Example one: O(1) -> Constant time (since no new data structure is created !!)

```javaScript
function sum(arr) {
  return arr[0]; // O(1)
}
```


Example two: O(n^2) -> Quadratic time

```javaScript
// bubble sort
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        // Swap elements
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return arr;
}
// Average/Worst Case: O(n²)
```

```javaScript
// get all subarrays
function getAllSubarrays(arr) {
  const subarrays = [];
  
  for (let i = 0; i < arr.length; i++) {
    for (let j = i; j < arr.length; j++) {
      subarrays.push(arr.slice(i, j + 1));
    }
  }
  
  return subarrays;
}
// O(n²) - number of subarrays grows quadratically
```



Example three: O(n + a) = O(n) → Linear time

```javaScript
function sum(arr) {
  let total = 0; // O(1)
  for (let j = 0; j < arr.length; j++) { // O(n)
    total += arr[j]; // O(1)
  }
  for (let i = 0; i < arr.length; i++) { // O(n)
    total += arr[i]; // O(1)
  }
  return total; // O(1)
}
```

## Time Complexity vs Space Complexity

Time Complexity:

Time complexity measures how the runtime of an algorithm grows as the input size increases.

- How many operations does it take to complete?
- How long will it take as input grows?

Example: O(1) -> Constant time

```javaScript
function sum(arr) {
  return arr[0]; // O(1)
}
```

Space Complexity:

Space complexity measures how the memory usage of an algorithm grows as the input size increases.

- How much memory does it take to run the algorithm?
- How much memory will it take as input grows?

Example: O(n) -> Linear space

```javaScript
function createArray(n) {
  let arr = []; // O(1)
  for (let i = 0; i < n; i++) { // O(n)
    arr.push(i); // O(1)
  }
  return arr; // O(1)
}
```

Concept | Time Complexity | Space Complexity
Measures | How fast it runs | How much memory it uses
Unit | Number of operations | Amount of memory
Example O(1) | One operation | One variable (❗️)
Example O(n) | Loop over input | Store n items (❗️)



## Some complex examples:

1. Sliding Window Technique:

```javaScript
function findLongestSubstring(str) {
  let longest = 0;
  let seen = new Set();
  let start = 0;

  for (let i = 0; i < str.length; i++) {
    while(seen.has(str[i])) {
      seen.delete(str[start]);
      start++;
    }
    seen.add(str[i]);
    longest = Math.max(longest, i - start + 1);
  }
  return longest;
}
// the total number of operations inside while loop is 2n at most which is linear. Thus, this is O(n) ~
```

2. Recursion: O(n) -> n recursive calls x O(1) = O(n) !!!!

```javaScript

function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}
```



## Order for the time complexity:

O(n!) > O(c ^ n) > O(2 ^ n) > O(n ^ 3) > O(n ^ 2) > O(n * log n) > O(n) > O(log n) > O(1)

O(n!) -> factorial time (most expensive time complexity) - better avoid it inside codebase !!!!

O(c ^ n), O(2 ^ n): recursion tree based loops

O(n ^ 3) > O(n ^ 2): 2 dimentional array, 2 - 3 nested loops

O(n * log n) -> 2 ^ n = x, n = log x -> heapsort, mergesort, quicksort

O(n) -> linear search

O(log n) -> binary search

O(1) -> constant time
