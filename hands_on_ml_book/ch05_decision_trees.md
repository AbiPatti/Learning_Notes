## Decision Trees

### The Concept: "The Greedy Manager" (CART Algorithm)
- **How it works:** The algorithm looks at the data and tries to find the **single best question** (e.g., "Is Income > 50k?") to split the group into two cleaner piles.
- **"Greedy":** It doesn't plan ahead. It just makes the best split *right now* and moves to the next node.

### Gini Impurity (The Scoreboard)
- **What is it?** A measure of how "messy" a node is.
- **0.0:** Perfect Purity (The bucket contains *only* Apples).
- **0.5:** Maximum Impurity (The bucket is 50/50 Apples and Bananas).
- **Goal:** The Tree always tries to minimize the Gini score.

### Regularization (Stopping the Tree)
Trees love to overfit. If you don't stop them, they will create a specific rule for every single data point.
- **`max_depth`:** "Stop splitting after 5 levels."
- **`min_samples_leaf`:** "Don't create a bucket unless it has at least 10 people in it."

### Crucial Note: Scaling
- **Does Tree need Scaling (`StandardScaler`)?** **NO.**
- **Why?** Trees only check conditions (`Is x > 5?`). They don't care about the magnitude or "distance" of numbers.
- **Benefit:** You can feed raw data directly into a tree.
