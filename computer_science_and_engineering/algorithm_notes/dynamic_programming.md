# Dynamic Programming Notes and Questions

## Prerequisites for DP to be applicable
* The problem has optimal sub-structures
  * i.e. The solution to a sub-problem is part of the solution to the original problem
* The problem has **overlapping** sub-problems

## Top-down DP
* Essentially, a recursive complete search with memoization

## Bottom-up DP
Steps:
1. Determine the required set of parameters that uniquely describe the problem (the state)
2. If there are N params required to represent the states, prepare an N dimensional DP table. Initialize base cases.
3. Follow the transitions (recurrence relations) to fill the DP table.

## DP decision table
| Top-Down                                  | Bottom-Up |
--------------------------------------------|-------------------------------
| Pros: <li>Natural transformation from the normal Complete Search recursion.</li> <li>Computes the sub-problems only when necessary</li> | Pros: <li>Faster if many sub-problems are revisited</li> <li>Can ave memory space with the 'space saving trick'</li> |
| Cons: <li>Slower if many sub-problems are revisited due to function call overhead</li> <li>Cannot use 'space saving trick' to reduce memory</li> | Cons: <li>The style may not be intuitive</li> <li>Need to visit all states and fill the values</li>

<br>
<br>

## Classical examples
### Max 1D range sum
- Question
  ```
  Given an integer array A containing n <= 20K non-zero integers, determine the maximum (1D) range sum (sum of a subarray) of A.
  ```

- Idea
  * State dp[i]: the largest range sum of any subarray that ends with at i.

### Max 2D range sum
- Question
  ```
  Given an n-by-n square matrix of integers where each integer ranges from [-127...127], find a sub-matrix of A with the maximum sum.
  ```
- Idea
  * Turn the original matrix into a **cumulative sum matrix**， where a(i, j) represents the sum of all items within submatrix (0, 0) to (i, j).
  * Apply the idea of 1-D range sum

### 0-1 knapsack (subset sum)
- Qustion
  ```
  Given n items, each with own value V_i and weight W_i, and a maximum knapsack size S (limits the total weight). Compute the maximum value of the items that we can carry.
  (0-1 because for each item, take it or leave it)
  ```
- Idea
  * State: (id, remaiming_weight)
    * the value of total items when choosing id given remaining_weight
    * Recurrence relationship:
      * val(id, remaining_w) = val(id+1, remaining_w) if w[id] > remaining_w
      * val(id, remaining_w) = max(val(id+1, remaining_w), val(id+1, remaining_w - w[id])) if w[id] <= remaining_w
    * Natural to use top down DP

### Coin change
- Question
  ```
  Given a target amount V cents and a list of denominations for n coin types. What is the minimum number of coints that we must use to represent V. (assume we have unlimited supply of coins of any type)
  [Noted that this problem may not have a solution depending on the possible values of coins]
  ```
- Idea
  * State: min_coin_count = f(value_needed)

- Variant
  ```
  Count the number of possible ways to get value V cents given the denominations.
  ```

- Idea
  * State: way_count = f(coin_type_idx, target_value)
  * Terminate conditions:
    ```
    f(type_idx, 0) = 1 // one way, use nothing
    f(type_idx, <0) = 0 // impossible
    f(n, val) = 0 // finished all possible values
    ```
  * Recurrence relationship:
    ```
    f(type_idx, val) = f(type_idx+1, val) /* not using type_idx at all */
                       + f(type, value-coins[type_idx])
    ```

### Longest palindrome
- Question
  ```
  Given a string of up to n = 10000 characters, determine the length of the longest palindrome that you can make from it by deleting zero or more characters.
  ```
- Idea
  * State: `len(l, r)` is the length of the longest palindrome from string `a[l..r]`.
  * Base cases:
    * l == r: len(l, r)=1
    * l+1==r: len(l,r)=2 if (a[l] == a[r]) else 1
  * Recurrences:
    * len(l, r) =
      * 2+len(l+1,r-1) if (a[l]==a[r])
      * max(len(l,r-1), len(l+1,r)) otherwise
- Variant
  ```
  Do not allow deletion. Just return the longest palindrome substring.
  ```
 
### String alignment (edit distance)
- Question
  ```
  Align two strings a and b with the maximum alignment score (or minimum number of edit operations)
  Operations and scores
  * a[i] and b[i] match: +2
  * a[i] -> b[i] (replace): -1
  * insert a space in a[i] (insert): -1
  * delete a letter at a[i] (delete): -1
  ```

- Idea
  * State: `V(i, j)` is the score of the optimal alignment between prefix a[1..i] and b[1..j]
  * Base cases: V(0, 0)=0, V(i,0) = i * -1, V(0, j) = j * -1
  * Recurrences:
    ```
    V(i, j) = max(option1, option2, option3)
    option1 = V(i-1, j-1) + score(a[i], b[i]) // handle same or replace
    option2 = V(i-1, j) + score(delete) // delete a_i
    option3 = V(i, j-1) + score(delete) // insert b_j
    ```
  * More natural to do bottom up DP

### Longest common subsequence
- Question
  ```
  Given two strings a and b, return the longest common subsequence between them.
  (subsequence must be continuous)
  ```

### Forming quiz teams (**DP with mask**)
- Question
  ```
  Let (x,y) be the coordinates of a student's house on a 2D plane. There are 2N students and we want to pair them into N groups.
  Let d_i be the distance between the houses of 2 students in group i.
  Form N groups such that cost = sum of d_i where i=1 to N is minimized.
  Output the minimum cost.
  ```