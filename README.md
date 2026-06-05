# ML-From-Scratch

Educational machine learning implementations written to make the algorithms visible.

This repository is a growing collection of machine learning models implemented from first principles. The goal is not to compete with NumPy, scikit-learn, PyTorch, or production ML libraries. The goal is to expose the mechanics: the data structures, objective functions, update rules, parameter choices, and failure modes that are usually hidden behind library calls.

The notebooks are used for demonstrations and comparisons. The package code is where the reusable implementations live.

## Current status

This repo is being refactored from notebook-only implementations into an importable educational Python package.

Current focus:

- `KMeans` clustering from scratch
- pure-Python distance and clustering logic
- notebook demonstrations using synthetic data
- comparisons against scikit-learn for sanity checks
- companion Obsidian notes for full mathematical explanations

Planned next improvement:

- add k-means++ centroid initialization

## Design principles

### Plain Python first

Core implementations should use plain Python data structures and explicit loops unless a dependency is intentionally being used for demonstration or comparison.

This makes the code slower than optimized numerical libraries, but easier to inspect:

```text
lists instead of hidden arrays
loops instead of vectorized black boxes
small functions instead of one large notebook cell
clear docstrings instead of terse production abstractions
```

### Add abstractions only when needed

This is not intended to become a complete replacement for NumPy, SciPy, or scikit-learn.

If an algorithm needs a helper function, metric, or data structure, add the smallest useful version at that point. Over time, the supporting library will grow naturally from the algorithms rather than from premature framework design.

Examples of reasonable future additions:

- vector and matrix type aliases
- distance metrics
- simple train/test splitting
- basic evaluation metrics
- small optimization helpers
- clustering initialization utilities

### Not production software

These implementations are for learning, teaching, and technical explanation.

Use mature libraries for real modeling work:

- scikit-learn
- NumPy
- SciPy
- PyTorch
- JAX

## Repository structure

Current / intended structure:

```text
ML-From-Scratch/
  README.md
  pyproject.toml
  model_notebooks/
    k-means.ipynb
  ml_from_scratch/
    clustering/
      kmeans.py
    metrics/
      distance.py
  tests/
    test_kmeans.py
```

The exact package layout is still being cleaned up as the notebook code is moved into modules.

## Implemented / in progress

### K-means clustering

Status: in progress

K-means partitions numeric data into `k` clusters by alternating between:

1. assigning each point to the nearest centroid;
2. updating each centroid to the mean of its assigned points.

The current notebook implementation includes:

- Euclidean distance
- random centroid initialization
- several initialization strategies
- label assignment
- centroid updates
- convergence checking by centroid movement
- inertia calculation
- repeated random initialization runs
- `fit` / `predict` style wrapper class
- comparison against scikit-learn on synthetic blob data

Planned additions:

- k-means++ initialization
- tests for edge cases
- cleaner package module
- notebook updated to import from the package

## Relationship between notebooks and package code

The intended split is:

```text
Package modules:
  reusable implementation code
  docstrings
  tests
  small educational abstractions

Notebooks:
  demonstrations
  visualizations
  experiments
  comparison with established libraries

Obsidian notes:
  full mathematical explanations
  derivations
  parameter breakdowns
  assumptions and failure modes
```

This keeps the repo useful both as code and as a learning artifact.

## Example direction

Eventually, usage should look something like:

```python
from ml_from_scratch.clustering import KMeans

X = [
    [0.0, 0.0],
    [0.1, 0.0],
    [10.0, 10.0],
    [10.1, 10.0],
]

model = KMeans(k=2, init_method="kmeans++")
model.fit(X)
labels = model.predict(X)
```

The implementation should remain readable enough that a learner can open the source file and understand what each step is doing.

## Testing strategy

As the repo is refactored, each algorithm should get tests for:

- basic expected behavior
- shape / dimensionality errors
- deterministic behavior when a random seed is supplied
- convergence on simple toy datasets
- edge cases such as empty clusters or invalid `k`
- comparison against known hand-computed results where possible

For k-means specifically, useful tests include:

- two obvious clusters converge to the expected centroids
- `predict` raises before `fit`
- invalid `k` raises a clear error
- mismatched vector dimensions raise a clear error
- k-means++ chooses `k` initial centers without duplicates when possible

## Why this project exists

Libraries make machine learning productive. From-scratch implementations make machine learning understandable.

This project is meant to sit in the second category: readable code, explicit algorithms, and enough mathematical explanation to connect the implementation back to the underlying model.

## License

See `LICENSE`.
