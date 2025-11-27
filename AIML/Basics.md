# AI/ML Notes - Part 1: Basics

Practical hands-on notes with code + **data format requirements**.

## 1. Simple Problems (Basics)

### 1.1 Linear Regression (Predict House Prices)

**Required Data Format**  
CSV or DataFrame with at least 2 columns:
```csv
size,bedrooms,price
1400,3,325000
1600,3,360000
...
```

→ price = target (continuous), others = features

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# Sample data
data = {
    'size': [650, 850, 1000, 1200, 1500, 1800],
    'price': [150000, 200000, 250000, 300000, 400000, 500000]
}
df = pd.DataFrame(data)

X = df[['size']]
y = df['price']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print(f"MSE: {mean_squared_error(y_test, y_pred):.2f}")
print(f"R²: {r2_score(y_test, y_pred):.2f}")
print(f"Price for 1300 sq.ft: ${model.predict([[1300]])[0]:,.2f}")
```

### 1.2 Logistic Regression (Binary Classification)
Required Data Format
csv study_hours,attendance,passed
5.5,90,1
2.1,60,0
8.0,95,1
...

→ Last column must be 0/1 (binary)

```python
from sklearn.datasets import make_classification
from sklearn.linear_model import LogisticRegression

X, y = make_classification(n_samples=1000, n_features=4, n_classes=2, random_state=42)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LogisticRegression()
model.fit(X_train, y_train)

accuracy = model.score(X_test, y_test)
print(f"Accuracy: {accuracy*100:.2f}%")
```

### 1.3 K-Nearest Neighbors (Iris Classification)
Built-in dataset → No preparation needed
But if using your own flower data:
csvsepal_length,sepal_width,petal_length,petal_width,species
5.1,3.5,1.4,0.2,setosa
7.0,3.2,4.7,1.4,versicolor

```python
from sklearn.datasets import load_iris
from sklearn.neighbors import KNeighborsClassifier

iris = load_iris()
X, y = iris.data, iris.target

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train, y_train)

print(f"Accuracy: {knn.score(X_test, y_test)*100:.2f}%")
print("Prediction:", iris.target_names[knn.predict([[5.1, 3.5, 1.4, 0.2]])[0]])
```

## 2. Complex Problems (Still Basics)

#### 2.1 Diabetes Prediction
Built-in dataset → Already numerical and clean
Real-world equivalent format:
csvage,sex,bmi,bp,s1,s2,s3,s4,s5,s6,disease_progression
0.038,0.050,0.062,0.022,...,0.020,172
...

```python
from sklearn.datasets import load_diabetes
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression

diabetes = load_diabetes()
X, y = diabetes.data, diabetes.target

# Always scale for better performance
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)

print(f"R² Score: {r2_score(y_test, model.predict(X_test)):.3f}")
```

#### 2.2 Titanic Survival (Decision Tree)
Required Columns (minimum):
csvPclass,Sex,Age,Fare,Survived
1,male,22,70.5,0
3,female,26,7.9,1
→ Survived = 0 or 1
→ Sex must be encoded: male=0, female=1
text---

```python
import pandas as pd
from sklearn.tree import DecisionTreeClassifier
from sklearn.preprocessing import LabelEncoder

# Load data
url = "https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv"
df = pd.read_csv(url)

# Simple feature engineering
df = df[['Pclass', 'Sex', 'Age', 'Fare', 'Survived']].dropna()
df['Sex'] = LabelEncoder().fit_transform(df['Sex'])

X = df.drop('Survived', axis=1)
y = df['Survived']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

dt = DecisionTreeClassifier(max_depth=5, random_state=42)
dt.fit(X_train, y_train)

print(f"Accuracy: {dt.score(X_test, y_test)*100:.2f}%")
```
