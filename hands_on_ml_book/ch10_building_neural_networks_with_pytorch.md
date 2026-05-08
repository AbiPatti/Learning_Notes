# Chapter 10: Building Neural Networks with PyTorch

## 1. PyTorch Fundamentals
*   **Tensors:** The core data structure. Like NumPy arrays, but support GPU acceleration and auto-differentiation. Default to 32-bit floats for deep learning to save RAM.
*   **Hardware Acceleration:** Tensors and models can be moved to GPUs (or Apple MPS, TPUs) using `.to(device)` to massively speed up matrix operations.
*   **Autograd:** PyTorch's automatic differentiation engine. 
    *   Set `requires_grad=True` on tensors to track operations.
    *   Call `.backward()` on the loss to compute gradients.
    *   Use `with torch.no_grad():` to temporarily disable tracking (e.g., during evaluation).
    *   **Crucial:** Always zero gradients (`optimizer.zero_grad()`) before the next step, as PyTorch accumulates them by default.

## 2. High-Level API (`torch.nn`)
*   **Modules:** The building blocks (e.g., `nn.Linear`, `nn.ReLU`). They contain parameters and a `forward()` method.
*   **Sequential:** `nn.Sequential` chains modules together for standard feedforward networks (MLPs).
*   **Optimizers:** Found in `torch.optim` (e.g., `SGD`). They update model parameters using computed gradients.
*   **Loss Functions:** Called "criterions" in PyTorch (e.g., `nn.MSELoss` for regression, `nn.CrossEntropyLoss` for classification).

## 3. Mini-Batch Gradient Descent
*   **DataLoaders:** `torch.utils.data.DataLoader` efficiently loads data in mini-batches, handles shuffling, and speeds up training via multiprocessing (`num_workers`) and memory pinning (`pin_memory=True`).
*   **Datasets:** `TensorDataset` wraps your data tensors into a compatible format.
*   **Modes:** Always call `model.train()` before training (enables dropout/batchnorm) and `model.eval()` before inference/evaluation.

## 4. Custom Architectures (Non-Sequential)
*   **Subclassing `nn.Module`:** Required for complex networks (like Wide & Deep models) that can't be strictly sequential.
*   **Implementation:** Define layers in `__init__()` and dictate the exact data flow in `forward(self, X)`.
*   **Multiple Inputs/Outputs:** Easily handled by passing multiple arguments to `forward()` and returning tuples (useful for auxiliary regularization losses).

## 5. Image Classification & TorchVision
*   **TorchVision:** Ecosystem library for computer vision datasets, pre-trained models, and image transforms (e.g., `v2.ToImage`, `v2.ToDtype`).
*   **Image Shapes:** PyTorch expects `[Channels, Height, Width]`, unlike Scikit-Learn or Matplotlib formats.
*   **Classification Loss:** `nn.CrossEntropyLoss` expects raw, unnormalized **logits**. Do *not* put a Softmax activation on the final output layer during training (apply `F.softmax()` manually only if you need to view probabilities during inference).

## 6. Tuning, Saving, and Compiling
*   **Hyperparameter Tuning:** Use dedicated libraries like **Optuna** (TPE algorithm). It handles dynamic search spaces and trial pruning (stopping bad runs early).
*   **Saving Models:** Never save the entire model object (pickle security risks and environment brittleness). Save only the weights using `torch.save(model.state_dict(), 'weights.pt')`.
*   **PyTorch 2.0 Compilation:** Use `torch.compile(model)` to enable Just-In-Time (JIT) compilation (TorchDynamo/TorchInductor), vastly speeding up execution.

---

## 7. Implementation Notes (Standard PyTorch Training Loop)

Initialize the dataset, model, loss criterion, and optimizer.
```python
# Create dataset and dataloader
dataset = TensorDataset(X_train, y_train)
loader = DataLoader(dataset, batch_size=32, shuffle=True)

# Initialize model, loss, and optimizer
model = nn.Sequential(nn.Linear(8, 50), nn.ReLU(), nn.Linear(50, 1)).to(device)
criterion = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
```

Iterate through epochs and batches to train the model.
```python
# Set to training mode
model.train() 

for epoch in range(20):
    for X_batch, y_batch in loader:
        X_batch, y_batch = X_batch.to(device), y_batch.to(device)
        
        # 1. Forward pass
        y_pred = model(X_batch)
        loss = criterion(y_pred, y_batch)
        
        # 2. Backward pass (compute gradients)
        loss.backward()
        
        # 3. Update weights
        optimizer.step()
        
        # 4. Reset gradients
        optimizer.zero_grad()
```

Evaluate the model without tracking gradients.
```python
# Set to evaluation mode
model.eval() 

with torch.no_grad():
    y_pred_logits = model(X_test_batch.to(device))
    
    # For classification: get class index with highest logit
    predictions = y_pred_logits.argmax(dim=1) 
```
