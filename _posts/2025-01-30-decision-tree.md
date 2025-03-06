---
title: 'Simple Decision Trees and Its Implementation'
date: 2025-01-30
permalink: /posts/2025/03/decision-tree/
tags:
  - ML Basics
  - Learn with Code
  - AI explains AI
---

A **decision tree** is a machine learning model that makes decisions by recursively splitting data based on the best feature, forming a tree-like structure.

### **Why Use Decision Trees?**
✅ **Easy to understand** – Works like a flowchart.  
✅ **Handles numerical & categorical data** – No need for feature scaling.  
✅ **Feature importance** – Identifies the most important factors.  
✅ **Widely used** – In applications like fraud detection, medical diagnosis, and recommendation systems.

---
## **2. Understanding Gini Impurity**
The **Gini Impurity** measures how “impure” a node is, meaning how mixed the classes are. It is used to determine the best split at each step.

### **Mathematical Formula:**
For a dataset with **C** classes, Gini Impurity is calculated as:

$$ Gini = 1 - \sum_{i=1}^{C} p_i^2 $$

Where:
- \( p_i \) is the proportion of class **i** in the node.
- A Gini Impurity of **0** means the node is **pure** (all samples belong to one class).

---
## **3. Learn with Coding: Implementing Decision Tree from Scratch**

### **Step 1: Compute Gini Impurity**
```python
import numpy as np

def gini_impurity(labels):
    """
    Compute Gini Impurity for a given set of labels.
    
    Parameters:
    labels (array-like): List or array of class labels.
    
    Returns:
    float: Gini impurity score.
    """
    unique, counts = np.unique(labels, return_counts=True)
    probs = counts / counts.sum()
    return 1 - np.sum(probs ** 2)
```

---
### **Step 2: Find the Best Split**
```python
def best_split(X, y):
    """
    Identify the best feature and threshold for splitting the dataset.
    
    Parameters:
    X (ndarray): Feature matrix.
    y (ndarray): Target labels.
    
    Returns:
    tuple: Best feature index, best threshold value.
    """
    best_feature, best_threshold, best_impurity = None, None, float('inf')
    for feature in range(X.shape[1]):
        thresholds = np.unique(X[:, feature])
        for threshold in thresholds:
            left = y[X[:, feature] <= threshold]
            right = y[X[:, feature] > threshold]
            impurity = (len(left) * gini_impurity(left) + len(right) * gini_impurity(right)) / len(y)
            
            if impurity < best_impurity:
                best_feature, best_threshold, best_impurity = feature, threshold, impurity
    return best_feature, best_threshold
```

---
### **Step 3: Build the Decision Tree**
```python
class DecisionTree:
    """
    A simple decision tree classifier implementing recursive binary splits.
    """
    def __init__(self, max_depth=3):
        """
        Initialize the Decision Tree model.
        
        Parameters:
        max_depth (int): Maximum depth of the tree.
        """
        self.max_depth = max_depth
        self.tree = None

    def fit(self, X, y, depth=0):
        """
        Train the decision tree model recursively.
        
        Parameters:
        X (ndarray): Feature matrix.
        y (ndarray): Target labels.
        depth (int): Current depth of the tree.
        
        Returns:
        dict or int: Tree structure if splitting is required, else majority class.
        """
        if depth == self.max_depth or len(set(y)) == 1:
            return np.bincount(y).argmax()
        
        feature, threshold = best_split(X, y)
        left_idx = X[:, feature] <= threshold
        right_idx = X[:, feature] > threshold
        
        return {feature: {
            "<= {:.2f}".format(threshold): self.fit(X[left_idx], y[left_idx], depth + 1),
            "> {:.2f}".format(threshold): self.fit(X[right_idx], y[right_idx], depth + 1)
        }}
```

---
### **Step 4: Train and Test the Decision Tree**
```python
# Example Dataset (X: Features, y: Labels)
X = np.array([[2, 3], [1, 1], [3, 4], [5, 6], [4, 5]])
y = np.array([0, 0, 1, 1, 1])

tree = DecisionTree(max_depth=2)
tree.tree = tree.fit(X, y)
print(tree.tree)
```
This will train the decision tree and output its structure.

---
## **4. Key Takeaways**
- **Decision Trees** recursively split data to make decisions.
- **Gini Impurity** helps determine the best feature to split on.
- **Building from scratch strengthens understanding** before using libraries like Scikit-Learn.

---
🤖 *This post was AI-assisted but reviewed and refined by [Dr. Rebecca Li](https://xiaoyang-rebecca.github.io/), ensuring clarity and accuracy.*

