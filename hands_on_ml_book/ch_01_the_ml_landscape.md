# The Machine Learning Landscape

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9798341607972/files/assets/hmls_0120.png)

## Learning paradigm
**Supervised**: You train the model with data that already includes the correct answers (called "labels").

**Self-supervised:** A type of supervised learning where the label is generated from the data itself (e.g., you hide a word and ask the model to predict it).

**Semi-supervised:** You have a large amount of data, but only a small portion of it is labeled with the correct answers.

**Unsupervised:** You train the model with data that has no labels. The model's job is to find hidden patterns (like grouping or "clustering") on its own.

**Reinforcement:** A model (an "agent") learns by performing actions and getting "rewards" for good ones and "penalties" for bad ones.

## Task
**Regression:** Predicting a continuous number (e.g., the price of a house, 450,250.75).

**Classification:** Predicting a specific category (e.g., "spam" or "not spam").

**Clustering:** An unsupervised task of grouping similar data points together, even if you don't know what the groups are.

**Dim. reduction (Dimensionality reduction):** Simplifying data by reducing the number of features (columns), while trying to keep the most important information.

**Anomaly detection:** An unsupervised task of finding "weird" data points that don't fit in with the rest (e.g., detecting a fraudulent transaction).

**Novelty detection:** Similar to anomaly detection, but the model is trained only on normal data and its job is to spot anything new or different.

## Training
**Batch:** The model is trained all at once using the entire dataset. To update it, you must retrain it from scratch on all the data (old + new).

**Online:** The model is trained in small pieces ("mini-batches"). It can be continuously updated as new data arrives.

## Modeling
**Instance-based:** The model "learns" by simply memorizing all the training examples. It makes predictions by finding the most similar instance from its memory.

**Model-based:** The model "learns" by finding a pattern and building a general mathematical representation (a "model"). It uses this (e.g., a formula) to make predictions.

## Other
**Transfer:** Taking a model that was already trained on one big task (e.g., identifying 1000 different objects) and fine-tuning it for a new, similar task (e.g., identifying cat vs. dog).

**Ensemble:** Combining the predictions from multiple different models. The "group vote" is often more accurate than any single model's guess.

**Federated:** A training approach where the model is sent to the data (e.g., to your phone), learns locally without your private data ever leaving, and then sends just the model's improvements back to a central server.

**Meta:** A model that learns how to learn, or a model that learns from the output of other models.
