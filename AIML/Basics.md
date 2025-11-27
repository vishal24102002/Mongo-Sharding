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
→ price = target (continuous), others = features
Pythonimport pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

df = pd.read_csv("house_prices.csv")
X = df[['size', 'bedrooms']]
y = df['price']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
model = LinearRegression().fit(X_train, y_train)
1.2 Logistic Regression (Binary Classification)
Required Data Format
csvstudy_hours,attendance,passed
5.5,90,1
2.1,60,0
8.0,95,1
...
→ Last column must be 0/1 (binary)
1.3 K-Nearest Neighbors (Iris Classification)
Built-in dataset → No preparation needed
But if using your own flower data:
csvsepal_length,sepal_width,petal_length,petal_width,species
5.1,3.5,1.4,0.2,setosa
7.0,3.2,4.7,1.4,versicolor
2. Complex Problems (Still Basics)
2.1 Diabetes Prediction
Built-in dataset → Already numerical and clean
Real-world equivalent format:
csvage,sex,bmi,bp,s1,s2,s3,s4,s5,s6,disease_progression
0.038,0.050,0.062,0.022,...,0.020,172
...
2.2 Titanic Survival (Decision Tree)
Required Columns (minimum):
csvPclass,Sex,Age,Fare,Survived
1,male,22,70.5,0
3,female,26,7.9,1
→ Survived = 0 or 1
→ Sex must be encoded: male=0, female=1
text---

### Updated File 2: `02_Intermediate.md`

```markdown
# AI/ML Notes - Part 2: Intermediate

## 1. Simple Intermediate Problems

### 1.1 Random Forest / XGBoost (Breast Cancer)

**Built-in** → Perfectly clean  
Your own medical dataset should look like:
```csv
mean_radius,mean_texture,mean_perimeter,...,worst_concavity,diagnosis
17.99,10.38,122.80,...,0.2654,malignant
→ diagnosis: malignant=1, benign=0
1.2 Simple Neural Network (MNIST / Digits)
For custom digit images:

Folder structure:

textdata/
  train/
    0/ → images of digit 0
    1/ → images of digit 1
    ...
  val/
    0/
    1/
Or use 64-pixel flattened vectors (like sklearn digits)
