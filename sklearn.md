# **Scikit-learn (sklearn) Notes**

## **1. Introduction**
- **Scikit-learn** is a popular open-source machine learning library for Python.
- Built on NumPy, SciPy, and Matplotlib.
- Provides simple and efficient tools for data mining and data analysis.

## **2. Installation**
```bash
pip install scikit-learn
```
- Requires:
  - Python (>= 3.8)
  - NumPy (>= 1.17.3)
  - SciPy (>= 1.5.0)
  - joblib (>= 1.2.0)
  - threadpoolctl (>= 2.0.0)

## **3. Key Features**
- Supervised & unsupervised learning algorithms.
- Tools for model evaluation & selection.
- Data preprocessing & feature extraction.
- Integration with Pandas & NumPy.

---

## **4. Basic Workflow**
### **4.1 Load Dataset**
```python
from sklearn.datasets import load_iris
iris = load_iris()
X, y = iris.data, iris.target
```

### **4.2 Split Data (Train/Test)**
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

### **4.3 Choose & Train a Model**
```python
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)
```

### **4.4 Make Predictions**
```python
y_pred = model.predict(X_test)
```

### **4.5 Evaluate Model**
```python
from sklearn.metrics import accuracy_score
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy:.2f}")
```

---

## **5. Common Algorithms**
### **5.1 Supervised Learning**
| Algorithm | Import Statement |
|-----------|------------------|
| Linear Regression | `from sklearn.linear_model import LinearRegression` |
| Logistic Regression | `from sklearn.linear_model import LogisticRegression` |
| Decision Trees | `from sklearn.tree import DecisionTreeClassifier` |
| Random Forest | `from sklearn.ensemble import RandomForestClassifier` |
| SVM | `from sklearn.svm import SVC` |
| K-Nearest Neighbors (KNN) | `from sklearn.neighbors import KNeighborsClassifier` |

### **5.2 Unsupervised Learning**
| Algorithm | Import Statement |
|-----------|------------------|
| K-Means Clustering | `from sklearn.cluster import KMeans` |
| DBSCAN | `from sklearn.cluster import DBSCAN` |
| PCA (Dimensionality Reduction) | `from sklearn.decomposition import PCA` |

---

## **6. Preprocessing & Feature Engineering**
### **6.1 Scaling/Normalization**
```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

### **6.2 Encoding Categorical Data**
```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
encoder = LabelEncoder()
y_encoded = encoder.fit_transform(y)
```

### **6.3 Handling Missing Values**
```python
from sklearn.impute import SimpleImputer
imputer = SimpleImputer(strategy='mean')
X_imputed = imputer.fit_transform(X)
```

### **6.4 Feature Selection**
```python
from sklearn.feature_selection import SelectKBest, chi2
selector = SelectKBest(chi2, k=2)
X_new = selector.fit_transform(X, y)
```

---

## **7. Model Evaluation**
### **7.1 Classification Metrics**
```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    confusion_matrix,
    classification_report
)
```

### **7.2 Regression Metrics**
```python
from sklearn.metrics import (
    mean_squared_error,
    mean_absolute_error,
    r2_score
)
```

### **7.3 Cross-Validation**
```python
from sklearn.model_selection import cross_val_score
scores = cross_val_score(model, X, y, cv=5)  # 5-fold CV
```

---

## **8. Hyperparameter Tuning**
### **8.1 Grid Search**
```python
from sklearn.model_selection import GridSearchCV
param_grid = {'n_estimators': [50, 100, 200], 'max_depth': [None, 10, 20]}
grid_search = GridSearchCV(RandomForestClassifier(), param_grid, cv=5)
grid_search.fit(X_train, y_train)
best_params = grid_search.best_params_
```

### **8.2 Randomized Search**
```python
from sklearn.model_selection import RandomizedSearchCV
random_search = RandomizedSearchCV(RandomForestClassifier(), param_distributions=param_grid, n_iter=10, cv=5)
random_search.fit(X_train, y_train)
```

---

## **9. Pipelines**
- Combine preprocessing & modeling steps.
```python
from sklearn.pipeline import Pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('classifier', RandomForestClassifier())
])
pipeline.fit(X_train, y_train)
```

---

## **10. Saving & Loading Models**
```python
import joblib
# Save
joblib.dump(model, 'model.pkl')
# Load
loaded_model = joblib.load('model.pkl')
```

---



Great question! Here's a simple and clear breakdown on **how to train and split data** for a machine learning model.

---

## ✅ Steps to Train and Split Data (with Example)

### 🧾 1. Load Your CSV Data
```python
import pandas as pd

# Load your dataset
df = pd.read_csv('your_data.csv')
```

---

### 🧮 2. Split Features (X) and Target (y)

Assume `price` is what you want to predict:

```python
X = df.drop('price', axis=1)  # Features
y = df['price']               # Target/Label
```

---

### ✂️ 3. Split the Data into Training and Testing Sets

Use `train_test_split` from `sklearn`:

```python
from sklearn.model_selection import train_test_split

# 80% training, 20% testing
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

- `X_train`, `y_train`: used to train your model.
- `X_test`, `y_test`: used to evaluate how well your model performs on unseen data.
- `random_state`: for reproducibility (you’ll get the same split every time).

---

### 🧠 4. Train the Model

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)  # Train the model
```

---

### 📊 5. Evaluate the Model

```python
from sklearn.metrics import mean_squared_error

y_pred = model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)

print("Mean Squared Error:", mse)
```

---

### ✅ Summary
| Step        | Code Example                         |
|-------------|--------------------------------------|
| Load data   | `pd.read_csv('your_data.csv')`       |
| Split X/y   | `X = df.drop('target', axis=1)`      |
| Train/Test  | `train_test_split(X, y, test_size=...)` |
| Train model | `model.fit(X_train, y_train)`        |
| Predict     | `model.predict(X_test)`              |

---

If you want, I can generate a full working script using mock housing data or help you process your own dataset — just upload it or tell me its structure.


### **Summary**
- **Scikit-learn** provides a consistent API for ML tasks.
- Follows the **fit-predict-transform** pattern.
- Great for quick prototyping & production use.

