# AAD Answers

-   **Answer Set**: No. 03
-   **Full Name**: Mohammad Hosein Nemati
-   **Student Code**: `610300185`

---

## Problem 1

In this problem we introduce the **Local Transformation** function for each problem:

### (a)

-   The **Structure** of each answer is an **Array** with size of **Number of vertices**, each item contains a number representing the **Vertex Color**:

    $$
    \begin{aligned}
        & \alpha = (0, 1, 1, 3, \dots, k)
    \end{aligned}
    $$

-   The **Initial Answer** can be an array from $0$ to $n$, each vertex has it's own **Unique Color**:

    $$
    \begin{aligned}
        & \alpha_{init} = (0, 1, 2, 3, \dots, n)
    \end{aligned}
    $$

-   The **Cost Function** returns the number of unique colors in answer array, the goal is to **Minimize** the cost:

    $$
    \begin{aligned}
        & Cost(0, 1, 3, 1, 5) = 4
    \end{aligned}
    $$

```js
answer[] GraphColorTransform(problem, answer) {
    // Create empty result set
    let result = [];

    for (i in answer) {
        // Clone the answer
        temp = answer;

        // Reduce the color value at index i
        temp[i]--;

        // Time-Complexity: O(e) = O(n^2)
        // Validate graph does not contains duplicate colors at each edge
        if (validate(problem, temp)) {
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

---

### (b)

-   The **Structure** of each answer is an **Array** with size of **Number of vertices**, each item contains a number representing the **Vertex Select or Not (0,1)**:

    $$
    \begin{aligned}
        & \alpha = (0, 1, 0, 0, \dots, 1)
    \end{aligned}
    $$

-   The **Initial Answer** can be an array full of $1$:

    $$
    \begin{aligned}
        & \alpha_{init} = (1, 1, 1, 1, \dots, 1)
    \end{aligned}
    $$

-   The **Cost Function** returns the number of **1** in answer array, the goal is to **Minimize** the cost:

    $$
    \begin{aligned}
        & Cost(0, 1, 0, 1, 0) = 2
    \end{aligned}
    $$

```js
answer[] VertexCoverTransform(problem, answer) {
    // Create empty result set
    let result = [];

    for (i in answer) {
        // Clone the answer
        temp = answer;

        // Toggle the vertex select/drop
        toggle(temp[i]);

        // Time-Complexity: O(e) = O(n^2)
        // Validate all edges contains atleast a selected vertex
        if (validate(problem, temp)) {
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

---

### (c)

-   The **Structure** of each answer is an **Array** with size of **Number of vertices**, each item contains a number representing the **Vertex Select or Not (0,1)**:

    $$
    \begin{aligned}
        & \alpha = (0, 1, 0, 0, \dots, 1)
    \end{aligned}
    $$

-   The **Initial Answer** can be an array full of $0$:

    $$
    \begin{aligned}
        & \alpha_{init} = (0, 0, 0, 0, \dots, 0)
    \end{aligned}
    $$

-   The **Cost Function** returns the number of **1** in answer array, the goal is to **Maximize** the cost:

    $$
    \begin{aligned}
        & Cost(0, 1, 0, 1, 0) = 2
    \end{aligned}
    $$

```js
answer[] CliqueTransform(problem, answer) {
    // Create empty result set
    let result = [];

    for (i in answer) {
        // Clone the answer
        temp = answer;

        // Toggle the vertex select/drop
        toggle(temp[i]);

        // Time-Complexity: O(e) = O(n^2)
        // Validate selected vertices is a complete graph
        if (validate(problem, temp)) {
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

---

### (d)

-   The **Structure** of each answer is an **Array** with size of **Number of items**, each item contains a number representing the **Item Select or Not (0,1)**:

    $$
    \begin{aligned}
        & \alpha = (0, 1, 0, 0, \dots, 1)
    \end{aligned}
    $$

-   The **Initial Answer** can be an array full of $0$:

    $$
    \begin{aligned}
        & \alpha_{init} = (0, 0, 0, 0, \dots, 0)
    \end{aligned}
    $$

-   The **Cost Function** returns the sum of profits for selected items in answer array, the goal is to **Maximize** the cost:

    $$
    \begin{aligned}
        & Cost(0, 1, 0, 1, 0) = x_1 + x_3
    \end{aligned}
    $$

```js
answer[] KnapsackTransform(problem, answer) {
    // Create empty result set
    let result = [];

    for (i in answer) {
        // Clone the answer
        temp = answer;

        // Toggle the item select/drop
        toggle(temp[i]);

        // Time-Complexity: O(n)
        // Validate selected weights is not greater than maximum weight
        if (validate(problem, temp)) {
            result.add(temp);
        }
    }

    return result;
}
```

As we can see the complexity of the above local-transformation function is:

$$
\begin{aligned}
    & O(n) \times O(n) = O(n^2)
\end{aligned}
$$

---

### (e)

-   The **Structure** of each answer is an **Array** with size of **Number of cut points**, each item contains a number representing the **Point Cut or Connect (0,1)**:

    $$
    \begin{aligned}
        & \alpha = (0, 1, 0, 0, \dots, 1)
    \end{aligned}
    $$

-   The **Initial Answer** can be an array full of $0$:

    $$
    \begin{aligned}
        & \alpha_{init} = (0, 0, 0, 0, \dots, 0)
    \end{aligned}
    $$

-   The **Cost Function** returns the sum of profits for rods with cutted points in answer array, the goal is to **Maximize** the cost:

    $$
    \begin{aligned}
        & Cost(0, 1, 0, 1, 0) = x_0 + x_2 + x_4
    \end{aligned}
    $$

```js
answer[] CutRodTransform(problem, answer) {
    // Create empty result set
    let result = [];

    for (i in answer) {
        // Clone the answer
        temp = answer;

        // Toggle the point cut/connect
        toggle(temp[i]);

        // Time-Complexity: O(1)
        // Validate nothing
        if (true) {
            result.add(temp);
        }
    }

    return result;
}
```

As we can see the complexity of the above local-transformation function is:

$$
\begin{aligned}
    & O(n) \times O(1) = O(n)
\end{aligned}
$$

---

## Problem 2

---

## Problem 3

---
