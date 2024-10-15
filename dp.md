# DP

Dynamic programming (DP) is a method used in computer science to solve problems by breaking them down into simpler subproblems and solving each subproblem just once, storing its solution in a memory structure (like an array or a map). This way, when the same subproblem occurs again, the program can simply look up the stored solution, avoiding the need to compute the answer again. This technique significantly reduces the computation time, especially for problems where the same subproblems are solved multiple times.

## Introduction to Dynamic Programming

The core idea behind dynamic programming is to avoid repeated calculations by storing the results of expensive function calls and reusing them when the same inputs occur again. This approach is particularly useful for problems that can be divided into overlapping subproblems, which are smaller instances of the same problem.

We would take Fibonacci series as our running example. 

$f(n) = f(n-1) + f(n-2)$  where  
$n \ge 2$  and  $f(0)=0$ and $f(1)=1$  

Naive way to get the answer of $f(n)$ is :
```c++
int f(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    return f(n - 1) + f(n - 2);
}
```

### Memoization (Top-Down Approach)

Memoization is a common strategy used in dynamic programming where you start solving the problem from the top (the original problem) and work your way down to the base cases by solving subproblems recursively. If a subproblem has been solved, its result is stored in a lookup table, so it doesn't have to be recalculated. This method transforms a recursive approach into a more efficient one by memorizing the results of previous computations.

**Example: Fibonacci Sequence**

The Fibonacci sequence is a classic example where dynamic programming can be applied. The naive recursive solution calculates the nth Fibonacci number by summing the two preceding numbers, which leads to an exponential time complexity due to the repeated calculation of the same subproblems. By using memoization, each unique subproblem is solved only once, reducing the time complexity from exponential to linear.

```c++
const int MAXN = 100;
bool found[MAXN];
int memo[MAXN];

int f(int n) {
    if (found[n]) return memo[n];
    if (n == 0) return 0;
    if (n == 1) return 1;

    found[n] = true;
    return memo[n] = f(n - 1) + f(n - 2);
}
```

#### Bottom-Up Approach

The bottom-up approach is the opposite of memoization. Instead of starting from the top and working your way down, you start from the base cases and solve the problem by building up the solution to the original problem. This approach iteratively solves all related subproblems and stores their results.

### Example: Fibonacci Sequence (Bottom-Up)

For the Fibonacci sequence, the bottom-up approach initializes the first two numbers of the sequence and then iterates to calculate subsequent numbers up to the nth number. This method only requires constant space if only the last two computed values are kept at any time.
```c++
const int MAXN = 100;
int fib[MAXN];

int f(int n) {
    fib[0] = 0;
    fib[1] = 1;
    for (int i = 2; i <= n; i++) fib[i] = fib[i - 1] + fib[i - 2];

    return fib[n];
}
```

### Key Points

- **Avoid Repeated Work:** The essence of dynamic programming is to avoid computing the same subproblem multiple times.
- **Memoization:** Store the results of expensive function calls and reuse them.
- **Bottom-Up Approach:** Start from the simplest subproblems and iteratively build up the solution to the larger problem.
- **Application:** Dynamic programming is widely used in algorithms that require optimization, such as finding the shortest path in a graph, or solving knapsack problems, where it significantly reduces computation time by avoiding redundant calculations.


## Knapsack DP 

The knapsack problem is the following problem in combinatorial optimization:

**Given a set of items, each with a weight and a value, determine which items to include in the collection so that the total weight is less than or equal to a given limit and the total value is as large as possible.**

Given a set of n items numbered from 1 to n, each with a weight $w_i$ and a value $v_i$, along with a maximum weight capacity $W$.

**maximize** 
$
    \sum_{i=1}^{n} v_i x_i
$

**subject to** 
$
    \sum_{i=1}^{n} w_i x_i \leq W 
$
and 
$
x_i \in \{0, 1\}.
$

In this section of the report, we will  delve into the solution of the 0-1 Knapsack Problem using dynamic programming, a method renowned for its efficiency in solving optimization problems that exhibit overlapping subproblems and optimal substructure characteristics. The 0-1 Knapsack Problem is a classic example of combinatorial optimization, where the objective is to maximize the total value of items that can be placed into a knapsack, given a weight limit, with each item having a specific weight and value. The "0-1" designation indicates that each item can either be included in its entirety or excluded; partial inclusion is not permitted.

### Dynamic Programming Solution

Dynamic programming solves problems by breaking them down into simpler, manageable subproblems, solving each once, and storing their solutions. For the 0-1 Knapsack Problem, the dynamic programming approach involves creating a table where each cell represents the maximum value that can be achieved with a subset of items up to a certain weight limit.

#### Example

Consider a scenario where a knapsack can carry up to 10 weight units, and there are 4 items to choose from, with the following weights and values:

- Item 1: Weight = 5, Value = 10
- Item 2: Weight = 4, Value = 40
- Item 3: Weight = 6, Value = 30
- Item 4: Weight = 3, Value = 50

To solve this using dynamic programming, we construct a table `dp` where `dp[i][w]` represents the maximum value achievable with the first `i` items and a knapsack capacity of `w`. The dimensions of the table are `(n+1) x (W+1)`, where `n` is the number of items and `W` is the knapsack capacity, to accommodate for the case where no items are chosen or the knapsack capacity is 0.

#### Code Snippet

```c++
int knapSack(int W, int wt[], int val[], int n) {
    int i, w;
    int dp[][] = new int[n+1][W+1];

    for (i = 0; i <= n; i++) {
        for (w = 0; w <= W; w++) {
            if (i==0 || w==0)
                dp[i][w] = 0;
            else if (wt[i-1] <= w)
                dp[i][w] = Math.max(val[i-1] + dp[i-1][w-wt[i-1]],  dp[i-1][w]);
            else
                dp[i][w] = dp[i-1][w];
        }
    }

    return dp[n][W];
}
```

This function `knapSack` calculates the maximum value that can be achieved within the knapsack's weight limit `W`, given the weights `wt[]` and values `val[]` of the items and the total number of items `n`. The function iteratively fills the `dp` table, comparing the value of including or excluding each item to find the optimal solution.

#### Time and Space Complexity

- **Time Complexity**: The time complexity of this dynamic programming solution is `O(nW)`, where `n` is the number of items and `W` is the maximum weight the knapsack can carry. This is because the solution iterates over all items for each possible weight limit up to `W`.
  
- **Space Complexity**: The space complexity is also `O(nW)` due to the storage requirements of the `dp` table. Each cell in the table represents a state defined by a specific combination of `n` items and `W` weight limit.

### Conclusion

The dynamic programming approach to the 0-1 Knapsack Problem efficiently solves it by utilizing a table to keep track of the maximum values that can be achieved with different combinations of items and weight limits. This method ensures that each subproblem is solved only once, significantly reducing the computational effort compared to naive recursive solutions. The example and code provided illustrate the process of populating the `dp` table and highlight the practical application of dynamic programming in solving complex optimization problems.

**Quick Question  :-Think about how can we optimize it's space complexity to $O(W).$**

### Answer :- 
$dp[i][weight_1]$ only depends on $dp[i-1][weight_2]$ so, we only need to maintain answer to the previous iteration. We can find $dp[current = 1][weight_1]$ from $dp[previous=0][weight_2]$.


## Grid DP

A common archetype of DP Problems involves a 2D grid of square cells (like graph
paper), and we have to analyze "paths." A path is a sequence of cells whose
movement is restricted to one direction on the $x$-axis and one direction on the
$y$-axis (for example, you may only be able to move down or to the right).
Usually, the path also has to start in one corner of the grid and end on another
corner. The problem may ask you to count the number of paths that satisfy some
property, or it may ask you to find the max/min of some quantity over all paths.

Usually, the sub-problems in this type of DP are a sub-rectangle of the whole
grid. For example, consider a problem in which we count the number of paths from
$(1, 1)$ to $(N, M)$ when we can only move in the positive $x$-direction and the
positive $y$-direction.

Let $\texttt{dp}[x][y]$ be the number of paths in the sub-rectangle whose
corners are $(1, 1)$ and $(x, y)$. We know that the first cell in a path counted
by $\texttt{dp}[x][y]$ is $(1, 1)$, and we know the last cell is $(x, y)$.
However, the second-to-last cell can either be $(x-1, y)$ or $(x, y-1)$. Thus,
if we pretend to append the cell $(x, y)$ to the paths that end on $(x-1, y)$ or
$(x, y-1)$, we construct paths that end on $(x, y)$. Working backwards like that
motivates the following recurrence:
$\texttt{dp}[x][y] = \texttt{dp}[x-1][y] + \texttt{dp}[x][y-1]$. We can use this
recurrence to calculate $\texttt{dp}[N][M]$. Keep in mind that
$\texttt{dp}[1][1] = 1$ because the path to $(1, 1)$ is just a single cell. In
general, thinking about how you can append cells to paths will help you
construct the correct DP recurrence.

When using the DP recurrence, it's important that you compute the DP values in
an order such that the dp-value for a cell is known before you use it to compute
the dp-value for another cell. In the example problem above, it's fine to
iterate through each row from $0$ to $M-1$:

### Code :- 
```c++
for (int i = 0; i < M; i++) {
	for (int j = 0; j < N; j++) {
		if (j > 0) dp[j][i] += dp[j - 1][i];
		if (i > 0) dp[j][i] += dp[j][i - 1];
	}
}
```

## Range DP (LR DP)
Dynamic programming on ranges is a general technique used to solve problems of
the form "what is the minimum/maximum metric one can achieve on an array $A$?"
with the following properties:

- A greedy approach seems feasible but yields incorrect answers.
- Given the answers for each subarray $A[l : x]$ and $A[y : r]$, we can
calculate the answer for the subarray $A[l : r]$ in $\mathcal O(r - l)$ time.
- Disjoint subarrays can be "combined" independently.
- $N$ (the size of $A$) is not greater than $500$.


This technique relies on the assumption that we can "combine" two subarrays
$A[l : x]$ and $A[x + 1 : r]$ to get a candidate for $A[l : r]$. We can thus
iterate over all $x$ and find the best possible answer for $A[l : r]$. (Note
that we need to process subarrays in increasing order of length!)

Since there are $\mathcal O(N^2)$ subarrays and processing each one takes
$\mathcal O(N)$ time, solutions using this technique generally run in
$\mathcal O(N^3)$ time.

## Digit DP 

Digit DP is a technique used to solve problems that asks you to find the number
of integers within a range that satisfies some property based on the digits of
the integers. Typically, the ranges are between large integers (such as between
$1$
 to 
$10^{18}$
), so looping through each integer and checking if it satisfies
the given property is too slow. Digit DP uses the digits of the integers to
quickly count the number of integers with the given property in the range of
integers.


Digit DP is a dynamic programming technique that deals with problems involving digits of numbers, particularly useful for solving problems related to numbers in a given range. The technique is based on the principle of breaking down a problem into smaller subproblems and solving each subproblem just once, storing its result in a memory table (memoization) to avoid recalculating the answer every time the subproblem is encountered.

### Concept of Digit DP

The core idea behind Digit DP is to use the digits of the numbers to formulate the DP states and transitions. It often involves counting or optimizing certain properties of numbers within a given range without having to iterate through all numbers in the range explicitly.

### Example Problem

Consider a problem where you need to find how many numbers `x` are there in the range `a` to `b`, where the digit `d` occurs exactly `k` times in `x`. This problem can be approached using Digit DP by focusing on the digits of the numbers from `a` to `b` and using dynamic programming to avoid redundant calculations.

### Solving Approach

1. **Define States**: The states in Digit DP usually include the current position (digit), the tight constraint (whether we are following the number exactly up to this digit), and any additional constraints specific to the problem (e.g., how many times a certain digit has appeared).

2. **Transitions**: The transitions depend on the choice of the next digit at each position, considering the constraints defined by the problem (e.g., the digit should not exceed the corresponding digit in the upper limit if we are under the tight constraint).

3. **Base Case**: The base case is usually when we have considered all digits, and we check if the number formed satisfies all conditions (e.g., the digit `d` appears exactly `k` times).

4. **Memoization**: Store the results of subproblems to avoid recalculating them.

### Implementation

The implementation involves a recursive function with memoization. The function parameters represent the states, and the function returns the count of numbers satisfying the problem's constraints up to the current digit. Pseudocode for a generic Digit DP solution might look like this:

```plaintext
function digitDP(pos, tight, additionalConstraints):
    if pos is the last position:
        return 1 if all conditions are satisfied else 0
    if result is already computed for the current state:
        return result from memoization table
    result = 0
    limit = 9 if not tight else digit at pos in upper limit
    for digit from 0 to limit:
        newTight = tight and (digit == limit)
        result += digitDP(pos + 1, newTight, updateConstraints(additionalConstraints, digit))
    memoize current state and result
    return result
```

### Time and Space Complexity

- **Time Complexity**: The time complexity depends on the number of states and the transitions. For a problem with `D` digits, if there are `T` possible states per digit and we consider all `10` possible digits for transitions, the time complexity would be `O(D * T * 10)`.
- **Space Complexity**: The space complexity is mainly due to the memoization table, which would be `O(D * T)`.


## Longest increasing subsequence
We are given an array with  
$n$  numbers:  
$a[0 \dots n-1]$ . The task is to find the longest, strictly increasing, subsequence in  
$a$ .

Formally we look for the longest sequence of indices  
$i_1, \dots i_k$  such that

 
$$i_1 < i_2 < \dots < i_k,\quad a[i_1] < a[i_2] < \dots < a[i_k]$$ 

### Solution in  $O(n^2)$  with dynamic programming

Dynamic programming is a very general technique that allows to solve a huge class of problems. Here we apply the technique for our specific task.

First we will search only for the length of the longest increasing subsequence, and only later learn how to restore the subsequence itself.

**Finding the length**

To accomplish this task, we define an array  
$d[0 \dots n-1]$ , where  
$d[i]$  is the length of the longest increasing subsequence that ends in the element at index  
$i$ .

We will compute this array gradually: first  
$d[0]$ , then  
$d[1]$ , and so on. After this array is computed, the answer to the problem will be the maximum value in the array  
$d[]$ .

So let the current index be  
$i$ . I.e. we want to compute the value  
$d[i]$  and all previous values  
$d[0], \dots, d[i-1]$  are already known. Then there are two options:

 
$d[i] = 1$ : the required subsequence consists only of the element  
$a[i]$ .

 
$d[i] > 1$ : The subsequence will end at  
$a[i]$ , and right before it will be some number $a[j]$  with $j < i$  and $a[j] < a[i]$ .

It's easy to see, that the subsequence ending in  
$a[j]$  will itself be one of the longest increasing subsequences that ends in  $a[j]$ . The number $a[i]$  just extends that longest increasing subsequence by one number.

Therefore, we can just iterate over all  
$j < i$  with  
$a[j] < a[i]$ , and take the longest sequence that we get by appending  
$a[i]$  to the longest increasing subsequence ending in  
$a[j]$ . The longest increasing subsequence ending in  
$a[j]$  has length  
$d[j]$ , extending it by one gives the length  
$d[j] + 1$ .

  
 
 
 
 
$$d[i] = \max_{\substack{j < i \\\\ a[j] < a[i]}} \left(d[j] + 1\right)$$ 
If we combine these two cases we get the final answer for  
$d[i]$ :

 
 
 
 
 
$$d[i] = \max\left(1, \max_{\substack{j < i \\\\ a[j] < a[i]}} \left(d[j] + 1\right)\right)$$ 

### Code for naive approach $O(n^2)$
```c++
int lis(vector<int> const& a) {
    int n = a.size();
    vector<int> d(n, 1);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (a[j] < a[i])
                d[i] = max(d[i], d[j] + 1);
        }
    }

    int ans = d[0];
    for (int i = 1; i < n; i++) {
        ans = max(ans, d[i]);
    }
    return ans;
}
```
We can use backtracking to resotre the actual sequence. 

## Solution in  $O(n \log n)$  with dynamic programming and binary search
In order to obtain a faster solution for the problem, we construct a different dynamic programming solution that runs in  
$O(n^2)$ , and then later improve it to  
$O(n \log n)$ .

We will use the dynamic programming array  
$d[0 \dots n]$ . This time 
$d[l]$  doesn't corresponds to the element 
$a[i]$  or to an prefix of the array. 
$d[l]$  will be the smallest element at which an increasing subsequence of length  
$l$  ends.

Initially we assume  
$d[0] = -\infty$  and for all other lengths  
$d[l] = \infty$ .

We will again gradually process the numbers, first  
$a[0]$ , then  
$a[1]$ , etc, and in each step maintain the array  
$d[]$  so that it is up to date.

Given the array  
$a = \{8, 3, 4, 6, 5, 2, 0, 7, 9, 1\}$ , here are all their prefixes and their dynamic programming array. Notice, that the values of the array don't always change at the end.

  
For example :- 

Given the array  
$a = \{8, 3, 4, 6, 5, 2, 0, 7, 9, 1\}$ , here are all their prefixes and their dynamic programming array. Notice, that the values of the array don't always change at the end.


 
$$ \begin{array}{ll} \text{prefix} = \{\} &\quad d = \{-\infty, \infty, \dots\}\\ \text{prefix} = \{8\} &\quad d = \{-\infty, 8, \infty, \dots\}\\ \text{prefix} = \{8, 3\} &\quad d = \{-\infty, 3, \infty, \dots\}\\ \text{prefix} = \{8, 3, 4\} &\quad d = \{-\infty, 3, 4, \infty, \dots\}\\ \text{prefix} = \{8, 3, 4, 6\} &\quad d = \{-\infty, 3, 4, 6, \infty, \dots\}\\ \text{prefix} = \{8, 3, 4, 6, 5\} &\quad d = \{-\infty, 3, 4, 5, \infty, \dots\}\\ \text{prefix} = \{8, 3, 4, 6, 5, 2\} &\quad d = \{-\infty, 2, 4, 5, \infty, \dots \}\\ \text{prefix} = \{8, 3, 4, 6, 5, 2, 0\} &\quad d = \{-\infty, 0, 4, 5, \infty, \dots \}\\ \text{prefix} = \{8, 3, 4, 6, 5, 2, 0, 7\} &\quad d = \{-\infty, 0, 4, 5, 7, \infty, \dots \}\\ \text{prefix} = \{8, 3, 4, 6, 5, 2, 0, 7, 9\} &\quad d = \{-\infty, 0, 4, 5, 7, 9, \infty, \dots \}\\ \text{prefix} = \{8, 3, 4, 6, 5, 2, 0, 7, 9, 1\} &\quad d = \{-\infty, 0, 1, 5, 7, 9, \infty, \dots \}\\ \end{array} $$ 
<br>

When we process  
$a[i]$ , we can ask ourselves. What have the conditions to be, that we write the current number  
$a[i]$  into the  
$d[0 \dots n]$  array?

We set  
$d[l] = a[i]$ , if there is a longest increasing sequence of length  
$l$  that ends in  
$a[i]$ , and there is no longest increasing sequence of length  
$l$  that ends in a smaller number. Similar to the previous approach, if we remove the number  
$a[i]$  from the longest increasing sequence of length  
$l$ , we get another longest increasing sequence of length  
$l -1$ . So we want to extend a longest increasing sequence of length  
$l - 1$  by the number  
$a[i]$ , and obviously the longest increasing sequence of length  
$l - 1$  that ends with the smallest element will work the best, in other words the sequence of length  
$l-1$  that ends in element  
$d[l-1]$ .

There is a longest increasing sequence of length  
$l - 1$  that we can extend with the number  
$a[i]$ , exactly if  
$d[l-1] < a[i]$ . So we can just iterate over each length  
$l$ , and check if we can extend a longest increasing sequence of length  
$l - 1$  by checking the criteria.

Additionally we also need to check, if we maybe have already found a longest increasing sequence of length  
$l$  with a smaller number at the end. So we only update if  
$a[i] < d[l]$ .

After processing all the elements of  
$a[]$  the length of the desired subsequence is the largest  
$l$  with  
$d[l] < \infty$ .

### Code 
```c++
int lis(vector<int> const& a) {
    int n = a.size();
    const int INF = 1e9;
    vector<int> d(n+1, INF);
    d[0] = -INF;

    for (int i = 0; i < n; i++) {
        int l = upper_bound(d.begin(), d.end(), a[i]) - d.begin();
        if (d[l-1] < a[i] && a[i] < d[l])
            d[l] = a[i];
    }

    int ans = 0;
    for (int l = 0; l <= n; l++) {
        if (d[l] < INF)
            ans = l;
    }
    return ans;
}
```