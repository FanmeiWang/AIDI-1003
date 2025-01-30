# Bagging Classifier on the Wine Dataset

This repository demonstrates how to build and tune a **Bagging Classifier** (using a Decision Tree as the base estimator) on the **Wine** dataset with **scikit-learn**. The project particularly focuses on:

- Employing Bagging (an ensemble learning technique) to reduce variance and improve robustness in classification tasks.  
- Using **GridSearchCV** to perform hyperparameter tuning (e.g., `n_estimators`, `max_samples`).  
- Applying cross-validation to evaluate and confirm the model's performance.  
- Visualizing the decision boundary in a 2D feature space to better understand the classification behavior.

## File Structure

- **`bagging_wine.py`**  
  - Loads and splits the Wine dataset  
  - Builds an initial Bagging model and evaluates performance  
  - Uses GridSearchCV for hyperparameter tuning  
  - Conducts cross-validation  
  - Plots the decision boundary for a 2D visualization  

## Main Steps

### 1. Load Data
- Uses `load_wine()` from `sklearn.datasets`.  
- Only the first two features (Alcohol, Malic Acid) are selected for 2D visualization.

### 2. Build a Bagging Model
- Employs `BaggingClassifier` with `DecisionTreeClassifier` as the `estimator`.  
- Evaluates initial performance on a test set.

### 3. Hyperparameter Tuning
- `GridSearchCV` searches over `n_estimators` (number of base learners) and `max_samples` (fraction of samples for each learner).
- Selects the best model and evaluates its performance on the test set again.

### 4. Cross-Validation
- Uses `cross_val_score` on the entire dataset (`X`, `y`) for multiple folds (default 5).
- Observes the model's mean accuracy across different splits.

### 5. Decision Boundary Visualization
- Creates a meshgrid and uses `predict` to render classification regions.
- Plots both training (circles) and testing (stars) points to illustrate how the model separates the three wine classes in a 2D space.

---

## Results and Reflection

- **Initial vs. Tuned Accuracy**: Hyperparameter tuning can significantly affect test-set performance, balancing the trade-off between runtime and accuracy gains.  
- **Cross-Validation**: If the cross-validation mean differs notably from the single test-set accuracy, consider data distribution, potential overfitting, or underfitting issues.  
- **Decision Boundary**: As this is a three-class problem and only two features are used, the model may produce “fragmented” classification regions, which is not uncommon for ensemble methods with multiple base learners.

---

## Requirements

- Python 3.7+  
- scikit-learn >= 1.2  
- matplotlib, numpy, pandas

---
