# Classification

## 1. The Basics

### Binary Classification
- **Goal:** Classify data into **two** buckets (e.g., "Is it a 5?" or "Is it NOT a 5?").
- **The Model:** `SGDClassifier` (Stochastic Gradient Descent).
    - Efficient for huge datasets.
    - Random by nature (needs `random_state=42` to be reproducible).

### Accuracy is Dangerous
- **The Trap:** On "skewed" datasets (where 90% of data is one class), a dumb model that guesses "False" every time will have **90% accuracy**.
- **Lesson:** Never use Accuracy alone for classifiers. You need the **Confusion Matrix**.

---

## 2. The Confusion Matrix (The Truth Table)

The only way to really see how your model is messing up.

| | **Predicted: Negative** | **Predicted: Positive** |
| :--- | :--- | :--- |
| **Actual: Negative** | **True Negative (TN)**<br>*(Correctly rejected)* | **False Positive (FP)**<br>*(False Alarm / Type I)* |
| **Actual: Positive** | **False Negative (FN)**<br>*(Missed Target / Type II)* | **True Positive (TP)**<br>*(Hit)* |

- **Row = Actual Class** (The Truth)
- **Column = Predicted Class** (The Guess)

---

## 3. Precision & Recall (The Metrics)

Since you can't have it all, you have to choose what matters more.

### Precision ("The Trust Factor")
- **Question:** "When the model claims it's a 5, how often is it right?"
- **Formula:** $TP / (TP + FP)$
- **Analogy:** **The Spam Filter.**
    - You want high Precision. You'd rather let some Spam in (FN) than accidentally delete an important email (FP).

### Recall / Sensitivity ("The Dragnet")
- **Question:** "Of all the actual 5s in the deck, how many did we find?"
- **Formula:** $TP / (TP + FN)$
- **Analogy:** **Cancer Screening.**
    - You want high Recall. You'd rather get a False Alarm (FP) than miss a patient who actually has cancer (FN).

### The F1 Score
- A single score that combines Precision and Recall. Good for comparing models, but hides the trade-off details.

---

## 4. The Trade-Off (The Threshold)

You can't increase Precision without lowering Recall (and vice versa).

- **The Decision Function:** The model assigns a "score" to every instance.
- **The Threshold:** The line in the sand (e.g., score > 0).
    - **Raise Threshold:** Model gets picky. **Precision UP**, **Recall DOWN**.
    - **Lower Threshold:** Model gets lenient. **Recall UP**, **Precision DOWN**.
- **Visualization:** Use `precision_recall_curve` to pick the perfect threshold for your specific business problem.

---

## 5. Other Tools

### ROC Curve (Receiver Operating Characteristic)
- Plots **True Positive Rate (Recall)** vs. **False Positive Rate**.
- **AUC (Area Under Curve):**
    - **1.0** = Perfect Classifier.
    - **0.5** = Random Guessing.
    - Use this when you care more about "ranking" than specific probability outputs.

---

## 6. Multiclass Classification strategies

Binary classifiers (like SGD or SVM) pick between 2 things. How do we pick between 10 digits (0-9)?

### OvR (One-vs-Rest) / OvA (One-vs-All)
- **Strategy:** "King of the Hill."
- **Method:** Train 10 classifiers (0 vs All, 1 vs All...). Run all 10. The one with the highest score wins.
- **Usage:** The default for almost everything (Logistic Regression, etc.).

### OvO (One-vs-One)
- **Strategy:** "Round Robin Tournament."
- **Method:** Train a classifier for every pair (0 vs 1, 0 vs 2, 1 vs 2...).
- **Math:** For 10 classes, that's 45 classifiers!
- **Usage:** Only used for **SVMs** (because SVMs prefer small data chunks). Scikit-Learn switches to this automatically when needed.

---

## 7. Cross-Validation Recap (for Classification)

- **Function:** `cross_val_score` or `cross_val_predict`.
- **StratifiedKFold:** The standard way splits are done in classification. It ensures each "fold" has the same ratio of classes as the whole dataset (e.g., if 20% of data is "5s", every test fold will have 20% "5s").
