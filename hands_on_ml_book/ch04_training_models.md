# Chapter 4: Training Models

## 1. Linear Regression: The Math
The goal is to find the best weights (parameters) to fit the data.

### The Prediction Formula
$$\hat{y} = \theta_0 + \theta_1 x_1 + \dots + \theta_n x_n$$
* **$\hat{y}$:** The prediction.
* **$n$:** Number of features.
* **$x_i$:** The feature value (e.g., square footage).
* **$\theta_j$:** The model parameter (weight). $\theta_0$ is the bias/intercept.

### Vectorized Form
Instead of a long loop, we use **Linear Algebra** to calculate everything at once.
$$\hat{y} = \theta^T \cdot \mathbf{x}$$
* **$\theta^T$ (Theta Transpose):** The parameters row vector.
* **$\mathbf{x}$ (Features):** The feature column vector.
* **Dot Product ($\cdot$):** Multiplies matching pairs and sums them up (like calculating a grocery bill: $price \times quantity$).

### The "Bias Trick"
To make the math work, Scikit-Learn adds a "dummy" feature ($x_0$) that is always **1** to every instance. This allows the model to have an intercept ($\theta_0$) that doesn't depend on the input features.

---

## 2. Finding the Best Parameters (Two Ways)

### Method A: The Normal Equation (The Math Formula)
A mathematical formula that calculates the perfect parameters directly, without "training."
* **Formula:** $\hat{\theta} = (X^T \cdot X)^{-1} \cdot X^T \cdot y$
* **Pros:** Finds the exact answer instantly. No scaling required.
* **Cons:** Extremely slow if you have too many *features* (columns). It crashes your RAM if the dataset is massive.
* **Used by:** `LinearRegression()`

### Method B: Gradient Descent (The Hiker)
An iterative optimization algorithm.

#### The Analogy: The Blind Hiker
* **The Scenario:** You are lost on a mountain at night (or in fog). You want to get to the bottom of the valley (Lowest Error).
* **The Method:**
    1.  **Measure:** You feel the slope under your feet (**Calculate the Gradient**).
    2.  **Step:** You take a step in the steepest downhill direction.
    3.  **Repeat:** You keep stepping until the slope is flat (Minimum cost).
* **Learning Rate ($\eta$):** The size of your step.
    * *Too Small:* Takes forever to reach the bottom.
    * *Too Large:* You jump across the valley and diverge (error gets worse).
* **Feature Scaling:** **Crucial.** If features are on different scales, the "mountain" is warped (like a long canyon), and the hiker zig-zags inefficiently. `StandardScaler` turns the mountain into a nice symmetrical bowl.

---

## 3. Types of Gradient Descent

### 1. Batch Gradient Descent (The "Careful Committee")
* **How it works:** Uses the **entire** dataset to calculate the slope for *every single step*.
* **Pros:** Walks in a straight line to the optimal solution.
* **Cons:** Very slow on large datasets. Requires loading all data into memory.

### 2. Stochastic Gradient Descent (The "Drunk Hiker")
* **How it works:** Picks **one random instance** at a time and calculates the slope based only on that.
* **Pros:** Extremely fast. Can jump out of "local minima" (small potholes) because it's erratic.
* **Cons:** Bounces around wildly. Never settles down perfectly.
* **Fix:** Use a **Learning Schedule** (start fast, slow down as you get closer).
* **Used by:** `SGDRegressor`

### 3. Mini-batch Gradient Descent (The "Goldilocks" / GPU Friendly)
* **How it works:** Uses a **small random set** (e.g., 32 or 64 instances) to calculate the step.
* **Pros:** The sweet spot. More stable than Stochastic, faster than Batch.
* **Hardware Bonus:** This is the only one that leverages **GPUs** (which love doing math on small matrices).

---

## 4. Comparison of Algorithms

| Algorithm | Large $m$ (Rows) | Large $n$ (Features) | Scaling Required? | Scikit-Learn Class |
| :--- | :--- | :--- | :--- | :--- |
| **Normal Equation** | Fast | **Slow** | No | `LinearRegression` |
| **Batch GD** | **Slow** | Fast | **Yes** | N/A (Manual loop) |
| **Stochastic GD** | **Fast** | Fast | **Yes** | `SGDRegressor` |
| **Mini-batch GD** | **Fast** | Fast | **Yes** | `SGDRegressor` (partial_fit) |

*Note: "Fast" for features means it scales well mathematically, unlike the Normal Equation which calculates a matrix inverse.*
