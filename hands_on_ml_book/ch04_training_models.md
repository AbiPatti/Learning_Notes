# Training Models

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

---

## 5. Polynomial Regression (Fitting Curved Data)

What if your data isn't a straight line? You can still use a Linear model to fit non-linear data.

### The Trick
You take your original features (e.g., $x$) and add **powers** of them as new features (e.g., $x^2$, $x^3$).
* **Code:** `PolynomialFeatures(degree=2, include_bias=False)`
* **Result:** If you have one feature $x$, the model now sees two features: $x$ and $x^2$.
* **The Magic:** The model is still "linear" (it's just finding weights for these new features), but the resulting plot is a **curve**.

### Interaction Terms (Warning)
* If you have multiple features (e.g., $a$ and $b$) and use `degree=2`, Scikit-Learn creates **interaction terms** too.
* You get: $a^2$, $b^2$, and **$ab$**.
* **Danger:** The number of features explodes quickly! ($n$ features with degree $d$ creates roughly $n^d$ new features).

---

## 6. Learning Curves (Diagnosing the Model)

How do you know if your model is **Overfitting** or **Underfitting**? You plot the error (RMSE) as the training set gets bigger.

### 1. Underfitting (High Bias)
* **The Graph:**
    * **Training Error:** Starts low but goes up quickly and flattens out at a **high error**.
    * **Validation Error:** Starts high and drops slightly to meet the training curve.
    * **The Key Sign:** Both curves plateau close together at a **high error**.
* **Diagnosis:** The model is too simple.
* **Fix:** Adding more data **will not help**. You need a more complex model (e.g., increase polynomial degree).

### 2. Overfitting (High Variance)
* **The Graph:**
    * **Training Error:** Stays very low (it memorized the data).
    * **Validation Error:** Stays high.
    * **The Key Sign:** There is a huge **GAP** between the two curves.
* **Diagnosis:** The model is too complex relative to the amount of data.
* **Fix:** Adding more data **will help** (it closes the gap). Or, simplify the model (regularization).

---

## 7. The Bias/Variance Trade-off

Every model's error comes from three sources:

1.  **Bias (Wrong Assumptions):** The model is too simple (e.g., fitting a line to a curve). Leads to **Underfitting**.
2.  **Variance (Sensitivity):** The model is too sensitive to small changes in the training data. Leads to **Overfitting**.
3.  **Irreducible Error:** Noisiness in the data itself. You can't fix this (unless you clean the data).

**The Goal:** Find the sweet spot—low enough bias to capture the pattern, but low enough variance to ignore the noise.

---

## 8. Regularized Linear Models (Constraining the Model)

Standard Linear Regression often **overfits** the training data (it tries too hard to hit every noisy point). We fix this by **Regularization**: forcing the model to keep the weights (parameters) small.

### Do these replace Linear Regression?
**Yes, practically speaking.**
You should almost *never* use plain Linear Regression. You should almost always use at least a little bit of regularization (like Ridge).

### 1. Ridge Regression ($\ell_2$ Norm)
* **The Concept:** Adds a "penalty" equal to the square of the magnitude of coefficients.
* **The Effect:** Forces weights to be small, but **never zero**. It shrinks the model to be stable.
* **The Dial:** `alpha` ($\alpha$).
    * $\alpha=0$: Plain Linear Regression.
    * $\alpha=High$: Weights end up very close to zero (Result is a flat line through the data's mean).
* **Must Do:** You **must scale the data** (`StandardScaler`) before using Ridge, or it won't work effectively.

### 2. Lasso Regression ($\ell_1$ Norm)
* **The Concept:** Adds a "penalty" equal to the absolute value of the magnitude of coefficients.
* **The Magic:** It tends to eliminate the weights of the least important features entirely (setting them to **zero**).
* **The Result:** It performs automatic **Feature Selection**. It gives you a "sparse model" (only the useful features remain).

### 3. Elastic Net
* **The Concept:** A mix of both Ridge and Lasso.
* **The Dial:** `l1_ratio` (mix ratio).
    * `l1_ratio=0`: It's Ridge.
    * `l1_ratio=1`: It's Lasso.
    * `l1_ratio=0.5`: Half and half.

---

## 9. Which One Should You Use? (Cheatsheet)

| Model | When to use it |
| :--- | :--- |
| **Plain Linear Regression** | Almost never. Only if you have tons of data and very few features. |
| **Ridge Regression** | **The Default.** This should be your starting point. |
| **Lasso Regression** | When you suspect **only a few features are useful**. It filters out the junk. |
| **Elastic Net** | **Better than Lasso** when features are correlated (e.g., "square footage" and "number of rooms"). Lasso might erratically pick one and drop the other; Elastic Net groups them. |

---

## 10. Early Stopping (The "Beautiful Free Lunch")

A different way to regularize iterative models (like Gradient Descent).

* **The Method:**
    1.  Train the model step-by-step (epoch by epoch).
    2.  Measure the **Validation Error** at every step.
    3.  **STOP** as soon as the Validation Error reaches a minimum and starts going back up.
* **Why:** When validation error starts rising, the model has begun to **overfit**. Stopping there saves the "best" version of the model.

---

## 11. Logistic Regression (Classification)

Despite the name "Regression," this is used for **Classification** (predicting a category).

### Estimating Probabilities
* Instead of predicting a raw number (like house price), it predicts the **probability** that an instance belongs to the positive class (e.g., "There is an 80% chance this is Spam").
* **The Math:** It uses a **Sigmoid Function** ($\sigma$) to squash the output between 0 and 1.
    * Output $< 0.5$: Predict Class 0 ("Not Spam")
    * Output $\ge 0.5$: Predict Class 1 ("Spam")

### Training and Cost Function
* **Cost Function:** "Log Loss".
    * If actual class is **1** and model predicts **0** -> **Huge Penalty**.
    * If actual class is **0** and model predicts **1** -> **Huge Penalty**.
* **Optimization:** There is no "Normal Equation" for this. You **must** use Gradient Descent (or similar solvers) to find the parameters.

---

## 12. Softmax Regression (Multinomial Logistic Regression)

What if you have more than 2 classes (e.g., Classifying plants into Type A, B, or C)?

* **OvR vs Softmax:** You *could* use One-vs-Rest (training 3 binary models), OR you can use **Softmax**.
* **How it works:**
    1.  The model calculates a "score" for every class (A, B, C).
    2.  It applies the **Softmax Function** to turn those scores into probabilities.
    3.  The probabilities sum to 1 (e.g., A: 10%, B: 70%, C: 20%).
    4.  It picks the class with the highest probability.
* **Cross Entropy:** The cost function used to train Softmax. It penalizes the model when it estimates a low probability for the target class.

**Key Note:** Softmax predicts **one** class at a time (mutually exclusive). It cannot handle multi-label output (e.g., "This image contains a Dog AND a Cat").
