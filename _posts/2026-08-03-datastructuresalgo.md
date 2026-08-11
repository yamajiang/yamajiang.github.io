---
layout: distill
title: Data Structures & Algorithms 
description: >-
  DSA Topics and Notes <br>
tags:
  - notes
published: false
giscus_comments: false
hide_date: true
featured: false

_styles: >
  .post.distill p,
  .post.distill li {
    line-height: 1.5;
    margin-bottom: 0.45rem;
  }

  .post.distill h2,
  .post.distill h3 {
    margin-top: 1rem;
    margin-bottom: 0.35rem;
  }

  .post.distill ul {
    margin-top: 0.25rem;
    margin-bottom: 0.5rem;
  }

toc:
  - name: Background
    subsections:
      - name: Big O Notation
      - name: Recursion
  - name: Arrays and Strings
    subsections:
      - name: Two Pointers
      - name: Sliding Window
   

---
## Background
### Big O Notation 
Big O notation is an important concept in computer science. It is used to help us describe the time complexity and space complexity for a particular algorithm. 
- **Time Complexity**: how the run time changes as the input size (```n```) changes 
- **Space Complexity**: how much memory the algorithm requires when input size (```n```)  changes
- Big O notation describes the asymptotic upper bound on an algorithm's growth rate as the input size (n) becomes very large. It is commonly used to describe worst case time or space complexity, although it can also describe best case or average case complexity when specified
- We ignore constants and lower order terms because they become insignificant as n grows larger 
- Algorithms can be analyzed using three cases: **Best Case**, **Average Case**, and **Worst Case**. These cases may differ depending on the algorithm and input conditions 
- **Worst-case scenario** is typically chosen to represent an algorithm because it provides a reliable upper bound on performance.

#### Properties of Big O Notation
- **Reflexivity:** A function is always Big O of itself (For function `f(n)`, `f(n) = O(f(n))`)
- **Transitivity:** If `f(n) = O(g(n))` and `g(n) = O(h(n))`, then `f(n) = O(h(n))`
- **Constant Factor Rule:** Constants can be ignored because they do not affect growth rate (`O(2n)` → `O(n)`)
- **Sum Rule:** When adding complexities, keep only the dominant term (`O(n² + n)` → `O(n²)`)
- **Product Rule:** When multiplying operations, multiply their complexities (`O(n) * O(n)` → `O(n²)`)
- **Composition Rule:** When one operation is performed inside another (such as nested loops or a function called repeatedly inside a loop), multiply their complexities (If `f(n) = O(g(n))`, then `f(h(n)) = O(g(h(n)))`)

#### Analyzing Time Complexity 
Time complexity is calculated by counting how the number of operations executed by the algorithm changes as input size (```n```) changes 

**General Steps**:
1. Identify the basic operations performed by the algorithm
2. Determine how many times each operation executes relative to n
3. Analyze loops, nested loops, conditionals, and recursive calls
4. Combine the resulting complexities
5. Remove constant factors and lower order terms
6. Express the result using Big O notation


**Common Patterns**:
- Single operation → `O(1)`
- Single loop → `O(n)`
- Two consecutive loops → `O(n)`
- Nested loops → `O(n²)`
- Three nested loops → `O(n³)`
- Divide the input in half each iteration → `O(log n)`
- Loop with a logarithmic operation → `O(n log n)`
- Recursive branching into two calls → Often `O(2ⁿ)`
- Generate every permutation → `O(n!)`


#### Analyzing Space Complexity

Space complexity measures how the amount of memory an algorithm requires changes as the input size (`n`) changes

When analyzing space complexity, we focus on the additional memory required by the algorithm like variables, arrays, lists, hash tables, and recursive call stacks.

**General Steps**:
1. Identify the additional memory allocated by the algorithm
2. Determine whether that memory grows with the input size (n)
3. Analyze data structures such as arrays, lists, and hash tables
4. Analyze recursive algorithms because each recursive call consumes stack space
5. Ignore constant factors and lower order terms
6. Express the result using Big O notation


#### Common Big O Complexities

| Complexity | Description | Example |
|---|---|---|
| **Constant Time (`O(1)`)** | The operation takes the same amount of time regardless of how large the input is. | Accessing an array element: `arr[5]` |
| **Linear Time <br>  (`O(n)`)** | The time increases directly with the size of the input. If the input doubles, the work roughly doubles. | Searching through an unsorted array |
| **Logarithmic Time (`O(log n)`)** | The input is repeatedly divided into smaller portions, so the amount of work grows very slowly as the input increases. | Binary search |
| **Linearithmic Time (`O(n log n)`)** | Combines linear and logarithmic growth. Common in efficient sorting algorithms. | Merge sort, Heap sort |
| **Quadratic Time (`O(n²)`)** | The amount of work grows roughly with the square of the input. Often occurs with two nested loops. | Comparing every element with every other element |
| **Cubic Time (`O(n³)`)** | The amount of work grows with the cube of the input. Often occurs with three nested loops. | Three nested loops over an array |
| **Polynomial Time (`O(nᵏ)`)** | A general category where the input is raised to some fixed power `k`. Includes `O(n²)`, `O(n³)`, etc. | Some dynamic programming algorithms |
| **Exponential Time (`O(2ⁿ)`)** | The amount of work roughly doubles every time the input increases by one. Becomes impractical quickly. | Generating all possible subsets |
| **Factorial Time (`O(n!)`)** | The algorithm performs work for every possible ordering of the input. Grows extremely quickly. | Generating all permutations |

#### Growth Rates 

Fastest → Slowest:

`O(1)` → `O(log n)` → `O(n)` → `O(n log n)` → `O(n²)` → `O(2ⁿ)` → `O(n!)`

As n becomes larger, algorithms with slower growth rates generally scale better 

### Recursion 
Recursion is a function that calls itself. <br>
Each recursive function has recursive case that continues the execution of that function and a base case that is used to stop the process. 

- The opposite of recursive is an iterative algorithm which uses loops to repeat blocks of codes until a set condition is met. 
- All iterative functions can be written recursively 

<u>Example: </u>

A factorial (n!) is where you multiple a number (n) by every whole number that is less than it down to one.

``` n! = n × (n-1) × (n-2) × ... × 1 ```
ex: ``` 5! = 5 × 4 × 3 × 2 × 1 = 120 ```

A factorial written iteratively :

```
def factorial(n):
  if n == 0:
      return 1

  result= 1
  
  for i in range(2, n + 1):
    result *= i

  return result  
```

vs recursively: 

```
def factorial(n):
  if n == 0:
    return 1
  
  return n * factorial(n-1)
```

#### Fibonacci Numbers
The Fibonacci sequence is a sequence of numbers where each number is the sum of the previous two numbers.

```F(n) = F(n-1) + F(n-2)```

The sequence starts with 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ... where: <br>
0 + 1 = 1 <br>
1 + 1 = 2<br>
1 + 2 = 3<br>
2 + 3 = 5<br>
3 + 5 = 8<br>

This is called a recurrence relation because it defines a value in terms of previous values.

Because each Fibonacci number is defined using the previous two numbers with the base cases of F(0) = 0 and F(1) = 1, Fibonacci is a natural example of recursion.

```
def fibonacci(n):

    if n <= 1:
        return n

    return fibonacci(n - 1) + fibonacci(n - 2)
```

Fibonacci is a good example of recursion because the original problem can be broken into smaller versions of the same problem. 
- For example if we need to solve F(5):
  - We need F(4) + F(3)
  - To solve F(4), we need F(3) + F(2)
  - And so on...

## Arrays and Strings
Arrays and strings are both data structures that stores collections of data.
- Mutable is a type of data that can be changed
  - Examples: list, dict, set
- Immutable data cannot be changed 
  - Examples: str, tuple, int

### Arrays
Python uses lists as dynamic arrays. <br> 
Arrays are initialized with brackets: 

``` arr = [] ``` <br>
``` nums = [1, 2, 3]```

They are of the most common data structures besides hash maps in Python.
- Arrays are mutable, ordered, and allow duplicate values 

<u>Common Operations: </u>

```
arr.append(4)      # Add to end
arr.pop()          # Remove last element
arr.pop(0)         # Remove first element (O(n))
arr.insert(1, 10)  # Insert at index
arr.remove(10)     # Remove first occurrence
len(arr)           # Length

arr[0]             # Access first element
arr[-1]            # Access last element
arr[1:4]            # Slice

# loop through arrays 
# using index:
for i in range(len(nums)):
	print(nums[i])

# without index:
for n in nums:
	print(n)

# with index and value 
for i, n in enumerate(nums):
	print(i, n)

# iterate through multiple arrays simultaneously with unpacking 
# zip takes both arrays and combines them into an array of pairs 

nums1 = [1, 3, 5]
nums2 = [2, 4, 6]

for n1, n2 in zip(nums1, nums2): 
	print(n1, n2)
# gives you:
# 1 2
# 3 4
# 5 6


#reversing array 
nums = [1, 2, 3]
nums.reverse() 
print(nums) # -> [3, 2, 1]

```

<u> Time Complexity:</u>

| Operation               | Time         |
| ----------------------- | ------------ |
| Access                  | O(1)         |
| Update                  | O(1)         |
| Append                  | O(1) average |
| Pop from end            | O(1)         |
| Insert/Delete beginning | O(n)         |
| Search                  | O(n)         |

### Strings 
Strings are a sequence of characters that are immutable. If you want to change something, you will need to create a new string.

``` s = "hello" ``` 

```
s[0]      # h
s[-1]     # o
s[1:4]    # ell
```

<u>Common Operations: </u>

```

len(s)                  # Returns the number of characters in the string

s.lower()               # Converts all characters to lowercase

s.upper()               # Converts all characters to uppercase

s.strip()               # Removes leading and trailing whitespace

s.split()               # Splits the string into a list (default is whitespace)

" ".join(words)         # Joins a list of strings into one string separated by spaces

s.replace("a", "b")     # Replaces all occurrences of a with b

s.find("abc")           # Returns the starting index of abc, or -1 if not found

s.startswith("he")      # Returns True if the string starts with he

s.endswith("lo")        # Returns True if the string ends with lo

```

<u> Time Complexity:</u>

| Operation               | Time                                 |
| ----------------------- | ------------------------------------ |
| Access `s[i]`           | O(1)                                 |
| Length `len(s)`         | O(1)                                 |
| Slice `s[a:b]`          | O(k), where *k* is the slice length  |
| Concatenation `s1 + s2` | O(n + m)                             |
| `s.lower()`             | O(n)                                 |
| `s.upper()`             | O(n)                                 |
| `s.strip()`             | O(n)                                 |
| `s.split()`             | O(n)                                 |
| `"".join(words)`        | O(total characters)                  |

### Two Pointers 
Two pointer is a technique used to help us solve array and string problems where we use two integer variables to iterate along something. 
- They can move toward each other (opposite directions) or in the same direction (fast/slow pointers or sliding window)
- Note: Opposite direction two pointers usually require the array to be sorted
- Time complexity: O(n)
- Space complexity: O(1)


<u>Some common uses: </u>
- Finding pairs with a target sum
- Removing duplicates
- Merging sorted arrays
- Reversing an array or string
- Checking if a string is a palindrome 
- Partitioning arrays

<u>Fast & Slow Pointers </u>
- Two pointers move in the same direction
- Slow moves one step
- Fast moves two (or more) steps
- Common Uses: Detect cycle in linked list, find middle of linked list, find duplicate number


<u> Generic Template for Two Pointers (Opposite Directions): </u>

```
def fn(arr):
    left = 0
    right = len(arr) - 1

    while left < right:
        # Do some logic with arr[left] and arr[right]

        if condition1:
            left += 1
        elif condition2:
            right -= 1
        else:
            left += 1
            right -= 1
```



<u> Example: Find two numbers that sum to 6</u>

```
arr = [4, 1, 5, 2, 3]
       L           R
target = 6
def two_sum(arr, target):
    arr.sort()  # [1, 2, 3, 4, 5]

    left = 0
    right = len(arr) - 1

    while left < right:
        current_sum = arr[left] + arr[right]

        if current_sum == target:
            return left, right

        elif current_sum < target:
            left += 1

        else:
            right -= 1

    return -1

print(two_sum(arr, target))

``` 



### Sliding Window
Sliding Window is a specialized form of the Two Pointers technique used to solve problems involving contiguous subarrays or substrings efficiently. Instead of checking every possible subarray, we use a window and move it through the array or string while updating it based on a set condition.

- A window is defined by two pointers, usually named left and right
- The window can expand by moving the right pointer or shrink by moving the left pointer
- Time complexity: O(n)
- Space complexity: O(1) or O(k) if using a hash map/set

 <u>Some common uses:</u>
- Finding the maximum or minimum sum of a subarray
- Longest substring with unique characters
- Smallest subarray with a target sum
- Longest substring with at most K distinct characters
- Maximum number of consecutive ones
- Finding anagrams in a string

<u>Types of Sliding Window:</u>

1. Fixed Size Window
- The window size never changes
- Move both pointers together while updating the current window

Example: Find the maximum sum of any subarray of size k

```
Array = [2, 1, 5, 1, 3, 2]
k = 3

Window 1: [2, 1, 5] → Sum = 8
Window 2: [1, 5, 1] → Sum = 7
Window 3: [5, 1, 3] → Sum = 9
Window 4: [1, 3, 2] → Sum = 6

Maximum Sum = 9

def max_sum_subarray(arr, k):
    left = 0
    window_sum = 0
    max_sum = float('-inf')

    for right in range(len(arr)):
        window_sum += arr[right]

        # Window has reached size k
        if right - left + 1 == k:
            max_sum = max(max_sum, window_sum)

            # Slide the window
            window_sum -= arr[left]
            left += 1

    return max_sum


```

<u>Generic Template (Fixed Size Window):</u>

```
def fn(arr, k):
    left = 0
    window_sum = 0

    for right in range(len(arr)):
        window_sum += arr[right]

        # Window has reached size k
        if right - left + 1 == k:
            # Do something with the current window
            print(window_sum)

            # Remove left element and slide window
            window_sum -= arr[left]
            left += 1

```

2. Variable Size Window
- The window expands until a condition is met
- Then it shrinks until the condition becomes valid again

Example: Find the smallest subarray whose sum is at least target

```
array = [2, 3, 1, 2, 4, 3]
target = 7

def min_subarray_len(target, nums):
    left = 0
    current_sum = 0
    min_length = float('inf')

    for right in range(len(nums)):
        # Expand the window
        current_sum += nums[right]

        # Shrink the window while condition is satisfied
        while current_sum >= target:
            min_length = min(min_length, right - left + 1)
            current_sum -= nums[left]
            left += 1

    return 0 if min_length == float('inf') else min_length

print(min_subarray_len(target, nums))


```

<u>Generic Template (Variable Size Window):</u>

```
def fn(arr):
    left = 0

    for right in range(len(arr)):
        # Expand the window
        # Update any data structures

        while window_is_invalid:
            # Shrink the window
            left += 1

        # Update answer using current valid window

```



