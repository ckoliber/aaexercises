# AAD Answers

-   **Answer Set**: No. 02
-   **Full Name**: Mohammad Hosein Nemati
-   **Student Code**: `610300185`

---

## Problem 1

We are going to implement a **Dynamic Programming Algorithm**, that will solve the `SSSP` problem in **Pseudo-Polynomial Time-Complexity**:

```cpp
bool SSSP (int[] array, int expectedSum) {
    // cache[i][j] is true, whenever exists
    // a subset of array[0..j-1]
    // with sum equals to i
    bool cache[array.size() + 1][expectedSum + 1];

    // Time-Complexity: O(n)
    // If expectedSum is 0, result is true
    for (int i = 0; i <= array.size(); i++) {
        cache[i][0] = true;
    }

    // Time-Complexity: O(m)
    // If expectedSum is not 0 and array is empty answer is false
    for (int i = 1; i <= expectedSum; i++) {
        cache[0][i] = false;
    }

    // Time-Complexity: O(n*m)
    // Fill the cache matrix (bottom-up approach)
    // Iterate over subset size of array from 0 to array.size()
    for (int i = 1; i <= array.size(); i++) {
        // Iterate over expected sum from 0 to expectedSum
        for (int j = 1; j <= expectedSum; j++) {

            // If the current array item is larger than expectedSum, drop it
            if (array[i - 1] > j) {
                cache[i][j] = cache[i - 1][j];
            }

            // If the current array item is smaller than expectedSum, accept/drop it
            if (array[i - 1] <= j) {
                cache[i][j] = (
                    cache[i - 1][j - cache[i - 1]] ||
                    cache[i - 1][j]
                );
            }

        }
    }

    return cache[array.size()][expectedSum];
};
```

The time complexity analysis of the algorithm above is as follows:

$$
\begin{aligned}
	& n: \text{Size of array}
	\\
	& m: \text{Expected sum}
	\\ \\
	& \texttt{Set first column to true}: O(n)
	\\
	& \texttt{Set first row to false}: O(m)
	\\
	& \texttt{Fill the cache table}: O(n \times m)
	\\
	\\
	& Complexity = O(n) + O(m) + O(n \times m) = O(n \times m)
\end{aligned}
$$

Now, we can see, if the **Expected Sum** is a **Polynomial** function of **Array Size**, then the algorithm will solve problem in polynomial time.

---

## Problem 2

This problem, is a special case of the last problem, whenever we set the **Expected Sum** to the **Half Of Elements Sum**, in this case the other half sum is also equals to the **Half Of Elements Sum**:

```cpp
bool Partition (int[] array) {
    // Time-Complexity: O(n)
    // Find the entire elements sum
    int sum = 0;
    for (int i = 0; i < array.size(); i++) {
        sum += array[i];
    }

    // Time-Complexity: O(n*m)
    return SSSP(array, sum/2);
};
```

The time complexity analysis of the algorithm above is as follows:

$$
\begin{aligned}
	& n: \text{Size of array}
	\\
	& m: \text{Elements sum}
	\\ \\
	& \texttt{Find elements sum}: O(n)
	\\
	& \texttt{Solve using SSSP}: O(n \times m)
	\\
	\\
	& Complexity = O(n) + O(n \times m) = O(n \times m)
\end{aligned}
$$

Now, we can see, if the **Elements Sum** is a **Polynomial** function of **Array Size**, then the algorithm will solve problem in polynomial time.

---

## Problem 3

We are going to implement a **Back-Tracking Algorithm**, that will solve the `MAX-SAT` problem:

```typescript
const MaxSat = (
    formula: number[][],
    variablesCount: number,
    variablesValues: number[]
): number => {
    // Time-Complexity: O(n)
    // If all variables have an assignment
    // find the max number of satisfiable clauses
    if (variablesCount < 0) {
        return satisfiableClauses(formula, variablesValues);
    }

    // Time-Complexity: O(2^m)
    // Select the maximum number of satisfiable clauses
    // When last variable is true or false
    return Math.max(
        MaxSat(formula, variablesCount - 1, [true, ...variablesValues]),
        MaxSat(formula, variablesCount - 1, [false, ...variablesValues])
    );
};

// Time-Complexity: O(n)
// Find the number of satisfiable clauses by an assignment of variables
const satisfiableClauses = (formula: number[][], variablesValues: number[]) => {
    let satisfiableClauses = 0;

    // Iterate over each clause and check is satisfiable
    for (const clause in formula) {
        if (isSatisfiableClause(clause, variablesValues)) {
            satisfiableClauses++;
        }
    }

    return satisfiableClauses;
};

// Check the clause is satisfiable or not
const isSatisfiableClause = (clause: number[], variablesValues: number[]) => {
    // Iterate over literals and find at least one true valued literal
    for (const literal in clause) {
        if (
            (variablesValues[Math.abs(literal)] && literal > 0) ||
            (!variablesValues[Math.abs(literal)] && literal < 0)
        ) {
            return true;
        }
    }

    return false;
};
```

The time complexity analysis of the algorithm above is as follows:

$$
\begin{aligned}
	& n: \text{Number of literals in entire formula}
	\\
	& m: \text{Number of variables in entire formula}
	\\ \\
	& \texttt{Find number of satisfiable clauses}: O(n)
	\\
	& \texttt{Generate each permutation of variables assignment}: O(2^m)
	\\
	\\
	& Complexity = O(n) \times O(2^m) = O(n \times 2^m)
\end{aligned}
$$

Now, we can see, the complexity of algorithm is depends on the number of variables, so the **MAX-SAT** problem is `fixed-parameter tractable` according to **Number of variables**.

---

## Problem 4

---

## Problem 5

We are going to implement a **Back-Tracking Algorithm**, then we will do some optimizations and introduce a new **Branch-and-Bound Algorithm** for it:

```typescript
let minCost = Math.MAX;
const MinCostSetCover = (
    sets: number[][],
    costs: number[],
    universe: number[],
    selectedSets: number[] = [],
    selectedCost: number = 0
) => {
    const currentLevel = selectedSets.length;

    // Validate the answer (Back-Track finish)
    if (selectedSets.length == sets.length) {
        // Check the selected sets contains universe elements
        // Then check selectedCost is less than min cost
        if (selectedSets.contains(universe) && selectedCost < minCost) {
            minCost = selectedCost;
            return;
        }
    }

    // Branch-and-Bound optimization
    // The current cost is greater than minCost
    // We already have another good solution
    // Just drop the branch
    if (selectedCost > minCost) {
        return;
    }

    // Branch-and-Bound optimization
    // The current selection, contains universe
    // We don't need to peek any other sets
    if (!selectedSets.contains(universe)) {
        MinCostSetCover(
            sets,
            costs,
            universe,
            [...selectedSets, true],
            selectedCost + costs[currentLevel]
        );
    }

    MinCostSetCover(
        sets,
        costs,
        universe,
        [...selectedSets, false],
        selectedCost
    );
};
```

As we can see, there are two types of optimizations over the **Back-Tracking Algorithm**:

1. **Selected Cost Validate**: Check the current cost is not greater than `minCost`
2. **Selected Sets Validate**: Check the current sets does not contains the `universe`

---

## Problem 6

As we know, the complexity of a **Back-Tracking** algorithm (**Branch-and-Bound** without any optimization) is about $O(Exp(n) \times P(n))$ where the $Exp(n)$ (`Exponential`) is the complexity of generating all possible solutions, and the $P(n)$ (`Polynomial`) is the complexity of validating a specific solution.

Using some **Branch-and-Bound** optimizations we can reduce the complexity of generating solutions, or in other word, we can filter some invalid solutions and drop the entire branch.

If the number of **Semi-Critical** nodes in a **B&B Tree** is $O(P(n))$, then the number of **Semi-Critical** and **Critical** nodes are $O(P(n))$

So the entire possible solutions to be validated is in $O(P(n))$, and the **Branch-and-Bound** complexity will be $O(P(n) \times P(n)) = O(P(n))$.

---

## Problem 7

---
