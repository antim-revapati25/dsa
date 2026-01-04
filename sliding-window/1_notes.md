# DSA Revision Notes (Personal)

## Sliding Window

### First Negative Integer in Every Window of Size k

🔗 Problem Link
[https://www.geeksforgeeks.org/problems/first-negative-integer-in-every-window-of-size-k3345/1](https://www.geeksforgeeks.org/problems/first-negative-integer-in-every-window-of-size-k3345/1)

📌 **Problem**
Given an array `arr[]` and an integer `k`, return an array containing the first negative integer in every contiguous subarray (window) of size `k`.
If a window does not contain a negative integer, store `0`.

---

### ✅ Approach 1: Brute Force (Baseline / Naive)

💡 **Idea**
For every window of size `k`, scan all elements inside the window and pick the first negative number.

🧠 **Steps**

* Iterate over all valid window starting indices `(0 to n - k)`
* For each window:

  * Traverse elements from `i` to `i + k - 1`
  * As soon as a negative number is found, store it
  * If no negative number is found, store `0`

⏱️ **Complexity**

* Time: `O(n × k)`
* Space: `O(1)` (excluding output)

#### ✅ Code (Brute Force)

```js
class Solution {

    firstNegInt(arr, k) {

        let ans = [];

        for (let i = 0; i <= arr.length - k; i++) {
            let found = false;

            for (let j = i; j < i + k; j++) {
                if (arr[j] < 0) {
                    ans.push(arr[j]);
                    found = true;
                    break;
                }
            }

            if (!found) ans.push(0);
        }

        return ans;
    }
}
```

---

### ✅ Approach 2: Optimized (Sliding Window + Queue)

💡 **Idea**
Instead of scanning the window every time, store all negative numbers of the current window in a queue.

* Queue front → first negative number of window
* Remove elements when they go out of window

🧠 **Steps**

* Use two pointers: `start` and `end`
* Use a queue to store negative numbers
* For each `end`:

  * Add `arr[end]` if it is negative
  * When window size becomes `k`:

    * Push queue front to answer (or `0` if empty)
    * If outgoing element equals queue front → remove it
    * Slide window

⏱️ **Complexity**

* Time: `O(n)`
* Space: `O(k)`

#### ✅ Code (Optimized Sliding Window)

```js
class Solution {

    firstNegInt(arr, k) {

        let ans = [];
        let start = 0;
        let end = 0;
        let negative = [];

        while (end < arr.length) {

            // Add incoming element
            if (arr[end] < 0) {
                negative.push(arr[end]);
            }

            // When window size becomes k
            if (end - start + 1 === k) {

                // Store answer
                ans.push(negative.length ? negative[0] : 0);

                // Remove outgoing element if needed
                if (negative.length && negative[0] === arr[start]) {
                    negative.shift();
                }

                // Slide window
                start++;
            }

            end++;
        }

        return ans;
    }
}
```

📌 **Key Invariants (Very Important for Revision)**

* Queue always stores only negative numbers
* Queue front is always the first negative in the window
* Incoming element is processed **before** checking window size
* Outgoing element is removed **only if it matches queue front**

---

## Count Anagrams

🔗 Problem Link
[https://www.geeksforgeeks.org/problems/count-occurences-of-anagrams5839/1](https://www.geeksforgeeks.org/problems/count-occurences-of-anagrams5839/1)

```js
class Solution {
    search(pat, txt) {
        let ans = 0;
        let map = new Map();
        let count =0;
        
        for(let x of pat){
          map.set(x,(map.get(x)||0)+1);
        }
        count = map.size;
        
        let start =0,end=0;
        while(start<=end && end<txt.length){
          let e = txt.charAt(end);
          let s = txt.charAt(start);
          
          if(map.has(e)){
            map.set(e,map.get(e)-1);
            if(map.get(e)===0) count--;
          }
          
          if(end-start+1===pat.length){
            if(count===0)ans++;
            if(map.has(s)){
              if(map.get(s)==0) count++;
              map.set(s,map.get(s)+1);
            }
            start++;
          }
          end++;
        }
        return ans;
    }
}
```

### Summary Logic

* Store frequency of pattern characters in map
* Maintain `count` = number of unique chars not matched
* Expand window
* When window size matches pattern length:

  * If `count === 0` → anagram found
  * Remove start character (reverse logic)

---

## Fixed Size Window – General Logic

```
CALCULATE → CHECK WINDOW → STORE ANSWER → SLIDE WINDOW
```

---

## Variable Size Window

---

## Longest Subarray With Sum K

🔗 [https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_6682399](https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_6682399)

### Logic: First Approach

```js
function longestSubarrayWithSumK(arr, k) {
    let sum=0,start=0,end=0,max=0;

    while(end<arr.length){
        sum+=arr[end];
        if(sum===k){
            max = Math.max(max,end-start+1);
            end++;
        }
        else if(sum>k){
            while (sum > k && start<=end) {
                sum-=arr[start];
                start++;
            }
            if(sum===k){
                max = Math.max(max,end-start+1);
            }
            end++;
        }else{
            end++;
        }
    }
    return max;
}
```

### Logic: Second Approach

```js
let sum=0,start=0,end=0,max=0;

while(end<arr.length){
    sum += arr[end];
    while (sum > k) {
        sum -= arr[start];
        start++;
    }
    if (sum === k) {
        max = Math.max(max, end - start + 1);
    }
    end++;
}
```

---

## Binary Search (Basics)

```js
var search = function(arr, t) {
    let ind =-1;
    let start =0,end = arr.length-1;
    while(start<=end){
        let mid = Math.floor(start +(end-start)/2);
        if(arr[mid]===t){
            ind = mid;
            return ind;
        }else if(arr[mid]>t){
            end=mid-1;
        }else{
            start=mid+1;
        }
    }
    return ind;
};
```

---

## Recursion

### Basics

* Recursion = Base case + current work + recursive call
* Backtracking = recursion + undo

### Sum of n Numbers

```js
function sum(n){
  if(n===0) return 0;
  return n+sum(n-1);
}
```

### Print 1 to n

```js
function print(n) {
  if (n === 0) return;
  print(n - 1);
  console.log(n);
}
```

### Reverse Array

```js
let arr = [1, 2, 3, 4, 5];

function reverse(left, right) {
  if (left >= right) return;
  let temp = arr[left];
  arr[left] = arr[right];
  arr[right] = temp;
  reverse(++left, --right);
}
```

### Fibonacci

```js
var fib = function(n) {
    function fib(n){
        if(n==0||n==1)return n;
        else return fib(n-1)+fib(n-2);
    }
    return fib(n);
};
```

### Subsequence

* Contiguous / Non‑contiguous
* Order must be preserved

---

## ✅ End of Personal DSA Revision Notes
