---
layout: post
title: Nonlinear Support Vector Machine (SVM) with Polynomial Kernel
date: 2025-12-20
description: Nonlinear support vector machine from scratch
tags: formatting images
categories: sample-posts
thumbnail: assets/img/svm.png
---

We implement a Nonlinear Support Vector Machine (SVM) from scratch in Python using Quadratic Programming (QP) with the help of the cvxopt optimization library.
The model uses a polynomial kernel to separate non-linearly separable data and is tested on a small dataset (t030.csv) [see github page].

The implementation includes:

- Training SVM with a polynomial kernel
- Identifying support vectors
- Plotting decision boundaries
- Train-test split evaluation
- Accuracy calculation

## Project

Support Vector Machines (SVMs) are powerful supervised learning models for classification tasks. While linear SVMs work on linearly separable data, this implementation extends SVM to nonlinear classification using a polynomial kernel. We solve the optimization problem using cvxopt quadratic programming solver.

```python
import numpy as np
import matplotlib.pyplot as plt
from cvxopt import matrix, solvers

def polynomial_kernel(x1, x2, degree=2):
    """Compute the polynomial kernel between two sets of samples."""
    return (1 + np.dot(x1, x2.T)) ** degree

def svm_nonlinear(traindata, trainclass, C):
    """Train a nonlinear SVM with a polynomial kernel."""

    trainclass = np.where(trainclass == 2, -1, 1)
    m = traindata.shape[0]

    # Compute the kernel matrix
    K = polynomial_kernel(traindata, traindata)

    # matrices for the QP problem
    P = matrix(np.outer(trainclass, trainclass) * K)
    q = matrix(-np.ones(m))
    G = matrix(-np.eye(m))
    h = matrix(np.zeros(m))
    A = matrix(trainclass, (1, m), 'd')
    b = matrix(0.0)

    # Solve the QP problem
    sol = solvers.qp(P, q, G, h, A, b)

    # Get the Lagrange multipliers
    alphas = np.ravel(sol['x'])

    # Identify support vectors
    sv_idx = alphas > 1e-5
    alphas = alphas[sv_idx]
    svs = traindata[sv_idx]
    sv_classes = trainclass[sv_idx]

    # Compute the bias term
    w0 = np.mean([sv_classes[i] - np.sum(alphas * sv_classes * polynomial_kernel(svs[i], svs)) for i in range(len(svs))])

    return svs, sv_classes, alphas, w0
```

# nice plotting function

````python
def plot_decision_boundary(traindata, trainclass, svs, sv_classes, alphas, w0):
    """Plot the decision boundary and the support vectors."""
    plt.figure(figsize=(10, 6))

    # Plot the training data
    plt.scatter(traindata[trainclass == 1][:, 0], traindata[trainclass == 1][:, 1], color='blue', label='Class 1', s=100)
    plt.scatter(traindata[trainclass == 2][:, 0], traindata[trainclass == 2][:, 1], color='red', label='Class 2', s=100)

    # Plot support vectors
    plt.scatter(svs[:, 0], svs[:, 1], facecolors='none', edgecolors='k', s=300, label='Support Vectors')

    # Create a grid to plot the decision boundary
    xlim = plt.xlim()
    ylim = plt.ylim()
    xx, yy = np.meshgrid(np.linspace(xlim[0], xlim[1], 100), np.linspace(ylim[0], ylim[1], 100))

    # Evaluate decision function on the grid
    grid = np.c_[xx.ravel(), yy.ravel()]
    K_grid = polynomial_kernel(grid, svs)
    decision_scores = np.dot(K_grid, alphas * sv_classes) + w0
    decision_scores = decision_scores.reshape(xx.shape)

    # Plot the decision boundary and margins
    plt.contour(xx, yy, decision_scores, levels=[0], linewidths=2, colors='k')
    plt.title('Nonlinear SVM with Polynomial Kernel')
    plt.xlabel('Feature 1')
    plt.ylabel('Feature 2')
    plt.legend()
    plt.grid()
    plt.show()
    ```
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/svm.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Clone the repository and install the required dependencies:

git clone https://github.com/OLIVIERKANAMUGIRE/Support-Vector-Machines-from-Scratch.git
````
