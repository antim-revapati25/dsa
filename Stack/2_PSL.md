# Previous Smaller Element — Complete Explanation (All Approaches, Fully Detailed)

This document explains **every detail** of the logic behind finding the **Previous Smaller Element (PSE)** for each element in the array.

You used two approaches:

1. **Brute Force (O(n²)) — slow, repeated scanning**
2. **Optimized Monotonic Stack (O(n)) — clean and efficient**

This is written so you can understand it even after **5+ years**.

---

# 📌 Problem Statement

For every element in the array, find the **nearest smaller element to its *left***.

* If there is no smaller element on the left → return `-1`.

Example:

```
Input : [1, 5, 0, 3, 4, 5]
Output: [-1, 1, -1, 0, 3, 4]
```

---

# 📌 Given Array

```
[1, 5, 0, 3, 4, 5]
```

We must process elements **from left → right**, because previous elements lie on the left.

---

# 🧠 APPROACH 1 — BRUTE FORCE (O(n²))

### ✔️ Code you wrote

```js
for (let i = 0; i < n; i++) {
  let curr = arr[i];
  if (stack.length === 0) {
    ans.push(-1);
  } else if (stack.length > 0 && stack[stack.length - 1] >= curr) {
    while (stack.length > 0 && stack[stack.length - 1] >= curr) {
      stack.pop();
    }
    ans.push(stack.length === 0 ? -1 : stack[stack.length - 1]);
  }
  else {
    ans.push(stack[stack.length-1]);
  }
  stack.push(curr);
}
```

### ❓ What this tries to do

For each element, you try to identify the nearest smaller number to the **left**.

But this logic still tries to use stack in a brute-force-ish way:

* It checks stack top (previous element)
* If ≥ current → pop
* Continue until you find smaller

So pop operations simulate the brute-force way of going backwards.

### 😫 Why it feels complicated

* Too many if/else blocks
* Hard to read
* Mixing brute-force logic with stack
* Conditions are duplicated

### ❌ True Brute Force (just for reference)

For clarity, true brute force looks like:

```js
for (let i = 0; i < n; i++) {
  let found = false;
  for (let j = i - 1; j >= 0; j--) {
    if (arr[j] < arr[i]) {
      ans.push(arr[j]);
      found = true;
      break;
    }
  }
  if (!found) ans.push(-1);
}
```

This is clearly O(n²).

---

# 🤔 Why move to the optimized approach?

Because brute force:

* Checks previous elements again and again
* Repeats work unnecessarily
* Very slow for large input

We need a way to **remember** previous useful elements.

That leads to the **Monotonic Increasing Stack**.

---

# 🧠 APPROACH 2 — CLEAN MONOTONIC STACK (O(n))

### ✔️ Code you wrote

```js
for (let i = 0; i < n; i++) {
  let curr = arr[i];

  // remove all elements that are >= curr
  while (stack.length > 0 && stack[stack.length - 1] >= curr) {
    stack.pop();
  }

  // after popping:
  ans.push(stack.length === 0 ? -1 : stack[stack.length - 1]);

  // push current element
  stack.push(curr);
}
```

---

# 📘 Explanation — What changed?

You simplified the logic by focusing on **the only thing that matters**:

### 🎯 KEY IDEA (Very Important)

To find the **previous smaller** element:

* Any element **greater than or equal** to current can never help
* So remove them (pop)

Once all such elements are removed:

* If stack is empty → no smaller element exists
* Else → top of stack is the nearest smaller element

Then push `curr`.

---

# 🧱 Why does this work?

Because the stack always maintains a **strictly increasing order**.

Try tracking the stack:

* Each time you push an element, all bigger elements before it are removed
* So the stack only stores "valid candidates" for future elements

This prevents redundant backward scanning.

---

# 🧠 Big Picture (Lifetime Memory Trick)

Think like this:

> "I am moving left → right. I want the previous smaller.
>
> So I keep a stack where the top always contains the nearest smaller candidate.
>
> Before using the top, I remove everything bigger or equal — because they can never help me."

This is the essence of a **monotonic increasing stack**.

---

# 🔍 Example Walkthrough

Given array:

```
[1, 5, 0, 3, 4, 5]
```

### i = 0 → 1

Stack empty → answer = -1
Push 1 → stack = [1]

### i = 1 → 5

Stack top = 1 < 5 → answer = 1
Push 5 → stack = [1,5]

### i = 2 → 0

Pop 5 (>=0)
Pop 1 (>=0)
Stack empty → answer = -1
Push 0 → stack = [0]

### i = 3 → 3

Stack top = 0 < 3 → answer = 0
Push 3 → stack = [0,3]

### i = 4 → 4

Stack top = 3 < 4 → answer = 3
Push 4 → stack = [0,3,4]

### i = 5 → 5

Stack top = 4 < 5 → answer = 4
Push 5 → stack = [0,3,4,5]

Final answer:

```
[-1, 1, -1, 0, 3, 4]
```

---

# 🧱 Why this approach is the best

✔️ Strict O(n) time
✔️ Very clean logic
✔️ Same monotonic stack pattern used in interviews
✔️ Works for previous smaller & next smaller
✔️ Code is minimal but powerful

---

# 📌 Summary Table

| Approach              | Time  | Logic                       | Drawbacks               |
| --------------------- | ----- | --------------------------- | ----------------------- |
| Brute Force           | O(n²) | Check all previous elements | Too slow, repeated work |
| Stack Version 1       | O(n)  | Mixed brute + stack         | Messy conditions        |
| Clean Monotonic Stack | O(n)  | Pure, elegant logic         | Best version            |

---

# 🎯 Final Conclusion

Mastering monotonic stacks is essential for many problems:

* Previous Smaller Element
* Next Smaller Element
* Previous Greater Element
* Next Greater Element
* Stock Span
* Histogram Largest Rectangle

Your final clean approach is exactly how professionals solve these problems.

---

# 🎉 End of Notes
