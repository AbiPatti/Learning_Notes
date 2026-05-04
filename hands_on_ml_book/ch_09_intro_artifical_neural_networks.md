# Chapter 9: Introduction to Artificial Neural Networks

## 1. The Deep Learning Renaissance 
Artificial Neural Networks (ANNs) have gone through several "winters" but are now dominating due to:
*   **Massive Datasets:** Enough data exists to properly train complex models.
*   **Compute Power:** GPUs and cloud platforms handle massive matrix multiplications.
*   **Algorithmic Tweaks:** Minor but highly effective improvements to 1990s training algorithms.
*   **Transformers (2017):** Revolutionized handling of diverse data (text, image, audio) and enabled zero/few-shot learning.
*   **Virtuous Cycle:** Better models $\rightarrow$ more funding $\rightarrow$ better models.

---

## 2. The Evolution of the Neuron
### Biological vs. Artificial
*   **Biological:** Cell body, dendrites (inputs), axon (output), synapses. Fire "action potentials" based on neurotransmitter accumulation.
*   **McCulloch-Pitts (1943):** Simple binary threshold model. Capable of computing any logical proposition (AND, OR, NOT).

### The Perceptron (Rosenblatt, 1957)
*   Based on the **Threshold Logic Unit (TLU)**.
*   Computes a weighted sum: $z = w^T x + b$
*   Applies a step function: $h_w(x) = \text{step}(z)$ (usually Heaviside step function).
*   **Dense Layer:** A fully connected layer where every TLU is connected to every input.
*   **Learning:** Based on Hebb's rule ("cells that fire together, wire together"). Weights update to reduce prediction errors.
*   **Fatal Flaw:** Cannot solve trivial non-linear problems like the XOR classification (exclusive OR). 

---

## 3. Multilayer Perceptrons (MLPs) & Backprop
Stacking perceptrons creates an MLP, solving the XOR problem.
*   **Architecture:** Input layer $\rightarrow$ Hidden layer(s) $\rightarrow$ Output layer.
*   **Deep Neural Network (DNN):** An ANN with a deep stack of hidden layers.
*   **Feedforward:** Signal flows only in one direction (input to output).

### Backpropagation (1970/1985)
The engine of modern training. It combines **reverse-mode automatic differentiation** with **gradient descent**.
1.  **Forward Pass:** Feed a mini-batch through the network to get predictions. Keep intermediate results.
2.  **Measure Error:** Use a loss function to evaluate predictions.
3.  **Reverse Pass:** Apply the chain rule backward from the output to the input to measure the error gradient across all connection weights and biases.
4.  **Gradient Descent Step:** Tweak the weights to reduce the error.
*   **Crucial Setup:** Hidden layer weights *must* be initialized randomly to break symmetry, otherwise all neurons learn the exact same thing.

---

## 4. Activation Functions
Step functions have flat gradients, making backpropagation impossible. You must use continuous, differentiable functions to introduce nonlinearity (otherwise, chained layers just equal one linear layer).

*   **Sigmoid:** $\sigma(z) = 1 / (1 + \exp(-z))$. Output range 0 to 1.
*   **Tanh (Hyperbolic Tangent):** S-shaped, output range -1 to 1. Centers data around 0 to speed up convergence.
*   **ReLU (Rectified Linear Unit):** $\max(0, z)$. Fast to compute, no maximum output value. **Default for most hidden layers.**

---

## 5. MLP Architectures

### Regression MLPs
*   **Outputs:** 1 neuron per target dimension.
*   **Output Activation:** None (if predicting any number), ReLU/Softplus (positive numbers only), or Sigmoid/Tanh (bounded ranges).
*   **Loss:** Mean Squared Error (MSE), or Huber loss if dealing with outliers.

### Classification MLPs
*   **Binary:** 1 output neuron, Sigmoid activation, Cross-Entropy loss.
*   **Multilabel Binary:** 1 neuron per label, Sigmoid activation, Cross-Entropy loss.
*   **Multiclass:** 1 neuron per class, Softmax activation, Cross-Entropy loss.

---

## 6. Implementation Notes (Scikit-Learn)

```python
from sklearn.neural_network import MLPClassifier
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import MinMaxScaler

# 1. Define the model
# 2 hidden layers: 300 neurons in first, 100 in second
# early_stopping=True sets aside 10% of training data to prevent overfitting
mlp_clf = MLPClassifier(hidden_layer_sizes=[300, 100], 
                        early_stopping=True, 
                        random_state=42)

# 2. Build pipeline
# Neural networks hate unscaled data. 
# MinMaxScaler is often better than StandardScaler for image pixels (0-255 to 0-1)
pipeline = make_pipeline(MinMaxScaler(), mlp_clf)

# 3. Train and score
pipeline.fit(X_train, y_train)
accuracy = pipeline.score(X_test, y_test)

# Note: Neural nets tend to be overconfident. 
# 99.9% probability output doesn't always mean it's right.
```

## 7. Hyperparameter Tuning Guidelines

*  **Number of Layers:** Deep networks have high parameter efficiency (can learn complex things with exponentially fewer neurons). Start with 1-2 hidden layers. Use Transfer Learning for complex tasks.
*  **Neurons per Layer:** The "stretch pants" approach is easiest—pick a size slightly larger than you think you need, then use regularization/early stopping to prevent overfitting.
*  **Learning Rate:** The most critical parameter. Often found by starting tiny ($10^{-5}$), ramping up to a large value (10), plotting the loss, and picking the point right before loss shoots back up.
*  **Batch Size:** Large sizes use GPU efficiently but can cause instability. If large batches underperform, try small batches (2 to 32) or use learning rate warmups.
