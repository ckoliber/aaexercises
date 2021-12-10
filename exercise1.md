# AAD Answers

-   **Answer Set**: No. 01
-   **Full Name**: Mohammad Hosein Nemati
-   **Student Code**: `610300185`

---

## Problem 1

$$
\begin{aligned}
	f = (x_1 \or y_1) \and (x_2 \or y_2) \and \dots \and (x_n \or y_n)
\end{aligned}
$$

We know that $(x \,\or\, y)$ and $(\neg x \implies y)$ and $(\neg y \implies x)$ are equal

Replace each term with it's implication equivalent formulas

$$
\begin{aligned}
	f = [(\neg x_1 \implies y_1), (\neg y_1 \implies x_1)] \and \dots \and [(\neg x_n \implies y_n), (\neg y_n \implies x_n)]
\end{aligned}
$$

Create a directed graph with $2n$ vertices for all $x_i$'s and $y_i$'s

For each term add a directed edge from $\neg x_i$ to $y_i$ and from $\neg y_1$ to $x_1$

Now $f$ is not satisfiable if both $\neg x_i$ and $x_i$ are in the same `Strongly Connected Component`, and we can solve an _SCC_ problem using **Kosaraju Algorithm** in $O(n.\log{n} + e)$

---

## Problem 2

### (a)

-   **Tip**: This problem can also be solved using a `Deterministic Polynomial Algorithm`

```typescript
const kThMin = (array: number[], k: number): number => {
    const n = array.length;
    let sorted = [];

    // Generate each permutation of array numbers O(n) [Parallel]
    for (let i = 0 ; i < n ; i++) {
        let j = fork({0,1,...,n-1});
        if (sorted[j] != null) {
            exit(-1);
        }
        sorted[j] = array[i];
    }

    // Validate generated permutation is sorted O(n) [Sequential]
    for (let i = 1 ; i < n ; i++) {
        if (sorted[i-1] > sorted[i]) {
            exit(-1);
        }
    }

    // Return kTh element of sorted array
    return sorted[k];
};
```

---

### (b)

```typescript
const SSSP = (array: number[], expectedSum: number): number[] => {
    const n = array.length;
    let indices = [];

    // Generate each permutation of indices select/drop O(n) [Parallel]
    for (let i = 0 ; i < n ; i++) {
        indices[i] = fork({0,1});
    }

    // Validate generated sum of select element indices is equals to expected value O(n) [Sequential]
    let sum = 0;
    for (let i = 0 ; i < n ; i++) {
        sum += array[i] * indices[i];
    }
    if (sum != expectedSum) {
        exit(-1);
    }

    // Return element indices arrays with the sum equals to expectedSum
    return indices;
};
```

---

### (c)

```typescript
const DHC = (graph: number[][]): number[] => {
    const n = array.length;
    let vertices = [];

    // Generate each permutation of graph vertices O(n) [Parallel]
    for (let i = 0 ; i < n ; i++) {
        let j = fork({0,1,...,n-1});
        if (vertices[j] != null) {
            exit(-1);
        }
        vertices[j] = i;
    }

    // Validate for each two consecutive vertices, an edge exists O(n) [Sequential]
    for (let i = 1 ; i < n ; i++) {
        if (graph[vertices[i-1]][vertices[i]] == 0) {
            exit(-1);
        }
    }

    // Validate for first and last vertices, an edge exists O(1)
    if (graph[vertices[0]][vertices[n-1]] == 0) {
        exit(-1);
    }

    // Return vertices sequence of hamiltonian cycle
    return vertices;
};
```

---

### (d)

```typescript
const threeColoring = (graph: number[][]): number[] => {
    const n = array.length;
    let colors = [];

    // Generate each permutation of vertex colors O(n) [Parallel]
    for (let i = 0 ; i < n ; i++) {
        colors[i] = fork({0,1,2});
    }

    // Validate for each edge, two side vertices does not have the same color O(n^2) [Sequential]
    for (let i = 0 ; i < n ; i++) {
        for (let j = 0 ; j < n ; j++) {
            if (graph[i][j] != null && colors[i] == colors[j]) {
                exit(-1);
            }
        }
    }

    // Return vertex colors sequence
    return colors;
};
```

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
