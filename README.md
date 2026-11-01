# Ex.No-02-LINEAR-AND-POLYNOMIAL-TREND-ESTIMATION
# Date: 01/11/2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

# Dataset Used : canteen_shop_data
# Software Required:
- Jupyter Notebook

### ALGORITHM:
- Import necessary libraries (NumPy, Matplotlib)
- Load the dataset
- Calculate the linear trend values using least square method
- Calculate the polynomial trend values using least square method
- End the program

### PROGRAM:
# Developed By: DIVYA PRIYA S
# Register No: 212225240037

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load dataset
data = pd.read_csv("canteen_shop_data.csv", parse_dates=['Date'])

print(data.head())

# Select Date and Employee ID
resampled_data = data.groupby('Date')['Employee ID'].mean().to_frame()

print(resampled_data.head())

# Reset index
resampled_data.reset_index(inplace=True)

print(resampled_data.head())

# Extract values
dates = resampled_data['Date']
employee_id = resampled_data['Employee ID'].tolist()

# Convert Date into numeric values
months = np.arange(len(resampled_data))

# Linear Trend Estimation
X = [i - months[len(months) // 2] for i in months]

x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, employee_id)]

n = len(X)

b = (n * sum(xy) - sum(employee_id) * sum(X)) / (
    n * sum(x2) - (sum(X) ** 2)
)

a = (sum(employee_id) - b * sum(X)) / n

linear_trend = [
    a + b * X[i]
    for i in range(n)
]

# Polynomial Trend Estimation (Degree 2)
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]

x2y = [i * j for i, j in zip(x2, employee_id)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [
    sum(employee_id),
    sum(xy),
    sum(x2y)
]

A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * (X[i] ** 2)
    for i in range(n)
]

# Display Trend Equations
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")

print(
    f"Polynomial Trend: y = {a_poly:.2f} + "
    f"{b_poly:.2f}x + {c_poly:.2f}x²"
)

# Add trend values
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

# Set Date as index
resampled_data.set_index('Date', inplace=True)

# Plot Linear Trend
plt.figure(figsize=(10, 5))

resampled_data['Employee ID'].plot(
    marker='o',
    label='Employee ID'
)

resampled_data['Linear Trend'].plot(
    linestyle='--',
    label='Linear Trend'
)

plt.title('Linear Trend Estimation')
plt.xlabel('Date')
plt.ylabel('Employee ID')
plt.legend()
plt.grid(True)
plt.show()

# Plot Polynomial Trend
plt.figure(figsize=(10, 5))

resampled_data['Employee ID'].plot(
    marker='o',
    label='Employee ID'
)

resampled_data['Polynomial Trend'].plot(
    linestyle='--',
    marker='o',
    label='Polynomial Trend'
)

plt.title('Polynomial Trend Estimation (Degree 2)')
plt.xlabel('Date')
plt.ylabel('Employee ID')
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT

<img width="989" height="670" alt="image" src="https://github.com/user-attachments/assets/ef1853ee-71a3-45cd-ac5a-ffb8762d9b35" />

- A - LINEAR TREND ESTIMATION:
<img width="1267" height="706" alt="image" src="https://github.com/user-attachments/assets/7f6e1c21-0057-4cef-b295-cc1d4cff68e6" />

- B- POLYNOMIAL TREND ESTIMATION:
<img width="1278" height="660" alt="image" src="https://github.com/user-attachments/assets/66a53b19-0b2e-4ed4-8ec6-c659e843947a" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
