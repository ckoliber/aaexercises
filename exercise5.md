# AAD Answers

-   **Answer Set**: No. 05
-   **Full Name**: Mohammad Hosein Nemati
-   **Student Code**: `610300185`

---

## Problem 1

The `Randomized Las Vegas` version of **n-Queen** algorithm:

1. Iterate over all rows
2. Try to find `all available cells` for queen
3. Select `randomly` one `possible cell` and place queen
4. If there is `no available` cell, `fail`

This algorithm may fail, without any solution, for this kind of problem, we run the algorithm until get a possible solution

```cpp
int[] nQueenLV(int n) {
    int[] result;

    // Iterate over all rows and try to place one queen in each
    for (int i = 0 ; i < n ; i++) {
        // Find all available columns in this row, for new queen
        int[] availableColumns = findAvailableColumns(i);

        // Check there are available columns
        if (availableColumns.size() <= 0) {
            throw new Exception("No possible column");
        }

        // Select one column randomly
        int column = random({1..availableColumns.size()});
        result.push(column);
    }

    return result;
}

// Run algorithm until getting a possible solution
while(!nQueenLV())
```

---

## Problem 2

This problem is the **Dual Version** of **NonEquality** problem

1. Generate a `random prime number` in range $\{2, n^2\}$
2. Compute $r = (x \;mod\; p)$
3. Send $M = <p, r>$ as message
4. Compute $s = (y \;mod\; p)$
5. Check $r \overset{?}{=} s$

The communication complexity is:

$$
\begin{aligned}
    & M = <p, r>
    \\
    & max\{p\} = n^2 \implies \log(n^2)
    \\
    & max\{r\} = p-1 \implies \log(n^2)
    \\
    \\ \implies
    & bits(M) = 4.log(n)
\end{aligned}
$$

Now we can find the probabilities:

1. $(x = y) \implies Pr[A(x,y) = 1] = 1$
2. $(x \neq y) \implies Pr[A(x,y) = 0] \geq \frac{1}{2}$

---

### Proof (1)

We want to proof:

$$
\begin{aligned}
    & (x = y) \implies Pr[A(x,y) = 1] = 1
\end{aligned}
$$

So:

$$
\begin{aligned}
    & \forall p \in [2,n^2] : (x \;mod\; p) = (y \;mod\; p) \implies r = s
    \\ \implies
    & Pr[A(x,y) = 1] = Pr[r = s] = 1
\end{aligned}
$$

---

### Proof (2)

We want to proof:

$$
\begin{aligned}
    & (x \neq y) \implies Pr[A(x,y) = 0] \geq \frac{1}{2}
\end{aligned}
$$

There are maximum $n-1$ prime numbers like $L$ where:

$$
\begin{aligned}
    & (x \neq y) \;and\; (x \;mod\; L) = (y \;mod\; L)
\end{aligned}
$$

Also, we know that in the range of $[2,n^2]$, there are at most, $\frac{n^2}{ln(n^2)}$ prime numbers, so:

$$
\begin{aligned}
    & (x \neq y) \implies Pr[r = s] = \frac{n-1}{\frac{n^2}{ln(n^2)}} \leq \frac{2.ln(n)}{n} \leq \frac{1}{2}
\end{aligned}
$$

So we can find the $Pr[r \neq s]$:

$$
\begin{aligned}
    & Pr[r \neq s] = 1 - Pr[r = s]
    \\
    \\ \implies
    & Pr[r \neq s] \geq \frac{1}{2}
\end{aligned}
$$

---

## Problem 3

The `Randomized One-Sided Error Monte Carlo (OSE-MC)` version of **Matrix Multiplication Verifier** algorithm:

1. Generate a $1 \times n$ vector contains `random bits`
2. Compute $P = A \times (B \times R) - (C \times R)$
3. Return `false` with $Prob = 1$ if $P$ is non zero vector
4. Return `true` with $Prob \geq \frac{1}{2}$ if $P$ is zero vector

For example:

$$
\begin{aligned}
    & AB = \begin{bmatrix}
        2 & 3
        \\
        3 & 4
    \end{bmatrix}
    \begin{bmatrix}
        1 & 0
        \\
        1 & 2
    \end{bmatrix}
    \overset{?}{=}
    \begin{bmatrix}
        6 & 5
        \\
        8 & 7
    \end{bmatrix} = C
    \\
    & R = \begin{bmatrix}
        1 \\ 1
    \end{bmatrix}
    \\
    \\
    & P = A \times (B \times R) - (C \times R)
    \\ =
    & \begin{bmatrix}
        2 & 3
        \\
        3 & 4
    \end{bmatrix}
    \times (
        \begin{bmatrix}
            1 & 0
            \\
            1 & 2
        \end{bmatrix}
        \times
        \begin{bmatrix}
            1 \\ 1
        \end{bmatrix}
    ) - (
        \begin{bmatrix}
            6 & 5
            \\
            8 & 7
        \end{bmatrix}
        \times
        \begin{bmatrix}
            1 \\ 1
        \end{bmatrix}
    ) =
    \begin{bmatrix}
        0 \\ 0
    \end{bmatrix}
    \\
    \\ \implies
    & Pr[AB = C] \geq \frac{1}{2}
\end{aligned}
$$

Now, we want to proof that the algorithm is **OSE-MC**:

1. $\forall x \in L : Pr[P = 0] = 1$
2. $\forall x \not\in L : Pr[P \neq 0] \geq \frac{1}{2}$

---

### Proof (1)

We want to proof:

$$
\begin{aligned}
    & A \times B = C \implies Pr[P = 0] = 1
\end{aligned}
$$

So:

$$
\begin{aligned}
    & A \times (B \times R) - (C \times R) =
    \\
    & (A \times B) \times R - (C) \times R =
    \\
    & (A \times B - C) \times R
\end{aligned}
$$

We know that $A \times B = C$ so $A \times B - C = 0$:

$$
\begin{aligned}
    & (A \times B - C) \times R =
    \\
    & 0 \times R = 0
\end{aligned}
$$

So, regardless of $R$ values, the $P$ is always $0$

---

### Proof (2)

We want to proof:

$$
\begin{aligned}
    & A \times B \neq C \implies Pr[P \neq 0] \geq \frac{1}{2}
\end{aligned}
$$

So:

$$
\begin{aligned}
    & P = \underbrace{(A \times B - C)}_{D} \times R
    \\
    \\
    & D = A \times B - C = (d_{ij})
\end{aligned}
$$

Now, we must have a cell in $D$ where $d_{ij} \neq 0$, using **Bayes Theorem** we can partition over $y$:

$$
\begin{aligned}
    & p_i = \sum_{k=1}^{n} d_{ik} r_k = d_{i1} r_1 + d_{i2} r_2 + \dots + d_{in} r_n = d_{ij} r_j + y
\end{aligned}
$$

Now using **Bayes Theorem** we know that:

$$
\begin{aligned}
    & Pr[p_i=0] = Pr[p_i=0 | y=0] . Pr[y=0] + Pr[p_i=0 | y\neq0] . Pr[y\neq0]
    \\
    \\ \implies
    & Pr[p_i=0 | y=0] = Pr[r_j=0] = \frac{1}{2}
    \\
    & Pr[p_i=0 | y\neq0] = Pr[r_j=1 \land d_{ij} = -y] \leq Pr[r_j = 1] = \frac{1}{2}
\end{aligned}
$$

So we can find the $Pr[P = 0]$:

$$
\begin{aligned}
    & Pr[P = 0] = Pr[p_1=0 \land p_2=0 \land \dots \land p_n=0] \leq Pr[p_i] = 0 \leq \frac{1}{2}
\end{aligned}
$$

So we can find the $Pr[P \neq 0]$:

$$
\begin{aligned}
    & Pr[P \neq 0] = 1 - Pr[P = 0]
    \\
    \\ \implies
    & Pr[P \neq 0] \geq \frac{1}{2}
\end{aligned}
$$

---
