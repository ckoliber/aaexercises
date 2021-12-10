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

```typescript
const k01Knapsack = (
    weights: number[],
    profits: number[],
  	maxWeight: number,
   	minProfit: number
): boolean => {
    const n = array.length;
    let objects = new Set();

    // Generate each combination of objects O(n) [Parallel]
    for (let i = 0 ; i < n ; i++) {
		if (fork({0,1}) == 1) {
            objects.add(i);
        }
    }

    // Validate selected objects weight sum is not greater than maxWeight O(n) [Sequential]
    let weightSum = 0;
    for (let i = 0 ; i < n ; i++) {
        if (objects.contains(i)) {
            weightSum += weights[i];
        }
    }
    if (weightSum > maxWeight) {
        exit(-1);
    }

   	// Validate selected objects profit sum is not less than minProfit O(n) [Sequential]
    let profitSum = 0;
    for (let i = 0 ; i < n ; i++) {
        if (objects.contains(i)) {
            profitSum += profits[i];
        }
    }
    if (profitSum < minProfit) {
        exit(-1);
    }

    return true;
};
```

---

## Problem 4

```typescript
const kVertexCover = (graph: number[][], k: number): boolean => {
    const n = array.length;
    let vertices = new Set();

    // Generate each combination of graph vertices O(n) [Parallel]
    for (let i = 0 ; i < n ; i++) {
        if (fork({0,1}) == 1) {
            vertices.add(i);
        }
    }

    // Validate selected set size is less than or equals to K O(1) [Sequential]
    if (vertices.size() > k) {
        exit(-1);
    }

    // Validate for each edge, at least one side vertex exists in selected set O(n^2) [Sequential]
    for (let i = 0 ; i < n ; i++) {
        for (let j = 0 ; j < n ; j++) {
            if (graph[i][j] != null && !vertices.contains(i) && !vertices.contains(j)) {
                exit(-1);
            }
        }
    }

    return true;
};
```

---

## Problem 5

### (a)

$$
\begin{aligned}
    & \Phi =
    (\overline{x_2} \,\or\, \overline{x_3} \,\or\, \overline{x_4}) \and
    (\overline{x_1} \,\or\, {x_2} \,\or\, \overline{x_4}) \and
    (\overline{x_2} \,\or\, {x_3} \,\or\, \overline{x_5}) \and
    ({x_1} \,\or\, \overline{x_3} \,\or\, \overline{x_5})
    \\
    \\
    & \begin{cases}
    x_1 = \text{False}
    \\
    x_2 = \text{False}
    \\
    x_3 = \text{False}
    \\
    x_4 = \text{False}
    \\
    x_5 = \text{False}
    \end{cases}
    \\
    \\
    \implies & \Phi =
    (\text{T} \,\or\, \text{T} \,\or\, \text{T}) \and
    (\text{T} \,\or\, \text{F} \,\or\, \text{T}) \and
    (\text{T} \,\or\, \text{F} \,\or\, \text{T}) \and
    (\text{F} \,\or\, \text{T} \,\or\, \text{T})
    \\
    \implies & \Phi = (\text{T}) \and (\text{T}) \and (\text{T}) \and (\text{T})
    \\
    \implies & \Phi = \text{T}
\end{aligned}
$$

---

### (b)

$$
\begin{aligned}
    & \Phi =
    (\overline{x_2} \,\or\, \overline{x_3} \,\or\, \overline{x_4}) \and
    (\overline{x_1} \,\or\, {x_2} \,\or\, \overline{x_4}) \and
    (\overline{x_2} \,\or\, {x_3} \,\or\, \overline{x_5}) \and
    ({x_1} \,\or\, \overline{x_3} \,\or\, \overline{x_5})
    \\
    \\
    & \begin{cases}
    x_1 = \text{True}
    \\
    x_2 = \text{True}
    \\
    x_3 = \text{True}
    \\
    x_4 = \text{True}
    \\
    x_5 = \text{True}
    \end{cases}
    \\
    \\
    \implies & \Phi =
    (\text{F} \,\or\, \text{F} \,\or\, \text{F}) \and
    (\text{F} \,\or\, \text{T} \,\or\, \text{F}) \and
    (\text{F} \,\or\, \text{T} \,\or\, \text{F}) \and
    (\text{T} \,\or\, \text{F} \,\or\, \text{F})
    \\
    \implies & \Phi = (\text{F}) \and (\text{T}) \and (\text{T}) \and (\text{T})
    \\
    \implies & \Phi = \text{F}
\end{aligned}
$$

---

### (c)

-   **K-Clique**: This picture is not complete !

![test.drawio (1)](C:\Users\KoLiBer\Downloads\test.drawio (1).png)

-   **K-VertexCover**:

![test.drawio (2)](C:\Users\KoLiBer\Downloads\test.drawio (4).png)

-   **Subset Sum**:

![Book1](C:\Users\KoLiBer\Downloads\Book1.png)

---

## Problem 6

---

## Problem 7

---
