# AAD Answers

-   **Answer Set**: No. 04
-   **Full Name**: Mohammad Hosein Nemati
-   **Student Code**: `610300185`

---

## Problem 1

$$
\begin{aligned}
    & m: \text{Number of machines}
    \\
    & n: \text{Number of jobs}
    \\
    \\
    & n = 2m + 1
    \\
    & jobs = \begin{Bmatrix}
        m
        \\
        m+1 & m+2 & \dots & 2m-1 & 2m
        \\
        m+1 & m+2 & \dots & 2m-1 & 2m
    \end{Bmatrix}
\end{aligned}
$$

For example:

$$
\begin{aligned}
    & m = 3
    \\
    & n = 2 \times 3 + 1 = 7
    \\
    & jobs = \{3, 4, 4, 5, 5, 6, 6\}
    \\
    \\
    & \text{Greedy}:
    \\
    & Machine_1 = \{6, 4, 3\} = 13
    \\
    & Machine_2 = \{6, 4\} = 10
    \\
    & Machine_3 = \{5, 5\} = 10
    \\
    \\
    & \text{Optimal}:
    \\
    & Machine_1 = \{6, 5\} = 11
    \\
    & Machine_2 = \{6, 5\} = 11
    \\
    & Machine_3 = \{4, 4, 3\} = 11
\end{aligned}
$$

---

## Problem 2

We can think of the cut as a partition of verticesl $C = (S, V - S)$ where $S \subseteq V$.
We can switch between different cuts by moving vertices across the cut, in to or out of $S$.
Moving $v$ across the cut swaps its cut edges with its non-cut edges.
This increase the value of the cut when the total weight of its non-cut edges exceeds the weight of the cut edges.
If $v \in S$ (and similarly if $v \in V - S$)

Now we will introduce a **Greedy Algorithm** for this problem, and find it's **Complexity**, then we will prove that algorithm is **2-Approximation**

---

### Algorithm

The algorithm is very simple:

1. Begin with an arbitrary cut (e.g. $S = \emptyset$).
2. While there are greedy moves improving the cost, perform them.

Now, we will introduce the **Local-Search Transform Function** for the problem, then we will compute the maximum number of algorithm iterations and find the final algorithm complexity:

-   The **Structure** of each answer is an **Array** with size of **Number of vertices**, each item contains a number representing the **Vertex Is in S or Not**:

    $$
    \begin{aligned}
        & \alpha = (0, 1, 0, 1, \dots, 0)
    \end{aligned}
    $$

-   The **Initial Answer** can be any array of $0$ or $1$, represents the initial selected items as $S$:

    $$
    \begin{aligned}
        & \alpha_{init} = (0, 0, 0, 1, \dots, 1)
    \end{aligned}
    $$

-   The **Cost Function** returns the number of edges between $S$ and $\overline{S}$, the goal is to **Maximize** the cost:

    $$
    \begin{aligned}
        & Cost(0, 1, 0, 1, 1) = 3
    \end{aligned}
    $$

```js
answer[] MaxCutTransform(problem, answer) {
    // Create empty result set
    let result = [];

    for (i in answer) {
        // Clone the answer
        temp = answer;

        // Toggle the vertex select (goto S)/drop (goto S_complete)
        toggle(temp[i]);

        // Time-Complexity: O(e) = O(n^2)
        // Validate number of edges between S,Scomplement is increased
        if (cost(temp) > cost(answer)) {
            result.add(temp);
        }
    }

    return result;
}
```

As we can see the complexity of the above local-transformation function is:

$$
\begin{aligned}
    & O(n) \times O(e) = O(n^3)
\end{aligned}
$$

In each step of algorithm iteration over neighbors, one improvement will occur, each improvement will increase the **Number of edges between $S$ and $\overline{S}$**, so in the worst case (**Bipartite Graph**) algorithm will iterate for the number of edges:

$$
\begin{aligned}
    & \text{Iterations} \in O(e)
    \\
    & \text{Transformation} \in O(n^3)
    \\
    \\ \implies
    & \text{Complexity} \in O(e) \times O(n^3) = O(n^5)
\end{aligned}
$$

So this approximation algorithm is **Polynomial Time**

---

### Approximation

Now, we want to find the **Approximation Ratio** of algorithm:

-   At the time of termination, for every vertex $u \in S$:

    $$
    \begin{aligned}
        & \sum_{v \in \overline{S}, v \neq u} w(u, v) \geq \sum_{v \in S, v \neq u} w(u, v)
        \\
        \\ \implies
        & \sum_{u \in S}\sum_{v \in \overline{S}} w(u, v) \geq \sum_{u \in S}\sum_{v \in S} w(u, v)
    \end{aligned}
    $$

-   Similarly, for any vertex $u \in \overline{S}$:

    $$
    \begin{aligned}
        & \sum_{v \in S, v \neq u} w(u, v) \geq \sum_{v \in \overline{S}, v \neq u} w(u, v)
        \\
        \\ \implies
        & \sum_{v \in \overline{S}}\sum_{v \in S} w(u, v) \geq \sum_{v \in \overline{S}}\sum_{v \in \overline{S}} w(u, v)
    \end{aligned}
    $$

-   By adding these equations together, we have:

    $$
    \begin{aligned}
        & 2.\sum_{v \in \overline{S}}\sum_{v \in S} w(u, v) \geq \sum_{v \in S}\sum_{v \in S} w(u, v) + \sum_{v \in \overline{S}}\sum_{v \in \overline{S}} w(u, v)
        \\
        \\ \implies
        & 2.\sum_{v \in \overline{S}}\sum_{v \in S} w(u, v) \geq Opt
        \\
        \\ \implies
        & 2.w(C) \geq Opt
        \\ \implies
        & \frac{Opt}{w(C)} \leq 2
    \end{aligned}
    $$

So this algorithm is **2-Approximation**.

---

## Problem 3

1. Take the minimum-weight spanning tree (**MST**) of the **TSP** graph.
   The MST can be computed in polynomial time (**Kruskal**, **Prim**)

    - We can prove that $Weights(MST) \leq Opt$

        > **Using Contradiction**:
        >
        > Assume an instance of a **Metric TSP** with $Opt \lt MST$.
        >
        > Removing an edge from Opt creats an acyclic graph hitting every node once,
        >
        > therefore it is a spanning tree with weight $T < Opt$,
        >
        > So $T < Opt < MST$.
        >
        > Therefore MST was not minimal.
        >
        > **Contradiction**.

2. Do a depth-first search (**DFS**) of the MST, hitting every edge exactly twice. This **seudotour** $PT$ has $Cost=2 \times MST \leq 2 \times Opt$. Write
   down each vertex as you visit it.

3. To go from $PT$ to $T*$: Rewrite the list of vertices, writing each vertex only the first time it appears in $PT$

    - We can prove that $Cost(T*) \leq PT$ therefore $Cost(T*) \leq 2 \times Opt$

        > **Using Triangle Inequality**:
        >
        > Going from A to B must be cheaper than A to C to B.

---

## Problem 4

---

## Problem 5

---

## Problem 6

First instead of solving this problem directly, we solve a slightly harder problem where we have an additional requirement that some numbers $p$ and $q$ must be in $S_1$ and $S_2$.

---

### Restricted Subset Sum Problem

In this problem, in addition to $a_1, \dots,a_n$, we are given two integers $1 \leq p \lt q \leq n$ as part of the input.

Our goal is to find $S_1$ and $S_2$ as before, but we are restricted to $S_1$ and $S_2$ such that $\{max(S_1), max(S_2)\}=\{p, q\}$.

That is, each of $p$ and $q$ appears as the largest number in $S_1$ or $S_2$.

---

### Pseudo-Polynomial time Algorithm

In this section we show that $P(\{a_1, \dots,a_n\}, p, q)$ can be solved in $poly(n,a_q)$ time.

One of many ways to solve this easy problem is to define a table $T_i[x, y]$, for any $i < q$ and $x y$.

This table keeps one pair of disjoint sets $S_1, S_2 \subseteq \{1,...,i\} \cup \{p, q\}$
such that $i \in S_1$ $a_i = x$, $j∈S2$ a $j=y$, and ${max S1,max S2}={p, q}$. This table can
be computed easily as in Algorithm 1. (We note that Algorithm 1 may change the value of some entry Ti[x, y] many
times. This is not necessary and could be avoided. However, we do not try to avoid this to keep the algorithm
description simple.) After this table is computed, we find a
pair x y such that Tq−1[x, y] = ∅ and x/y is minimum.
We output the corresponding sets which yield a ratio of
x/y

---

## Problem 7

---

## Problem 8

---
